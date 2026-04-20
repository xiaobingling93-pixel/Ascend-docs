# torch\_npu在单进程中使用多张NPU卡

在PyTorch2.1.0及以上版本中，使用torch\_npu可以在一个进程中使用多个device卡，根据指定的device id将任务下发到期望的卡上执行。通过单进程多卡的支撑，在torch\_npu上可以执行更灵活的设备操作。

> [!NOTICE]
>
> - 用户使用Ascend Extension for PyTorch  6.0.rc3版本配套的驱动（Ascend HDK 24.1.RC3），开启虚拟内存特性时，可以使用单进程多卡特性；用户使用Ascend Extension for PyTorch  6.0.rc3之前版本配套的驱动（Ascend HDK 24.1.RC3之前版本），开启虚拟内存特性时，不能使用单进程多卡特性。配置如下环境变量开启虚拟内存特性：
>
>   ```bash
>   export PYTORCH_NPU_ALLOC_CONF=expandable_segments:True
>   ```
>
> - 由于<term>Atlas 训练系列产品</term>/<term>Atlas A2 训练系列产品</term>硬件限制，卡间拷贝会有上限，当前最大值为8，超过则会导致异常报错。例如：从0卡拷贝数据到 7\~15卡（9张卡）时，最后一个底层建链会失败，导致拷贝异常。<term>Atlas A3 推理系列产品</term>无此限制。

torch\_npu在单进程中使用多张NPU卡具体示例如下：

```python
import torch
import torch_npu

# 默认device可以设置也可以不设置
torch.npu.set_device(0)

# 使用device 0
tensor_a = torch.randn(2,3, device="npu:0")
# 使用device 1
tensor_b = torch.randn(2,3, device="npu:1")
# 跨卡的拷贝
tensor_c = tensor_a.to("npu:1")
# 在device 1上进行计算
tensor_d = tensor_b + tensor_c
```
