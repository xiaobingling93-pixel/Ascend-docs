# 单机多卡场景

## 环境准备

1. 请参见《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（商用版）或《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（社区版）手册，安装硬件与OS，NPU驱动和固件以及CANN软件；请参见《[Ascend Extension for PyTorch 软件安装指南](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/installation_guide/installation_description.md)》手册，安装PyTorch框架、torch\_npu插件以及APEX模块（可选）。
2. 根据实际需求准备模型环境，如conda、docker以及三方库依赖。

    > [!CAUTION]
    > 
    > 分布式训练场景下，HCCL会使用Host服务器的部分端口进行集群信息收集，需要操作系统预留该部分端口。HCCL建议使用60000-60015端口，也可以通过环境变量HCCL\_IF\_BASE\_PORT指定Host网卡起始端口，此场景下需要预留以指定端口起始的16个端口。
    > - 若操作系统端口号预留仅需临时生效，可执行如下命令，临时生效的配置重启后会失效：
    >
    >     ```shell
    >     sysctl -w net.ipv4.ip_local_reserved_ports=60000-60015
    >     ```
    >
    > - 若操作系统端口号预留需永久生效，可执行如下操作：
    >     1. 以root用户登录服务器，编辑`/etc/sysctl.conf`文件。
    >
    >         ```shell
    >         vim /etc/sysctl.conf
    >         ```
    >
    >     2. 在“/etc/sysctl.conf”文件末尾加上`net.ipv4.ip_local_reserved_ports=60000-60015`，保存并退出。
    >     3. 执行如下命令使配置生效。
    >
    >         ```shell
    >         sysctl -p
    >         ```

## 模型脚本配置

本节以[适配样例（DDP单NPU单进程场景）](ddp_single_npu_single_process_example.md)章节的代码为样例，介绍将单机单卡训练脚本修改为单机多卡训练脚本的核心步骤。

1. 在主函数中添加如下代码。

    1. 添加分布式逻辑，即在环境变量中获取local\_rank参数。

        ```python
        local_rank = int(os.environ.get("LOCAL_RANK", 0)) 
        ```

    2. 用local\_rank自动获取device号。

        ```python
        device = torch.device(f'npu:{local_rank}')
        ```

    3. 初始化，将通信方式设置为HCCL。

        ```python
        torch.distributed.init_process_group(backend="hccl",rank=(args.node_rank)*(args.nproc_per_node) + local_rank)
        ```

2. 定义模型后，开启DDP模式。

    ```python
    model = torch.nn.parallel.DistributedDataParallel(model, device_ids=[local_rank], output_device=local_rank)
    ```

3. 在获取训练数据集后，设置train\_sampler。

    ```python
    train_sampler = torch.utils.data.distributed.DistributedSampler(train_data)
    ```

4. 将train\_sampler赋值至DataLoader中的sampler。

    ```python
    train_dataloader = DataLoader(dataset = train_data, batch_size=batch_size, sampler = train_sampler)
    ```

## 拉起训练

有5种脚本启动方式可拉起多卡训练：

