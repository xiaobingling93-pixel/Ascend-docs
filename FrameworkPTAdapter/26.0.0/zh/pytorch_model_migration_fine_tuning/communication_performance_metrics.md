# 通信性能指标

针对同一通信算子在不同卡之间通信时间的波动的分析，其主要目的在于需要识别一些可能存在的通信算子内部瓶颈单元。在分析过程中，主要采用以下几个指标：

-   算子的执行时间以及内部等待耗时的变化和比例

    ![](./figures/communication_performance_metrics_fig_01.png)

    该指标可以用来初步识别是否存在慢卡瓶颈，对每块卡上的**wait\_ratio**，通过比较**max\_wait\_ratio**  \(等待时间长\)  **- min\_wait\_ratio**（等待时间短）与**wait\_ratio\_threshold**（当前设置为0.2）的关系，识别是否存在慢节点，当max和min的差值大于阈值threshold时，认为出现了慢节点，慢节点为**min\_wait\_ratio**（等待时间短）对应的卡。

-   通信带宽分析

    DMA（Direct Memory Access）指的是直接内存访问。当CPU完成传输配置后，将不再参与传输过程，由DMA控制器将数据从一个地址空间复制到另外一个地址空间。

    按照传输介质，可以将DMA分为RDMA和SDMA。

    -   RDMA指的是其他节点通过网卡对当前节点进行直接内存访问。
    -   SDMA代表在节点内进行的直接内存访问。

    通信数据一般通过DMA机制在不同卡和不同节点间传输。在单节点内，遵循如下逻辑分析快慢卡。

    1.  对比通信算子在各卡上的数据传输耗时，通信算子执行最短的rank，即为慢卡。
    2.  通过传输数量sdma\_data、rdma\_data和传输时间sdma\_transit\_time、rdma\_transit\_time计算得到传输带宽。

        ![](./figures/communication_performance_metrics_fig_02.png)

        ![](./figures/communication_performance_metrics_fig_03.png)

