# 内存快照

为了调试内存使用情况，PyTorch提供了生成内存快照的方法，使用户可以更好地了解代码中最耗费内存的地方，并且可以在代码中进行相应的优化，以减少内存使用量。更多内存快照的介绍请参考《PyTorch 框架特性指南》中的“[内存快照](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/framework_feature_guide_pytorch/memory_snapshot.md)”章节。在生成内存快照后，可以使用交互式查看器（[memory\_viz](http://pytorch.org/memory_viz)）来查看快照。

内存快照的使用方法和案例具体请参考[LINK](https://pytorch.org/docs/2.1/torch_cuda_memory.html#understanding-cuda-memory-usage)。内存快照的API具体用法请参考[LINK](https://pytorch.org/docs/2.1/torch_cuda_memory.html#snapshot-api-reference)。

> [!NOTE]
>
> 在使用支持的API接口时，需要将API名称中的cuda转换为NPU形式才能使用：`torch.cuda.***`变换为`torch_npu.npu.***`。

除内存快照保存的相关文件外，同目录下还额外保存了CANN相关组件的内存占用信息，文件目录为PROF\_XXX，用户可参考《CANN 性能调优工具用户指南》中的“[msprof模型调优工具](https://www.hiascend.com/document/detail/zh/canncommercial/850/devaids/Profiling/atlasprofiling_16_0010.html)”章节，导出数据对内存进行进一步分析。

同时支持从so文件中加载自定义NPU内存分配器，详情请参考《PyTorch 框架特性指南》中的“[自定义内存分配器](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/framework_feature_guide_pytorch/custom_memory_allocator.md)”章节。
