# 特征值检测

## 简介

特征值检测是针对NPU的PyTorch API，作用是检测在训练过程中激活值的梯度特征值是否存在异常。当前仅能识别数据类型为**BF16或FP32**的模型在训练过程中出现的收敛异常。

> [!NOTE]
>
> - PyTorch图模式（TorchAir）不支持此特性。
> - Ascend Extension for PyTorch 7.0.0及之前，可参考Ascend Extension for PyTorch相应版本的资料进行操作。
> - 由于此特性检测对象为梯度，无梯度场景（如推理、强化学习的推理部分）不支持使用。
> - 特征值检测需要计算激活值梯度的统计值，会产生额外的显存占用，最多可能存在1.5G的额外显存消耗，用户显存紧张情况下可能导致OOM（Out of Memory，内存不足）。

特征值检测具体介绍可参见《PyTorch 框架特性指南》中的“[特征值检测](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-7.3.0/docs/zh/framework_feature_guide_pytorch/feature_value_detection.md)”章节。

## 开启检测开关

1. 训练前，配置如下环境变量，开启特征值检测和checksum联动功能：

    > [!CAUTION]
    > 
    > 开启特征值检测会导致性能损失，建议在满足如下所有条件时开启，否则性能损失可能大于2%。
    > - 模型规模：70B及以上；
    > - 集群规模：64卡及以上；
    > - torch.npu.set\_compile\_mode设置：jit\_compile=False。

    ```shell
    export NPU_ASD_CONFIG=enable:true,with_checksum:true         
    # 开启特征值检测和Checksum联动功能
    ```

    更多参数可参考《PyTorch 框架特性指南》中的“[特征值检测](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-7.3.0/docs/zh/framework_feature_guide_pytorch/feature_value_detection.md)”章节。

## 故障处理

对于检测过程中可能会产生的三种异常场景说明如下：

1. 梯度异常：单次梯度异常时，会打印“A grad-norm spike may happen”Event级别的训练打印日志，日志等级为INFO，默认不开启，可以通过设置“export TORCH\_NPU\_LOGS=silent”开启。训练过程中梯度单次跳变，如果后续不产生特征值异常告警，无需进行处理。
2. 特征值异常：特征值异常时，会打印“feature detection detects abnormal results”Warning级别的训练打印日志，训练过程中多次发生梯度跳变，建议通过checksum联动进一步确认是否为aicore错误的硬件故障。
3. checksum校验异常：checksum校验异常时，会打印“The result of Matmul checksum is abnormal”Warning级别的训练打印日志，确认报错卡产生了aicore错误的硬件故障，请联系华为工程师维修更换。
