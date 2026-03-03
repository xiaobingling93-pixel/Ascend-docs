# MindStudio使用导读

MindStudio是华为面向昇腾AI开发者提供的全流程工具链，致力于提供端到端的昇腾AI应用开发解决方案，使能开发者高效完成训练开发、推理开发和算子开发。

## 快速入门

- [算子开发工具快速入门](https://gitcode.com/Ascend/msot/blob/master/docs/zh/quick_start/op_tool_quick_start.md)
- 训练开发工具快速入门
  - [PyTorch场景msTT工具快速入门](https://gitcode.com/Ascend/mstt/blob/master/docs/zh/pytorch_mstt_quick_start.md)
  - [MindSpore场景msTT工具快速入门](https://gitcode.com/Ascend/mstt/blob/master/docs/zh/mindspore_mstt_quick_start.md)
- [推理开发工具快速入门](https://gitcode.com/Ascend/msit/blob/master/docs/zh/msit_quick_start.md)

训练、推理、算子开发场景下MindStudio工具使用的快速入门。

## 可视化工具

[MindStudio Insight工具](https://gitcode.com/Ascend/msit/tree/master/docs/zh/overview.md)

可视化性能调优工具，辅助定位模型和算子的性能问题。

## 算子开发工具

- [算子开发工具链](https://gitcode.com/Ascend/msot/blob/master/docs/zh/overview.md)

  提供算子开发工具整包的版本说明和安装指导。

  各个子工具也可以使用对应子包单独安装，可参照对应子工具安装手册。

- [算子设计工具](https://gitcode.com/Ascend/mskpp/blob/master/docs/zh/overview.md)

  支持用户输入算子表达，进而预测算子在这一算法实现下的性能上限。

- [算子调用工具](https://gitcode.com/Ascend/mskl/blob/master/docs/zh/overview.md)

  利用提供的接口在Python脚本中快速实现Kernel下发代码生成、编译及运行Kernel。

- [算子工程创建工具](https://gitcode.com/Ascend/msopgen/blob/master/docs/zh/overview.md)

  包含msOpGen提供模板工程生成能力，简化算子工程搭建并辅助算子测试验证；msOpST旨在真实的硬件环境中，对算子的输入输出进行测试，以验证算子的功能是否正确。

- [算子异常检测工具](https://gitcode.com/Ascend/mssanitizer/blob/master/docs/zh/overview.md)

  提供内存检测、竞争检测、未初始化检测及同步检测的能力，支持多核程序下内存问题的精准定位。

- [算子调试工具](https://gitcode.com/Ascend/msdebug/blob/master/docs/zh/overview.md)

  提供基于昇腾处理器的原生环境调试能力，实现灵活的变量展示。支持算子功能调试，单步调试（上板）等功能。

- [算子调优工具](https://gitcode.com/Ascend/msopprof/blob/master/docs/zh/overview.md)

  提供上板和仿真的性能数据采集方式，并通过MindStudio Insight进行可视化呈现，方便用户快速定位算子性能瓶颈。

## 训练开发工具

[训练开发工具链](https://gitcode.com/Ascend/mstt/tree/master/docs/zh/overview.md)

提供训练开发工具整包的版本说明和安装指导。

各个子工具也可以使用对应子包单独安装，可参照对应子工具安装手册。

**分析迁移**

[分析迁移工具](https://gitcode.com/Ascend/mstt/blob/master/msfmktransplt/docs/zh/msfmktransplt_instruct.md)

将PyTorch训练脚本一键式迁移至昇腾NPU。

**精度调试**

[精度调试工具](https://gitcode.com/Ascend/msprobe/blob/master/docs/zh/overview.md)

针对昇腾提供的全场景精度工具链，帮助用户快速提高模型精度定位效率。

**性能调优**

- [模型调优工具](https://gitcode.com/Ascend/msprof/blob/master/docs/zh/overview.md)

  支持采集CANN和NPU性能数据，提升昇腾设备性能调优效率。

- [Ascend PyTorch调优工具](https://gitcode.com/Ascend/pytorch/blob/v2.7.1/docs/zh/ascend_pytorch_profiler/ascend_pytorch_profiler_user_guide.md)

  提供PyTorch训练/在线推理场景采集性能数据，输出可视化的性能数据文件，提升性能分析效率。

- [性能分析工具](https://gitcode.com/Ascend/msprof-analyze/blob/master/docs/zh/overview.md)

  基于采集的性能数据进行分析，提供昇腾设备性能瓶颈快速识别能力。

- [内存检测工具](https://gitcode.com/Ascend/msmemscope/blob/master/docs/zh/overview.md)

  提供整网级多维度显存数据采集、自动诊断、优化分析能力。

- [在线监控工具](https://gitcode.com/Ascend/msmonitor/blob/master/docs/zh/overview.md)

  支持落盘和在线性能数据采集，提供集群场景性能监测及定位能力。

- [msPTI工具](https://gitcode.com/Ascend/mspti/blob/master/docs/zh/overview.md)

  用户可以通过msPTI构建针对NPU应用程序的工具，用于分析应用程序的性能。

## 推理开发工具

[推理开发工具链](https://gitcode.com/Ascend/msit/tree/master/docs/zh/overview.md)

提供推理开发工具整包的版本说明和安装指导。

各个子工具也可以使用对应子包单独安装，可参照对应子工具安装手册。

**性能调优**

- [模型调优工具](https://gitcode.com/Ascend/msprof/blob/master/docs/zh/overview.md)

  支持采集CANN和NPU性能数据，提升昇腾设备性能调优效率。

- [在线监控工具](https://gitcode.com/Ascend/msmonitor/blob/master/docs/zh/overview.md)

  支持落盘和在线性能数据采集，提供集群场景性能监测及定位能力。

- [服务化性能调优工具](https://gitcode.com/Ascend/msserviceprofiler/tree/master/docs/zh/overview.md)

  支持请求调度、模型执行过程可视化，提升服务化性能分析效率。

- [预检工具](https://gitcode.com/Ascend/msit/blob/master/msprechecker/README.md)

  支持环境预检，连通性预检，推理过程中的落盘和比对功能。

- [性能分析工具](https://gitcode.com/Ascend/msprof-analyze/blob/master/docs/zh/overview.md)

  基于采集的性能数据进行分析，提供昇腾设备性能瓶颈快速识别能力。

**精度调试**

- [精度调试工具](https://gitcode.com/Ascend/msprobe/blob/master/docs/zh/overview.md)

  针对昇腾提供的全场景精度工具链，帮助用户快速提高模型精度定位效率。

- [内存检测工具](https://gitcode.com/Ascend/msmemscope/blob/master/docs/zh/overview.md)

  提供整网级多维度显存数据采集、自动诊断、优化分析能力。

**模型量化**

模型压缩工具

以加速为目标、压缩为技术、昇腾为根本的亲和压缩工具。

## 实践案例

- [性能问题通用定位指南](https://gitcode.com/Ascend/docs/blob/master/MindStudio/general_performance_issue_troubleshooting_guide/overview.md)
- [大模型训练性能瓶颈定位流程案例](https://gitcode.com/Ascend/docs/blob/master/MindStudio/case_of_troubleshooting_performance_bottleneck_in_llm_training.md)
- [大模型训练精度问题定位案例](https://gitcode.com/Ascend/docs/blob/master/MindStudio/case_of_accuracy_issue_in_llm_training.md)
- [大模型推理精度问题分析案例](../cases/llm_inference_accuracy_analysis_case.md)
- [大模型推理量化调试调优指南](https://gitcode.com/Ascend/docs/blob/master/MindStudio/foundation_model_inference_quantization_debugging_and_tuning_guide.md)
- [传统模型推理迁移调试调优全流程指南](../cases/traditional_model_inference_optimizing_case.md)

## API

- [mstx API参考](https://gitcode.com/Ascend/mstx/tree/master/docs/zh/overview.md)

## 参考

- [昇腾产品形态说明](https://www.hiascend.com/document/detail/zh/AscendFAQ/ProduTech/productform/hardwaredesc_0001.html)
- 公网URL及邮箱
- 通信矩阵
- [术语](https://www.hiascend.com/document/detail/zh/Glossary/gls/gls_0001.html)