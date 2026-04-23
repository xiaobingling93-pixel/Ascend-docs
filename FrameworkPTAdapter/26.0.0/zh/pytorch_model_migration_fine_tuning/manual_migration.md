# 手工迁移

## 简介

手工迁移需要用户对AI模型有迁移基础，了解GPU与NPU的接口设计的异同点以及各种迁移手段。手工迁移过程中，各个模型使用的迁移方法不完全相同，下文给出手工迁移的核心要点。

## 单卡迁移

1. 导入NPU相关库。

    ```python
    import torch
    import torch_npu  
    ```

2. 指定NPU作为训练设备。指定训练设备需修改模型训练脚本，有两种指定方式：
    - to\(device\)方式：定义好device后可通过xx_.to\(device\)的方式将模型或数据集等加载到GPU或NPU上，如model.to\(device\)。该方式可以指定需要的训练资源，使用比较灵活。

        迁移前：

        ```python
        device = torch.device('cuda:{}'.format(local_rank)) # local_rank为当前设备卡号
        model.to(device)
        ```

        迁移后：

        ```python
        device = torch.device('npu:{}'.format(local_rank))
        model.to(device)
        ```

    - set\_device方式：调用set\_device接口，指定训练设备。需注意该方式不会自动使用NPU，用户需要手动在想使用NPU的地方，添加xx_.npu\(\)代码，将模型数据集等加载到NPU上，如model.npu\(\)。

        迁移前：

        ```python
        torch.cuda.set_device(local_rank)
        ```

        迁移后：

        ```python
        torch_npu.npu.set_device(local_rank)
        ```

3. 替换CUDA接口：将训练脚本中的CUDA接口替换为NPU接口，例如模型、损失函数、数据集等迁移到NPU上。常见替换接口请参见[常见PyTorch迁移替换接口](pytorch_migrate_api.md)。更多接口请参见《[Ascend Extension for PyTorch 自定义API参考](https://gitcode.com/Ascend/op-plugin/blob/26.0.0/docs/zh/custom_APIs/menu_Pytorch_API.md)》。

    1. CUDA接口替换为NPU接口。

        迁移前：

        ```python
        torch.cuda.is_available()
        ```

        迁移后：

        ```python
        torch_npu.npu.is_available()
        ```

    2. 模型迁移。

        迁移前：

        ```python
        model.cuda(local_rank)
        ```

        迁移后：

        ```python
        model.npu(local_rank)
        ```

    3. 数据集迁移。

        迁移前：

        ```python
        images = images.cuda(local_rank, non_blocking=True)
        target = target.cuda(local_rank, non_blocking=True)
        ```

        迁移后：

        ```python
        images = images.npu(local_rank, non_blocking=True)
        target = target.npu(local_rank, non_blocking=True)
        ```

## 多卡迁移（分布式训练迁移）

除单卡迁移包含的3个修改要点外，在分布式场景下，还需要切换通信方式，直接修改init\_process\_group的值。

修改前，GPU使用nccl方式：

```python
dist.init_process_group(backend='nccl',init_method="tcp://127.0.0.1:**", ...... ,rank=args.rank)    # **为端口号，根据实际选择一个闲置端口填写
```

修改后，NPU使用hccl方式：

```python
dist.init_process_group(backend='hccl',init_method="tcp://127.0.0.1:**", ...... ,rank=args.rank)    # **为端口号，根据实际选择一个闲置端口填写
```
