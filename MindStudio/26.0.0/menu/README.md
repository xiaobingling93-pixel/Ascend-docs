# MindStudio使用导读

MindStudio是华为面向昇腾AI开发者提供的全流程工具链，致力于提供端到端的昇腾AI应用开发解决方案，使能开发者高效完成训练开发、推理开发和算子开发。

-  [MindStudio是什么](https://gitcode.com/Ascend/docs/blob/master/MindStudio/26.0.0/mindstudio_introduction.md)
- [版本说明](https://gitcode.com/Ascend/docs/blob/master/MindStudio/26.0.0/release_notes.md)
  
- [快速入门](https://gitcode.com/Ascend/docs/blob/master/MindStudio/26.0.0/menu/ms_quickstart_menu.md)
   -  [算子开发工具快速入门](https://gitcode.com/Ascend/msot/blob/26.0.0/docs/zh/quick_start/op_tool_quick_start.md)
   -  训练场景工具快速入门
      - [PyTorch场景msTT工具快速入门](https://gitcode.com/Ascend/mstt/blob/26.0.0/docs/zh/pytorch_mstt_quick_start.md)
      - [MindSpore场景msTT工具快速入门](https://gitcode.com/Ascend/mstt/blob/26.0.0/docs/zh/mindspore_mstt_quick_start.md)
   -  [大模型推理工具快速入门](https://gitcode.com/Ascend/msit/blob/26.0.0/docs/zh/msit_quick_start.md)


## 可视化工具

[MindStudio Insight工具](https://gitcode.com/Ascend/msinsight/blob/26.0.0/docs/zh/user_guide/overview.md)

可视化性能调优工具，辅助定位模型和算子的性能问题。

## 算子开发工具

- [算子设计 msKPP](https://gitcode.com/Ascend/mskpp/blob/26.0.0/docs/zh/quick_start/mskpp_quick_start.md)

   支持用户输入算子表达，进而预测算子在这一算法实现下的性能上限。

- [算子调用 msKL](https://gitcode.com/Ascend/mskl/blob/26.0.0/docs/zh/quick_start/mskl_quick_start.md)

   利用提供的接口在Python脚本中快速实现Kernel下发代码生成、编译及运行Kernel。

- [算子工程 msOpGen](https://gitcode.com/Ascend/msopgen/blob/26.0.0/docs/zh/quick_start/msopgen_quick_start.md)

   包含msOpGen提供模板工程生成能力，简化算子工程搭建并辅助算子测试验证；msOpST旨在真实的硬件环境中，对算子的输入输出进行测试，以验证算子的功能是否正确。

- [异常检测 msSanitizer](https://gitcode.com/Ascend/mssanitizer/blob/26.0.0/docs/zh/quick_start/mssanitizer_quick_start.md)

   提供内存检测、竞争检测、未初始化检测及同步检测的能力，支持多核程序下内存问题的精准定位。

- [算子调试 msDebug](https://gitcode.com/Ascend/msdebug/blob/26.0.0/docs/zh/quick_start/msdebug_quick_start.md)

   提供基于昇腾处理器的原生环境调试能力，实现灵活的变量展示。支持算子功能调试，单步调试（上板）等功能。

- [算子调优 msOpProf](https://gitcode.com/Ascend/msopprof/blob/26.0.0/docs/zh/quick_start/msopprof_quick_start.md)

   提供上板和仿真的性能数据采集方式，并通过MindStudio Insight进行可视化呈现，方便用户快速定位算子性能瓶颈。

## 训练开发工具

**开发**

- [分析迁移 msTransplant](https://gitcode.com/Ascend/mstt/blob/26.0.0/msfmktransplt/README.md)

  将PyTorch训练脚本一键式迁移至昇腾NPU。

**调试**

- [精度调试 msProbe](https://gitcode.com/Ascend/msprobe/blob/26.0.0/docs/zh/quick_start/pytorch_quick_start.md)

  针对昇腾提供的全场景精度工具链，帮助用户快速提高模型精度定位效率。

**调优**

- [模型调优 msProf](https://gitcode.com/Ascend/msprof/blob/26.0.0/docs/zh/quick_start.md)

     支持采集CANN和NPU性能数据，提升昇腾设备性能调优效率。

- [Ascend PyTorch Profiler](https://gitcode.com/Ascend/pytorch/blob/v2.7.1/docs/zh/ascend_pytorch_profiler/ascend_pytorch_profiler_user_guide.md)

     提供PyTorch训练/在线推理场景采集性能数据，输出可视化的性能数据文件，提升性能分析效率。

- [MindSpore Profiler](../mindspore_profiler_user_guide.md)

     提供MindSpore训练/在线推理场景采集性能数据，输出可视化的性能数据文件，提升性能分析效率。

- [工具扩展SDK msTX](https://gitcode.com/Ascend/mstx/blob/26.0.0/docs/zh/api_reference/mstx_api_reference.md)

     可以自定义采集时间段或者关键函数的开始和结束时间点，识别关键函数或迭代等信息，对性能和算子问题快速定界。

- [性能分析 msprof-analyze](https://gitcode.com/Ascend/msprof-analyze/blob/26.0.0/docs/zh/README.md)

     基于采集的性能数据进行分析，提供昇腾设备性能瓶颈快速识别能力。

- [内存分析 msMemScope](https://gitcode.com/Ascend/msmemscope/blob/26.0.0/docs/zh/quick_start.md)

     提供整网级多维度显存数据采集、自动诊断、优化分析能力。

- [在线监测 msMonitor](https://gitcode.com/Ascend/msmonitor/blob/26.0.0/docs/zh/quick_start.md)

     支持落盘和在线性能数据采集，提供集群场景性能监测及定位能力。

- [调优工具接口库 msPTI](https://gitcode.com/Ascend/mspti/blob/26.0.0/docs/zh/quick_start.md)

     用户可以通过msPTI构建针对NPU应用程序的工具，用于分析应用程序的性能。

**扩展**

实践案例

- [性能问题通用定位指南](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/general_performance_issue_troubleshooting_guide/guide.md)
- [大模型训练性能瓶颈定位流程案例](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/case_of_troubleshooting_performance_bottleneck_in_llm_training.md)
- [大模型训练精度问题定位案例](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/case_of_accuracy_issue_in_llm_training.md)

## 推理开发工具

**调优**

- [模型调优 msProf](https://gitcode.com/Ascend/msprof/blob/26.0.0/docs/zh/quick_start.md)

  支持采集CANN和NPU性能数据，提升昇腾设备性能调优效率。

- [在线监测 msMonitor](https://gitcode.com/Ascend/msmonitor/blob/26.0.0/docs/zh/quick_start.md)

  支持落盘和在线性能数据采集，提供集群场景性能监测及定位能力。

- [服务化调优 msServiceProf](https://gitcode.com/Ascend/msserviceprofiler/blob/26.0.0/docs/zh/quick_start.md)

  支持请求调度、模型执行过程可视化，提升服务化性能分析效率。

- [预检工具 msprechecker](https://gitcode.com/Ascend/msit/blob/master/msprechecker/README.md)

  支持环境预检，连通性预检，推理过程中的落盘和比对功能。

- [性能分析 msprof-analyze](https://gitcode.com/Ascend/msprof-analyze/blob/26.0.0/docs/zh/README.md)

  基于采集的性能数据进行分析，提供昇腾设备性能瓶颈快速识别能力。

**调试**

- [精度调试 msProbe](https://gitcode.com/Ascend/msprobe/blob/26.0.0/docs/zh/dump/vllm_dump_instruct.md)

  针对昇腾提供的全场景精度工具链，帮助用户快速提高模型精度定位效率。

- [内存分析 msMemScope](https://gitcode.com/Ascend/msmemscope/blob/26.0.0/docs/zh/quick_start.md)

  提供整网级多维度显存数据采集、自动诊断、优化分析能力。

**扩展**

- [模型量化 msModelSlim](https://gitcode.com/Ascend/msmodelslim/blob/26.0.0/docs/zh/getting_started/quantization_quick_start.md)

  以加速为目标、压缩为技术、昇腾为根本的亲和压缩工具。

- [工具扩展SDK msTX](https://gitcode.com/Ascend/mstx/blob/26.0.0/docs/zh/api_reference/mstx_api_reference.md)

  可以自定义采集时间段或者关键函数的开始和结束时间点，识别关键函数或迭代等信息，对性能和算子问题快速定界。

- 实践案例

  - [性能问题通用定位指南](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/general_performance_issue_troubleshooting_guide/guide.md)
  - [大模型推理精度问题分析案例](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/llm_inference_accuracy_analysis_case.md)
  - [传统模型推理迁移调试调优全流程指南](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/traditional_model_inference_optimizing_case.md)
  - [大模型推理量化调试调优指南](https://gitcode.com/Ascend/docs/blob/26.0.0/MindStudio/cases/foundation_model_inference_quantization_debugging_and_tuning_guide.md)

## 参考

- [昇腾产品形态说明](https://www.hiascend.com/document/detail/zh/AscendFAQ/ProduTech/productform/hardwaredesc_0001.html)
- [术语](https://www.hiascend.com/document/detail/zh/Glossary/gls/gls_0001.html)
