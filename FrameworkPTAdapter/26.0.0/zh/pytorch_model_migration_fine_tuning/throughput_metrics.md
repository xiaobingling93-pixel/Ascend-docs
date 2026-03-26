# 吞吐率指标

神经网络的吞吐率（Throughput）定义为网络模型在单位时间（例如1s）内可以处理的最大输入的训练样本数据。

与涉及单个样本数据处理的延迟latency不同，为了实现最大吞吐率，我们希望在集群训练的过程中有效并行处理尽可能多的样本数据。这种有效的并行性显然依赖于数据、模型和设备规模。

因此，为了正确测量吞吐率，可以执行以下两个步骤：

1.  估计允许最大并行度的最佳训练样本数据批量大小，即batch size；
2.  在AI训练集群中给定这个最佳batch size大小，测量网络在1秒钟内可以处理的训练样本数据。

要找到最佳batch size大小值，一个有效的经验法则是达到Ascend昇腾处理器对给定数据类型的内存限制，即batch size占满内存。这取决于硬件类型、网络的大小以及输入数据的大小。

要找到这个最大的batch size，最快方法是执行二分搜索。当时间不重要时，简单的顺序搜索就足够了。不过在大模型训练的过程中，batch size的值会影响到重计算、Pipeline并行、Tensor并行等不同并行模式的配比，还有micro batch size的数据配比。因此默认batch size为16的倍数比较合理。

这确定了AI加速卡上可以处理的最大批量大小，用于训练大模型及其处理的输入训练样本数据。

在确定最佳batch size值后，可以使用以下公式计算实际吞吐量：

![](./figures/throughput_metrics_fig_01.png)

-   **BS**为batch size per DP，即每个数据并行维度的batch size大小。
-   **N**为集群中数据并行维度的大小。
-   **step\_time**为在分布式集群中，执行完一个total batch size的时间（单位为s）。

对于固定shape的NLP任务，吞吐率的单位为samples/s时，可通过**seq\_len**，将吞吐率指标单位换算为tokens/s，吞吐率换算公式为：

![](./figures/throughput_metrics_fig_02.png)

假设GLM10B网络模型的吞吐率为25 samples/s，max seq\_len为1024，那么按照tokens来计算吞吐率为25 \* 1024=25600 tokens/s，也就是每秒能处理2万多个tokens。

