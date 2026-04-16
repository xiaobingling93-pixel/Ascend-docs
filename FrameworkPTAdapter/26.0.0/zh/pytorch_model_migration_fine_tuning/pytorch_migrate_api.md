# 常见PyTorch迁移替换接口

用户需要替换原生PyTorch框架的接口，才能使用昇腾PyTorch框架。在进行网络迁移时，用户需要将部分接口转换成适配昇腾AI处理器后的接口。当前适配的部分接口请参见表1，更多接口请参见《[Ascend Extension for PyTorch 自定义API参考](https://gitcode.com/Ascend/op-plugin/blob/26.0.0/docs/zh/custom_APIs/overview.md)》。

**表 1**  设备接口替换表

|PyTorch原始接口|适配昇腾AI处理器后的接口|说明|
|--|--|--|
|torch.cuda.is_available()|torch_npu.npu.is_available()|判断当前环境中是否存在可用的NPU设备（返回True表示至少有一块可用的NPU）。|
|torch.cuda.current_device()|torch_npu.npu.current_device()|获取当前正在使用的device。|
|torch.cuda.device_count()|torch_npu.npu.device_count()|获取当前环境上的设备数量。|
|torch.cuda.set_device()|torch_npu.npu.set_device()|设置当前正在使用的device。|
|torch.tensor([1,2,3]).is_cuda|torch.tensor([1,2,3]).is_npu|判断某个tensor是否是CUDA/NPU设备上的格式。|
|torch.tensor([1,2,3]).cuda()|torch.tensor([1,2,3]).npu()|将某个tensor转换成CUDA/NPU格式。|
|torch.tensor([1,2,3]).to("cuda")|torch.tensor([1,2,3]).to('npu')|将某个tensor转换成CUDA/NPU格式。|
|torch.cuda.synchronize()|torch_npu.npu.synchronize()|同步等待事件完成。|
|torch.cuda.device|torch_npu.npu.device|生成一个device类，可以执行device相关操作。|
|torch.cuda.Stream(device)|torch_npu.npu.Stream(device)|生成一个stream对象。|
|torch.cuda.stream(Stream)|torch_npu.npu.stream(Stream)|多用于作用域限定。|
|torch.cuda.current_stream()|torch_npu.npu.current_stream()|获取当前stream。|
|torch.cuda.default_stream()|torch_npu.npu.default_stream()|获取默认stream。|
|device = torch.device("cuda:0")|device = torch.device("npu:0")|指定一个设备。|
|torch.autograd.profiler.profile(use_cuda=True)|torch.autograd.profiler.profile(use_npu=True)|指定执行profiler过程中使用CUDA/NPU。|
|torch.cuda.Event()|torch_npu.npu.Event()|返回某个设备上的事件。|

用户在构建网络或进行网络迁移时，需要创建指定数据类型的tensor。在昇腾AI处理器上创建的部分tensor如下，更多接口请参见《[Ascend Extension for PyTorch 自定义API参考](https://gitcode.com/Ascend/op-plugin/blob/26.0.0/docs/zh/custom_APIs/overview.md)》。

**表 2**  tensor创建接口替换

|GPU tensor|适配昇腾AI处理器后的接口|
|--|--|
|torch.tensor([1,2,3],dtype=torch.long,device='cuda')|torch.tensor([1,2,3],dtype=torch.long,device='npu')|
|torch.tensor([1,2,3],dtype=torch.int,device='cuda')|torch.tensor([1,2,3],dtype=torch.int,device='npu')|
|torch.tensor([1,2,3],dtype=torch.half,device='cuda')|torch.tensor([1,2,3],dtype=torch.half,device='npu')|
|torch.tensor([1,2,3],dtype=torch.float,device='cuda')|torch.tensor([1,2,3],dtype=torch.float,device='npu')|
|torch.tensor([1,2,3],dtype=torch.bool,device='cuda')|torch.tensor([1,2,3],dtype=torch.bool,device='npu')|
|torch.cuda.BoolTensor([1,2,3])|torch.npu.BoolTensor([1,2,3])|
|torch.cuda.FloatTensor([1,2,3])|torch.npu.FloatTensor([1,2,3])|
|torch.cuda.IntTensor([1,2,3])|torch.npu.IntTensor([1,2,3])|
|torch.cuda.LongTensor([1,2,3])|torch.npu.LongTensor([1,2,3])|
|torch.cuda.HalfTensor([1,2,3])|torch.npu.HalfTensor([1,2,3])|
