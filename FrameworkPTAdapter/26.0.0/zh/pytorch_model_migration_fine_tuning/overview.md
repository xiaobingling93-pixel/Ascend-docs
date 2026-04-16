# 概述

PyTorch作为当前深度学习领域中广泛采用的框架之一，经过Ascend Extension for PyTorch对昇腾平台的兼容性适配，现已支持在昇腾平台上高效运行。

本手册的主要目标是指导具有一定PyTorch模型训练基础的用户将原本在其他硬件平台（例如GPU）上训练的模型迁移到昇腾平台（NPU）。

手册内容涵盖模型全流程的迁移方法，无论是针对小模型还是大模型，都提供了详尽的指导。主要关注点是如何有效地将PyTorch训练模型迁移至昇腾平台上，并在合理精度误差范围内高性能运行。

本手册面向的读者主要是具有一定深度学习基础与编程经验的研究人员、工程师和开发者：

- 了解深度学习的基本概念和技术，能够使用Python编程语言、PyTorch框架进行深度学习模型开发和调试；
- 对深度学习模型的训练和优化有一定的了解，包括训练任务执行与评估，分布式训练，性能数据采集及分析等；
- 对常见的系统性能优化手段有基本认知，例如并行化、编译优化等。

## 什么是模型迁移

模型迁移是指将原本运行在GPU或其他硬件平台的深度学习模型迁移至NPU，并保障模型在合理精度误差范围内高性能运行。

## 为什么要做模型迁移

将模型从其他硬件平台迁移到NPU时，由于硬件架构和库的不同，涉及到一系列底层到上层的适配操作。以GPU为例，模型迁移至NPU需要适配的原因可分为三方面：

- 硬件特性和性能特点差异

    由于NPU和GPU的硬件特性和性能特点不同，模型在NPU上可能需要进一步的性能调试和优化，以充分发挥NPU的潜力。

- 计算架构差异

    NVIDIA GPU采用CUDA（Compute Unified Device Architecture）的并行计算架构，而华为NPU采用CANN（Compute Architecture for Neural Networks）的异构计算架构。

- 深度学习框架差异

    为了支持NPU硬件，需要通过[Ascend Extension for PyTorch](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/quick_start/quick_start.md)对PyTorch框架进行适配：包括适配张量运算、自动微分等功能，以便在NPU上高效执行。此外，[PyTorch](https://pytorch.org/tutorials/advanced/privateuseone.html)正持续原生支持NPU，以提供给用户更好的模型体验，实现迁移修改最小化。

## 如何进行模型迁移

模型迁移指导请参见表1。

**表 1**  模型迁移指导

<table>
  <tbody align="left">
    <tr>
      <th colspan="2">类别</th>
      <th>组件名称</th>
      <th>迁移指导</th>
    </tr>
    <tr>
      <td rowspan="2">传统模型</td>
      <td>已适配传统模型</td>
      <td rowspan="2">Ascend Extension for PyTorch</td>
      <td>请参见<a href="https://gitcode.com/Ascend/ModelZoo-PyTorch">支持模型列表</a>，并根据对应模型的README进行训练和推理。</td>
    </tr>
    <tr>
      <td>未适配传统模型</td>
      <td>请参见本文档进行迁移训练。</td>
    </tr>
    <tr>
      <td rowspan="3">大模型</td>
        <td>Megatron-LM分布式大模型</td>
        <td>MindSpeed Core亲和加速模块</td>
        <td>请参见<a href="https://gitcode.com/Ascend/MindSpeed/blob/2.3.0_core_r0.12.1/docs/user-guide/model-migration.md">《分布式训练加速库迁移指南》</a>。</td>
    </tr>
    <tr>
      <td>Megatron-LM大语言模型</td>
      <td>MindSpeed LLM套件</td>
      <td>请参见<a href="https://gitcode.com/Ascend/MindSpeed-LLM/wiki/Home.md">《MindSpeed LLM用户使用手册》</a>中的“MindSpeed LLM迁移指南（PyTorch框架）”章节。</td>
    </tr>
    <tr>
      <td>Megatron-LM多模态模型</td>
      <td>MindSpeed MM套件</td>
      <td>请参见<a href="https://gitcode.com/Ascend/MindSpeed-MM/blob/2.3.0/docs/user-guide/model-migration.md">《MindSpeed MM迁移调优指南》</a>。</td>
    </tr>
    <tr>
      <td colspan="2">自动驾驶模型</td>
      <td>Driving SDK自动驾驶加速库</td>
      <td>请参见<a href="https://gitcode.com/Ascend/DrivingSDK/blob/branch_v26.0.0/docs/zh/migration_tuning/model_optimization.md">《自驾模型迁移优化指导》</a>。</td>
    </tr>
  </tbody>
</table>
