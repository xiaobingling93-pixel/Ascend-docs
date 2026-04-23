# 混合精度计算分析

尽管低精度计算能够提供非常显著的训练速度提升，但低精度运算相比FP32也会引起数值错误和数值稳定性问题。所以使用低精度运算来加速训练的时候，理解张量运算核心的数值行为也非常重要。

FP16的取值范围为±65504，能表示的绝对值最小的规格数为2<sup>-14</sup>和-2<sup>-14</sup>，即为+/-0.000061035，相邻两个浮点数的相对差距约为千分之一。举例来说，比1大的第一个浮点数为1+1/1024，这意味着没法表示1.0001。使用FP16相对FP32会有更大精度误差。下面的代码在主流AI处理器上分别用FP32和FP16计算一个10000维随机张量（中位数0，方差1的正态分布）均值的相对误差。用FP16计算时，平均相对误差在0.1%，最大误差在23%。在大模型训练场景下，因为很多张量都很小，且维度高，所以需留心FP16的精度问题。同样的代码如果使用BF16计算，相对误差均值在1%，最大误差接近4800%。因为累加运算有不确定性，程序运行两次会有不同的结果。

```shell
python non-distributed.py --dtype fp16 --device npu
python non-distributed.py --dtype bf16 --device npu
```

```python
import torch
import argparse
parser = argparse.ArgumentParser(
                    prog='test_mean',
                    description='check the effect to final result under different floating point precision')
parser.add_argument("-t", "--dtype", default="fp16",help="precision selection")
parser.add_argument("-d", "--device", default="cuda", help="accelerator selection")
args = parser.parse_args()
# accelerator selection
if args.device == 'cuda':
    device = torch.device('cuda:0')
elif args.device == 'npu':
    import torch_npu
    device = torch.device('npu:0')
dtype = torch.bfloat16 if args.dtype == "bf16" else torch.float16
fn = torch.mean

def test(fn):
    N, batch = 10000, 1000
    # calculations based on FP16 or BF16
    x = torch.randn((batch, N))
    x_16 = x.to(device, dtype=dtype)
    x_mean_16 = fn(x_16, dim=-1, dtype=dtype)
    
    # calculations based on FP32
    x_fp32 = x.to(device)
    x_mean_fp32 = fn(x_fp32, dim=-1, dtype=torch.float32)

    # error analysis 
    rel_diff = abs((x_mean_16.float() - x_mean_fp32) / x_mean_fp32)
    return rel_diff
rel_diff= test(fn=fn)
max_rel_diff = rel_diff.max() # maximum calculation error
avg_rel_diff = rel_diff.mean() # average calculation error
print("average relative difference is: ", avg_rel_diff)
print("max relative difference is: ", max_rel_diff)
```

因为FP16浮点表示范围很小，所以很容易发生上溢，运算结果产生Inf/NaN。某些硬件会将Inf转成最大可表示数，一定程度缓解上溢，但仍然会带来计算误差。

BF16通过增加指数位宽，降低尾数位宽获得更大的浮点数可表示范围，同时也牺牲了可表示精度。它的取值范围为±9.2E38，能表示的绝对值最小的规格数是±2<sup>-126</sup>，约为1.18\*10<sup>-38</sup>，相邻两个浮点数的相对差距在百分之一。举例来说，比1大的第一个浮点数为1+1/128，这意味着没法表示1.001。BF16的精度更差，但不易发生上溢下溢，在精度不敏感的运算场景比FP16更方便。现有的大型transformer模型实验证明，使用BF16进行混合精度训练不太影响模型的收敛。

分布式通信算子有累加操作时，也会有低精度数的误差累积问题，如下代码所示，主流AI处理器8卡的FP16 all\_reduce均值误差大约在千分之一，最大误差约为2倍。而用BF16时，均值相对误差大约在1.5%，最大相对误差有9倍。因为通信算子的确定性计算问题，同样的输入运行两次都会有不同的结果。

```shell
torchrun --nnodes=1 --nproc-per-node=8 distributed.py --dtype fp16 --device npu
torchrun --nnodes=1 --nproc-per-node=8 distributed.py --dtype bf16 --device npu
```

```python
import os
import torch
import torch.distributed as dist

import argparse
parser = argparse.ArgumentParser(
                    prog='fp precision test',
                    description='check the effect to final result under different floating point precision')
parser.add_argument("-t", "--dtype", default="fp16")
parser.add_argument("-d", "--device", default="cuda", help='accelerator selection')
args = parser.parse_args()

dtype = torch.bfloat16 if args.dtype == "bf16" else torch.float16
# distributed environment
local_rank = int(os.environ['LOCAL_RANK'])
world_size = int(os.environ['WORLD_SIZE'])
# accelerator selection
if args.device == 'cuda':
    device = torch.device(f"cuda:{local_rank}")
    dist.init_process_group(backend='nccl', rank=local_rank, world_size=world_size)
elif args.device == 'npu':
    import torch_npu
    device = torch.device(f"npu:{local_rank}")
    torch.npu.set_device(device)
    dist.init_process_group(backend='hccl', rank=local_rank, world_size=world_size)
if __name__ == "__main__":
    print("start all reduce!")
    hccl_input = torch.randn(10000)
    print(local_rank, hccl_input.shape)

    # collective communication 
    hccl_input16 = hccl_input.to(device=device,dtype=dtype)
    hccl_input32 = hccl_input.to(device=device)
    torch.distributed.all_reduce(hccl_input16)
    torch.distributed.all_reduce(hccl_input32)
    if local_rank == 0:
        rel_diff = abs((hccl_input16.float() - hccl_input32) / hccl_input32)
        diff_mean = torch.mean(rel_diff)
        diff_max = torch.max(rel_diff)
        print("diff_mean=", diff_mean)
        print("max diff=", diff_max)
```

总结而言，选择何种精度进行混合精度训练需依据具体任务需求权衡精度损失与训练速度提升，同时考虑分布式环境下的误差累积效应。
