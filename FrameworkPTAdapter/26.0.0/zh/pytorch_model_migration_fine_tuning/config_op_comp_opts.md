# 设置算子编译选项

可以通过torch\_npu中的set\_option接口配置算子编译过程中的属性，通过ACL接口实现。

```python
import torch 
import torch_npu 
option = {key: val} 
torch_npu.npu.set_option(option) # 以dict方式进行设置
```

使用样例如下，以配置ACL\_PRECISION\_MODE为例。

```python
import torch 
import torch_npu 
option = {"ACL_PRECISION_MODE":"must_keep_origin_dtype"} 
torch_npu.npu.set_option(option)
```

**表 1**  key值和其对应val值的含义与说明

|可选值|含义|可设置的val值|说明|
|--|--|--|--|
|ACL_PRECISION_MODE|选择算子精度模式。|&#8226; **allow_fp32_to_fp16**：对于矩阵类算子，使用float16；对于矢量类算子，优先保持原图精度。如果网络模型中算子支持float32，则保留原始精度float32，如果网络模型中算子不支持float32，则直接降低精度到float16。<br>&#8226; **must_keep_origin_dtype**：保持原图精度。如果原图中某算子精度为float16，但NPU中该算子实现不支持float16、仅支持float32，则系统内部自动采用高精度float32；如果原图中某算子精度为float32，但NPU中该算子实现不支持float32、仅支持float16，此场景下不能使用此参数值，系统不支持使用低精度。|如果不配置该编译选项，则<br>&#8226; <term>Atlas 训练系列产品</term>默认采用allow_fp32_to_fp16。<br>&#8226; <term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>默认采用must_keep_origin_dtype。|
|ACL_OPTYPELIST_FOR_IMPLMODE|列举算子类型列表。|当前仅支持配置的算子为Pooling、SoftmaxV2、LRN、ROIAlign。|该参数当前仅支持设置某个具体算子的实现方式，不支持设置多个算子。|
|MM_BMM_ND_ENABLE|开启MatMul、BatchMatMul算子支持ND模式。|&#8226; enable：MatMul、BatchMatMul算子输入输出Shape每个维度都是16的整数倍时，会支持ND格式输入，不满足则使用算子内部私有格式并视情况结合插入转换算子。<br>&#8226; disable：MatMul、BatchMatMul算子使用私有格式。|默认采用enable模式。|
|ACL_OP_DEBUG_OPTION|算子内存检测开关。|**oom**：用于定位算子执行过程中检测Global Memory是否内存越界，越界算子会直接报错。|维测能力，默认不配置。|
