# npu\_conv2d和npu\_conv3d算子在1.8和1.11及以上版本上ONNX导出报错

## 问题描述

npu\_conv2d和npu\_conv3d算子在1.8、1.11及以上版本上ONNX导出，出现报错信息“\_convolution\(\) missing 1 required positional argument: 'allow\_tf32' ”。

## 处理方法

该问题属于PyTorch框架bug。

可根据路径信息对torch/onnx/symbolic\_opset9.py进行修改，在第1293行修改参数设置**allow\_tf32=None。**

```python
def _convolution(g, input, weight, bias, stride, padding, dilation,
                 transposed, output_padding, groups, benchmark, deterministic, cudnn_enabled, allow_tf32=None):
```

可参考[https://github.com/pytorch/pytorch/issues/75098](https://github.com/pytorch/pytorch/issues/75098)和[https://github.com/pytorch/pytorch/pull/75099](https://github.com/pytorch/pytorch/pull/75099)。
