# 性能调优工具介绍

性能调优工具是一套基于CANN的分析工具，其目的在于分析NPU在运行中模型的训练效率，在训练有瓶颈或者有性能优化需求的时候，可以利用性能调优工具进行性能分析。

工具主要包括Ascend PyTorch Profiler、性能比对工具、集群分析工具和MindStudio Insight。其中Ascend PyTorch Profiler是基于CANN实现，用于分析CANN上硬件的运行效率和性能数据；性能比对工具和集群分析工具取自开源社区，对Ascend PyTorch Profiler采集得到的数据进行专项分析；MindStudio Insight是一款调优可视化工具，集成了CANN数据的分析和可视化等功能。

## Ascend PyTorch Profiler

参见《CANN 性能调优工具用户指南》中的“[Ascend PyTorch调优工具](https://www.hiascend.com/document/detail/zh/canncommercial/900/devaids/Profiling/atlasprofiling_16_0033.html)”章节，Ascend PyTorch Profiler接口可全面采集PyTorch训练场景下的性能数据，主要包括PyTorch层算子信息、CANN层算子信息、底层NPU算子信息以及算子内存占用信息等，可以全方位分析PyTorch训练时的性能状态。

## 性能比对工具

性能比对工具（[compare\_tools](https://gitcode.com/ascend/mstt/tree/master/profiler/msprof_analyze/compare_tools)）用于比较模型在同平台或不同平台的性能差异，目前支持的比较GPU与NPU之间、NPU与NPU之间的性能差异，通过对训练耗时和内存占用的比对分析，定位到具体劣化的算子，帮助用户提升性能调优的效率。工具将训练耗时拆分为计算、通信、调度三大维度，并针对计算和通信分别进行算子级别的比对；将训练占用的总内存，拆分成算子级别的内存占用进行比对。

## 集群分析工具

集群分析工具（[cluster\_analyse](https://gitcode.com/ascend/mstt/tree/master/profiler/msprof_analyze/cluster_analyse)）是在集群场景下，通过此工具来进行集群数据的分析，当前主要对基于通信域的迭代内耗时分析、通信时间分析以及通信矩阵分析为主，从而定位慢卡、慢节点以及慢链路问题。

## MindStudio Insight

参见《MindStudio Insight 用户指南》中的“[安装与卸载](https://www.hiascend.com/document/detail/zh/mindstudio/830/GUI_baseddevelopmenttool/msascendinsightug/Insight_userguide_0005.html)”章节，完成工具准备。该工具是一款主要针对大模型集群场景的可视化调优工具，包括了Timeline视图、通信分析、计算耗时等的可视化呈现，以便用户分析潜在的性能瓶颈，并指导如何采取措施提升性能。
