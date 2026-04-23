# 优化建议

## 调整内存参数

- 设置内存因子，限制进程申请的内存上限，取值0\~1。

    例如：设置0.95，可使用的内存上限为60G \* 0.95 = 57G。

    内存申请超过上限触发内存释放。设置PyTorch申请的内存上限，可以避免内存使用极限场景下，PyTorch耗尽device内存，其他组件申请内存失败导致的进程异常。

    ```python
    torch_npu.npu.set_per_process_memory_fraction(0.95)
    ```

- 垃圾回收阈值，默认不开启，与内存因子配合使用，取值0\~1，为内存上限的百分比。

    当内存申请达到gc阈值，则触发内存池空闲内存块回收。建议由大到小配置调试。

    ```python
    export PYTORCH_NPU_ALLOC_CONF="garbage_collection_threshold:0.95"
    ```

- 内存块允许切分上限，单位MB。

    例如：设置50，大于等于50M的内存块不允许切分使用。设置该参数可避免大内存块被切分导致较多的内存碎片影响内存复用。调试时可以先采集内存profiling，按照算子内存申请降序排列，参数值由大到小尝试。

    ```python
    export PYTORCH_NPU_ALLOC_CONF="max_split_size_mb:50"
    ```

- 使能内存池扩展段功能，由PyTorch管理虚拟地址与物理地址映射，降低内存碎片。

    对于动态shape场景，shape随step增加而增大，从而导致内存块不能复用内存碎片增加，对该场景有较好优化。

    ```python
    export PYTORCH_NPU_ALLOC_CONF="expandable_segments:True"
    ```

    更多虚拟内存的使用请参考《PyTorch 框架特性指南》中的“[虚拟内存](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/framework_feature_guide_pytorch/virtual_memory.md)”章节。

> [!NOTE]
>
> 若同时设置多个参数可以通过逗号分隔，如下所示：
>
>```shell
>export PYTORCH_NPU_ALLOC_CONF="garbage_collection_threshold:0.95,max_split_size_mb:50"
>```
>
>**注意：expandable\_segments:True不能与上述两个环境变量共用**。

## 多流复用

流（stream）是PyTorch的一个重要机制，每个流代表一个时序上严格的执行逻辑。一般地，PyTorch在执行时会启动多个流，来并行完成模型的通信和计算任务。每个流在执行过程中会根据自身需要向设备（device）申请内存，称为该流的内存池。如果一个流申请的内存池需要给另一个流使用，两个流之间需要进行通信，以当前流的内存块（block）对应的数据执行完作为对应流使用这块内存的标志。host算子下发过快时，计算流的算子来不及复用通信流的算子，特别是通信流上的算子有依赖时，需要全部结束再释放复用，多流复用的逻辑就是让通信流上的内存提前释放，让计算流复用。

多流复用是PyTorch的非常有效的内存优化方法，需要配置环境变量，代码如下：

```shell
export MULTI_STREAM_MEMORY_REUSE=1
```

更多多流复用内容请参考《PyTorch 框架特性指南》中的“[多流内存复用](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/framework_feature_guide_pytorch/multistream_memory_reuse.md)”章节。

## 减小HCCL通信缓存

HCCL通信缓存是一种高性能、高可靠的通信缓存技术，用于提高分布式计算系统的通信效率和可靠性。它通过在计算节点上预分配一定数量的内存空间，用于存储通信数据，避免了频繁的内存分配和释放操作，从而提高了通信效率。

可以通过调整以下数值，来控制HCCL的缓存大小。默认值为200MB。

```shell
export HCCL_BUFFSIZE=200
```

> [!NOTE]
>
> 将HCCL\_BUFFSIZE设置得较小可能导致集群通信速度有严重劣化。

## Python GC优化

GC（Garbage Collector）是Python提供的可选的垃圾回收器的接口，提供的功能包括关闭收集器、调整收集频率以及设置调试选项。它同时提供对回收器找到但是无法释放的不可达对象的访问。

我们可以通过调用gc.disable\(\)来关闭Python的自动垃圾回收机制。

在大模型训练中，我们可能因为大量进程的开销而频繁触发GC进行垃圾回收，而垃圾回收在一定程度上会占用CPU的性能，如果频繁触发，可能会造成模型训练过程中性能的抖动。

在遇到此类问题时，首先可以主动通过gc.disable\(\)关闭自动垃圾回收机制；同时，通过如下接口设置回收频率和回收次数，保证内存稳定的同时降低性能抖动。

```python
gc.set_threshold(threshold0, threshold1, threshold2)
```

其中threshold0、threshold1及threshold2分别代表Python的三代对象。

所有新建的对象都是0代对象，当某一代对象经历过垃圾回收，依然存活，那么它就被归入下一代对象。垃圾回收启动时，一定会扫描所有0代对象，如果0代经过一定次数的垃圾回收，那么就会启动对0代和1代的扫描清理；当1代也经历了一定次数的垃圾回收，那么会启动0、1和2，即所有对象的扫描。命令示例如下：

```python
gc.set_threshold(700, 10, 5)
```

其中700、10、5分别代表0、1、2三代最大的引用数量，超过引用上限则会触发GC进行垃圾回收。