- [shell脚本方式](launch_multi_gpu_train_demo.md#suctom-anchor01)
- [mp.spawn方式](launch_multi_gpu_train_demo.md#suctom-anchor02)
- [torch.distributed.launch方式](launch_multi_gpu_train_demo.md#suctom-anchor03)
- [torchrun方式](launch_multi_gpu_train_demo.md#suctom-anchor04)
- [torch\_npu\_run方式](launch_multi_gpu_train_demo.md#suctom-anchor05)**（集群场景推荐）**：此方式是torchrun在大集群场景的改进版，提升集群建链性能。

附录[拉起多卡训练脚本示例](launch_multi_gpu_train_demo.md)中，以一个简单模型脚本为样例，展示了每种拉起方式脚本代码的修改方法以及各种拉起方式的适配方法，用户可以参考学习。

> [!NOTE]
>
> - 集合通信存在如下约束：
> - 数据并行模式中不同device上执行的计算图相同。
> - 针对<term>Atlas 训练系列产品</term>：AllReduce和reduce\_scatter仅支持int8、int32、float16和float32数据类型。
> - 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>：AllReduce和reduce\_scatter仅支持int8、int32、float16、float32和bfloat16数据类型。
> - 针对<term>Atlas A2 训练系列产品</term>/<term>Atlas A3 训练系列产品</term>，若用户准备进行2卡训练，可将8卡训练脚本进行改写，改为2卡训练脚本。可参见以下修改方法：
>   1. 若8卡脚本的batch\_size是单卡脚本的batch\_size的8倍，则将8卡训练时的batch\_size和learning\_rate同时除以4，作为2卡训练时的batch\_size和learning\_rate。
>   2. 将nproc\_per\_node修改为2。
> - 一个device对应执行一个训练进程，当前不支持多进程在同一个device上进行训练。

## 单机多卡示例

以torchrun方式启动为例，通过一个简单的自定义模型，展示单机八卡的模型代码和启动脚本样例。

1. 模型脚本配置示例，以下示例以一个简单的自定义模型为例，在适配NPU时需要注意和修改的内容已加注释。<a id="custom-anchor"></a>

    ```python
    import argparse
    import os
    from datetime import timedelta
    
    import torch
    import torch.distributed as dist
    import torch.nn as nn
    import torch.optim as optim
    # 导入DDP模块，用于分布式数据并行
    from torch.nn.parallel import DistributedDataParallel as DDP
    from torch.utils.data import DataLoader
    # 导入DistributedSampler模块，用于分布式训练数据采样
    from torch.utils.data.distributed import DistributedSampler
    import torch_npu
    from torchvision import datasets, transforms
    
    DATA_DIR = "./data"
    
    
    def cleanup():
        # 销毁进程组
        dist.destroy_process_group()
    
    
    class ToyModel(nn.Module):
        def __init__(self):
            super(ToyModel, self).__init__()
            self.layer1 = nn.Linear(784, 128)
            self.relu = nn.ReLU()
            self.layer2 = nn.Linear(128, 10)
    
        def forward(self, x):
            return self.layer2(self.relu(self.layer1(x)))
    
    
    def data_process(inputs, labels):
        inputs = inputs.view(-1, 784)
        labels = labels.view(-1)
        return inputs, labels
    
    
    def get_train_args():
        parser = argparse.ArgumentParser(description="command line arguments")
        parser.add_argument("--batch_size", type=int, default=64)
        parser.add_argument("--epochs", type=int, default=1)
        parser.add_argument("--learning_rate", type=float, default=0.0001)
        args = parser.parse_args()
        return args
    
    
    def train(args):
        # torchrun启动单机多卡训练时，通过环境变量获取参与训练的NPU总卡数world_size和当前进程所在NPU卡序号local_rank_idx
        world_size = int(os.environ["WORLD_SIZE"])
        local_rank_idx = int(os.environ["LOCAL_RANK"])
    
        devices_per_node = torch.npu.device_count()
    
        # 初始化进程组，将通信方式设置为hccl
        dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
    
        # 设定当前进程所在NPU卡序号
        torch_npu.npu.set_device(local_rank_idx)
    
        # 将模型转移到对应NPU卡上
        device_id = f"npu:{local_rank_idx}"
        model = ToyModel().to(device_id)
    
        transform = transforms.Compose([
            transforms.ToTensor(),
            transforms.Normalize((0.5,), (0.5,))
        ])
    
        # 为数据集准备分布式sampler和dataloader
        train_dataset = datasets.MNIST(root=DATA_DIR, train=True, download=True, transform=transform)
        train_sampler = DistributedSampler(train_dataset, num_replicas=devices_per_node, rank=local_rank_idx)
        train_dataloader = DataLoader(train_dataset, batch_size=args.batch_size, sampler=train_sampler)
    
        # 将交叉熵损失函数算子转移到对应NPU卡上
        criterion = nn.CrossEntropyLoss().to(device_id)
        optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)
    
        # 将模型封装为分布式数据并行训练状态
        model = DDP(model)
    
        for epoch in range(args.epochs):
            if local_rank_idx == 0:
                print(f"\nCurrent epoch: {epoch}")
    
            train_sampler.set_epoch(epoch)
    
            model.train()
    
            for i, (inputs, labels) in enumerate(train_dataloader):
                inputs, labels = data_process(inputs, labels)
                # 将数据转移到对应NPU卡上
                inputs, labels = inputs.to(device_id), labels.to(device_id)
    
                outputs = model(inputs)
                loss = criterion(outputs, labels)
    
                optimizer.zero_grad()
                loss.backward()
                optimizer.step()
    
                if local_rank_idx == 0:
                    print(f"Current step: {i}, loss: {loss.item()}")
    
        cleanup()
    
    
    def main():
        args = get_train_args()
        train(args)
    
    
    if __name__ == "__main__":
        main()
    ```

2. 启动脚本的配置样例，通过bash启动即可拉起训练：
    1. 新建启动脚本文件，例如`train_8p_torchrun.py`，将[1](#custom-anchor)的示例代码放置其中。
    2. 在bash执行如下指令启动：

        ```bash
        torchrun --nproc_per_node=8 --master_addr localhost --master_port 12345 train_8p_torchrun.py
        ```
