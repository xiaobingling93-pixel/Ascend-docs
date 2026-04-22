# MindSpore Profiler

MindSpore调优工具（MindSpore Profiler）MindSpore Profiler是针对MindSpore框架开发的性能分析工具，通过在MindSpore训练脚本中添加MindSpore Profiler接口，执行训练的同时采集性能数据，完成训练后直接输出可视化的性能数据文件，提升了性能分析效率。MindSpore Profiler接口可全面采集MindSpore训练场景下的性能数据，主要包括MindSpore层算子信息、CANN层算子信息、底层NPU算子信息、以及算子内存占用信息等，可以全方位分析MindSpore训练时的性能状态。

| 功能                                                         | 简介                                                         | 适用场景/优势                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [性能数据采集](https://www.mindspore.cn/tutorials/zh-CN/master/debug/profiler.html#%E6%96%B9%E5%BC%8F%E4%B8%80-mindsporeprofilerprofile%E6%8E%A5%E5%8F%A3%E4%BD%BF%E8%83%BD) | 通用性能数据采集、分析功能。                                 | 模型性能未达到预期，需要对模型性能数据进行采集、分析和调优。 |
| [轻量化打点数据采集](https://www.mindspore.cn/tutorials/zh-CN/master/debug/profiler.html#%E6%96%B9%E5%BC%8F%E4%BA%94-%E8%BD%BB%E9%87%8F%E5%8C%96%E6%89%93%E7%82%B9) | mstx轻量化打点模块，支持用户自定义打点以及框架内置打点。     | 大集群场景轻量化获取模型关键指标性能数据，实现问题快速定界。 |
| [动态Profiler](https://www.mindspore.cn/tutorials/zh-CN/master/debug/profiler.html#%E6%96%B9%E5%BC%8F%E4%BA%8C-%E5%8A%A8%E6%80%81profiler%E4%BD%BF%E8%83%BD) | 在不中断训练流程的前提下，修改配置文件并完成新配置下的性能数据采集任务。 | 常稳训练中发现性能劣化、抖动等，期望在不中断训练情况下进行性能分析。 |
| [离线解析数据](https://www.mindspore.cn/tutorials/zh-CN/master/debug/profiler.html#%E6%96%B9%E5%BC%8F%E5%9B%9B-%E7%A6%BB%E7%BA%BF%E8%A7%A3%E6%9E%90) | 对已采集的数据进行离线解析。                                 | 期望在模型运行过程中仅进行性能数据采集，以节省整体运行时间；或对历史已采集数据进行再次解析。 |
