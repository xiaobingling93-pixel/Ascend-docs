# 流水优化

- **一级流水优化**

    一级流水优化是一种通用有效的优化方法，主要是将部分算子适配任务迁移至二级流水，使两级流水负载更均衡，并减少dequeue唤醒时间。

- **使能说明**

    该特性可以通过环境变量设置，一般用于以下场景：host-bound严重的网络场景。

    使用方法如下：

    ```shell
    export TASK_QUEUE_ENABLE=2
    ```

    该环境变量的实现细节请参考《环境变量参考》中的“[TASK\_QUEUE\_ENABLE](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/environment_variable_reference/TASK_QUEUE_ENABLE.md)”章节。

> [!NOTE]
>
> - ASCEND\_LAUNCH\_BLOCKING设置为“1”时，task\_queue算子队列关闭，TASK\_QUEUE\_ENABLE设置不生效。
> - TASK\_QUEUE\_ENABLE配置为“2”时，由于内存并发，可能导致运行中NPU内存峰值上升。
