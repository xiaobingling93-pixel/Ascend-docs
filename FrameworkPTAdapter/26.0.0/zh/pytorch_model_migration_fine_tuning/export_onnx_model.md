# 导出ONNX模型

模型训练完成后，用户可以使用pth文件和pth.tar文件导出ONNX模型，然后通过ATC工具将其转换为适配昇腾AI处理器的.om文件用于离线推理。将ONNX模型转换为适配昇腾AI处理器的.om文件流程请参考《[CANN ATC离线模型编译工具用户指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/devaids/atctool/atlasatc_16_0001.html)》。离线推理应用构建请参考《[CANN 应用开发指南 \(C&C++\)](https://www.hiascend.com/document/detail/zh/canncommercial/850/appdevg/acldevg/aclcppdevg_000000.html)》。

## 简介

ONNX是业内目前比较主流的模型格式，广泛用于模型交流及部署。PyTorch模型在昇腾AI处理器上的部署策略是基于PyTorch官方支持的ONNX模块实现的。

本节主要介绍如何将Checkpoint文件通过torch.onnx.export()接口导出为ONNX模型。

> [!NOTICE]
>
> 用PyTorch框架导出ONNX模型时，框架中ACL\_OP\_SELECT\_IMPL\_MODE选项默认值为“high\_precision“，ACL\_OP\_SELECT\_IMPL\_MODE详情可参见《CANN  应用开发接口》的“[aclCompileOpt](https://www.hiascend.com/document/detail/zh/canncommercial/850/API/appdevgapi/aclcppdevg_03_1371.html)”章节。用户在使用导出的ONNX模型进行模型转换时，可参见《CANN ATC离线模型编译工具用户指南》中的“[--op\_select\_implmode](https://www.hiascend.com/document/detail/zh/canncommercial/850/devaids/atctool/atlasatcparam_16_0072.html)”章节设置与训练时相同的模式，以避免因模式选择不同而出现的精度或者性能差异。
> 导出ONNX模型前，需执行如下命令安装ONNX。
>
> ```shell
> pip3 install onnx
> ```

## .pth或.pt文件导出ONNX模型

保存的.pth或.pt文件可以通过PyTorch构建模型，再加载权重的方法恢复，然后导出ONNX模型，样例如下：

```python
import torch
import torch.nn as nn
import torch.onnx
import torch_npu
from torch_npu.contrib import transfer_to_npu

device = torch.device("cuda")


class ToyModel(nn.Module):
    def __init__(self):
        super(ToyModel, self).__init__()
        self.weight = nn.Parameter(torch.randn(20, 10))
        self.net1 = nn.Linear(10, 10)
        self.relu = nn.ReLU()
        self.net2 = nn.Linear(10, 5)

    def forward(self, x):
        return self.net2(self.relu(self.net1(x)))

def convert():
    model = ToyModel()  
    model.load_state_dict(torch.load('state_dict_model.pt', map_location='cuda'),strict=False)    #根据实际文件名称修改

    # 模型设置为推理模式
    model.eval()

    dummy_input = torch.randn(20, 10) #  定义输入shape
    torch.onnx.export(model, 
                      dummy_input, 
                      "model.onnx", 
                      input_names = ["input"],   # 构造输入名
                    output_names = ["output"],    # 构造输出名
                      opset_version=11,    # ATC工具目前支持opset_version=9，10，11，12，13
                      dynamic_axes={"input":{0:"batch_size"}, "output":{0:"batch_size"}})  #支持输出动态轴

if __name__ == "__main__":
    convert()
```

> [!NOTE]
>
> - 在导出ONNX模型之前，必须调用`model.eval()`将dropout和batch normalization层设置为推理模式。
> - 样例脚本中的model来自于torchvision模块中的定义，用户使用时需自行指定。
> - 构造输入输出需要对应训练时的输入输出，否则无法正常推理。

## .pth.tar文件导出ONNX模型

.pth.tar在导出ONNX模型时需要先确定保存时的信息，有时保存的节点名称和模型定义中的节点会有差异，例如会多出前缀和后缀。在进行转换的时候，可以对节点名称进行修改。转换代码样例如下：

```python
import torch
import torch.nn as nn
import torch.onnx
import torch_npu
from torch_npu.contrib import transfer_to_npu
from collections import OrderedDict

device = torch.device("cuda")

class ToyModel(nn.Module):
    def __init__(self):
        super(ToyModel, self).__init__()
        self.weight = nn.Parameter(torch.randn(20, 10))
        self.net1 = nn.Linear(10, 10)
        self.relu = nn.ReLU()
        self.net2 = nn.Linear(10, 5)

    def forward(self, x):
        return self.net2(self.relu(self.net1(x)))

def proc_nodes_module(checkpoint, AttrName):
    new_state_dict = OrderedDict()
    for key, value in checkpoint[AttrName].items():
        if key == "module.features.0.0.weight":
            print(value)
        #根据实际前缀后缀情况修改
        if(key[0:7] == "module."):
            name = key[7:]
        else:
            name = key[0:]

        new_state_dict[name] = value
    return new_state_dict


def convert():
    model = ToyModel()
    checkpoint = torch.load('checkpoint.pth.tar', map_location='cuda')  # 根据实际文件名称修改
    checkpoint['state_dict'] = proc_nodes_module(checkpoint, 'state_dict')
    model.load_state_dict(checkpoint['state_dict'], strict=False)

    # 模型设置为推理模式
    model.eval()

    dummy_input = torch.randn(20, 10) #  定义输入shape
    torch.onnx.export(model,
                      dummy_input,
                      "model.onnx",  # 输出文件名根据实际情况修改
                      input_names = ["input"],   # 构造输入名
                      output_names = ["output"],    # 构造输出名
                      opset_version=11,    # ATC工具目前支持opset_version=9，10，11，12，13
                      dynamic_axes={"input":{0:"batch_size"}, "output":{0:"batch_size"}})  #支持输出动态轴

if __name__ == "__main__":
    convert()
```

## 自定义算子导出ONNX模型

对于非NPU自定义算子，导出ONNX的逻辑和限制遵循PyTorch框架，请参考[官方网站](https://pytorch.org/docs/stable/onnx.html#limitations)的PyTorch框架issue或者文档进行修改。

对于使用torch\_npu自定义算子的模型，需要在导出脚本中加入对导出自定义算子的使能import torch_npu.onnx。在使用torch.onnx.export方式导出时，自定义算子在继承原生框架ONNX导出限制的基础上，还存在以下限制：

- 仅支持使用torch\_npu方式调用，如torch_npu.fast_gelu(x)，不能使用torch.fast_gelu(x)。
- 对于inplace和out类算子，在实际推理过程中并不会使用这类算子，如果使用的话会导致断图，请使用对应算子代替。示例如下：

    torch_npu.npu_silu_(input)修改为input = torch_npu.npu_silu(input)

    torch_npu.npu_broadcast(tensor, size, out=result)修改为result = torch_npu.npu_broadcast(tensor, size)

- 仅支持部分自定义算子的导出，支持清单参见[自定义算子导出ONNX支持清单](custom_operators_onnx_export_support_list.md)。
- 对于存在定制化正反向流程的模型（比如继承自torch.autograd.Function），例如：

    ```python
    class MyFunction(torch.autograd.Function):
        @staticmethod
        def forward(ctx, tensor1, pyscalar, tensor2):
            result = ...
            return result
        @staticmethod
        def backward(ctx, grad_output):
            result = ...
            return result
    ```

    由于ONNX导出原理的限制，这类模型需要修改实现逻辑才能导出ONNX模型，有两种解决方式：

    - 不能继承torch.autograd.Function，修改其实现逻辑。
    - 自定义onnx导出符号逻辑（symbolic函数），对应插件等也需要自行定义。

- 由于原生PyTorch框架bug，npu\_conv2d和npu\_conv3d算子ONNX导出时会出现如下报错信息：

    TypeError: _convolution() missing 1 required positional argument: 'allow_tf32'

    请单击[npu\_conv2d和npu\_conv3d算子在1.8和1.11及以上版本上ONNX导出报错](npu_conv2d_3d_onnx_export_err_v1.8_v1.11p.md)查看解决方法。

自定义算子导出ONNX模型使用样例如下：

```python
import torch
import torch_npu
import torch_npu.onnx   # 自定义算子导出功能使能，仅在onnx导出脚本中使用，其他场景如训练使能可能导致错误

#定义一个简单的模型，使用NPU自定义算子
class Model(torch.nn.Module):
    def __init__(self):        
        super(Model, self).__init__()     

    def forward(self, x):         
        x = torch_npu.npu_one_hot(x, depth=5)    #使用NPU自定义算子     
        return x

inputs = torch.IntTensor([5, 3, 2, 1]).npu()  #模型的样例输入，一般随机值即可
model = Model().to("npu")  # 得到模型结构，将模型加载到device侧，需要保证模型中自定义算子使用方式已经满足前述要求
model.eval() # 设置为推理模式，在推理模式下BatchNorm层、Dropout层等用于优化训练而添加的网络层会被关闭，从而使得推理时不会发生偏移
model(inputs) # 验证模型正常运行
onnx_model_name = "npu_model.onnx"  # 导出的onnx模型名称
with torch.no_grad(): 
     torch_npu.onnx.export(model, inputs, onnx_model_name)  # 导出onnx模型
```
