# 适配简介

混合精度训练是在训练时混合使用单精度（float32）与半精度\(float16\)数据类型，使用相同的超参数训练，能达到与float32几乎相同的精度。

混合精度适配支持两种方式：

- PyTorch框架内置的AMP功能模块
- 第三方APEX混合精度模块

推荐使用PyTorch框架内置的AMP功能模块来进行混合精度训练，本节将介绍其适配方法，AMP混合精度具体原理与计算过程请参考[混合精度原理与计算过程介绍](mixed_prec_intro.md)。

如果用户想使用APEX模块，可参考[APEX](https://gitcode.com/Ascend/apex)。

## 使用约束

当前混合精度适配约束如下：

- 混合精度适配操作在脚本迁移完成后，训练开始之前。
- 若用户使用<term>Atlas 训练系列产品</term>，则在迁移完成、训练开始之前，由于其架构特性需要，用户需要开启混合精度。若用户使用<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>，则可以自行选择是否开启混合精度。
- 混合精度开启可能会提升性能，但不针对所有模型生效。如果性能没有提升，则需要做进一步性能调优修改。
- 在开启混合精度训练后，由于数值表示范围和最小间隔发生变化，可能导致极少部分网络出现精度掉点甚至无法收敛的情况，可以参考[混合精度问题调测](mixed_prec_debug.md)进行解决。

## 接口说明

混合精度AMP适配主要包括混合精度[amp.autocast\(\)](https://docs.pytorch.org/docs/2.1/amp.html#torch.autocast)接口与损失缩放[amp.GradScaler\(\)](https://docs.pytorch.org/docs/2.1/amp.html#torch.cuda.amp.GradScaler)接口的适配。

对于AMP模块，适配训练脚本后，NPU上可以支持以下4种AMP场景。具体场景说明可以单击对应链接，跳转至PyTorch官网查看。

- [典型场景](https://docs.pytorch.org/docs/2.1/notes/amp_examples.html#typical-mixed-precision-training)：开启混合精度进行一般模型训练的场景。
- [梯度累加场景](https://docs.pytorch.org/docs/2.1/notes/amp_examples.html#working-with-scaled-gradients)：每计算一个batch的梯度，进行梯度的累加而不是清零，当累加到一定次数再更新参数、清零梯度的场景。
- [多模型、损失函数和优化器场景](https://docs.pytorch.org/docs/2.1/notes/amp_examples.html#working-with-multiple-models-losses-and-optimizers)：神经网络中同时存在多个损失函数和优化器的场景。
- [DDP单NPU单进程场景](https://docs.pytorch.org/docs/2.1/notes/amp_examples.html#distributeddataparallel-one-gpu-per-process)：在分布式训练中，一个进程在一个NPU上运行的场景。

本节先给出了AMP适配接口（amp.GradScaler\(\)）说明，后续章节给出了AMP的NPU适配示例代码，以此来介绍混合精度AMP适配方法。

对于amp.GradScaler\(\)接口，原生PyTorch默认使用动态损失缩放（Loss Scale），Loss Scale取值会根据训练情况进行动态调整。当前NPU上适配的amp.GradScaler接口增加了dynamic参数。当dynamic参数设置为False时，支持使用固定值的Loss Scale，并且可以通过init\_scale参数设置Loss Scale取值，如下所示：

```python
scaler = amp.GradScaler(init_scale = 2.**10, dynamic = False)
```

**表 1**  参数说明

|参数名称|参数说明|参数取值|
|--|--|--|
|dynamic|AMP是否使用动态Loss Scale。|True（默认）：使用动态Loss Scale。False：使用静态Loss Scale。|
|init_scale|在使用静态Loss Scale时的初始scale系数。|仅在dynamic=False时可用，用户根据实际情况设置。|
