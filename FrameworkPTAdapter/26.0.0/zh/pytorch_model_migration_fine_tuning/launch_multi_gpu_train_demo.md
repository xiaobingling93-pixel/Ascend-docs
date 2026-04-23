# 拉起多卡训练脚本示例

本章节以单机八卡torchrun方式模型脚本为示例，介绍了单机八卡和双机16卡其余拉起方式代码需要适配的操作供用户学习和参考。

## 模型脚本示例<a id="custom-anchor"></a>

单机八卡torchrun方式模型脚本示例：

```python
import argparse
import os
from datetime import timedelta

import torch
import torch.distributed as dist
import torch.nn as nn
import torch.optim as optim
from torch.nn.parallel import DistributedDataParallel as DDP
from torch.utils.data import DataLoader
from torch.utils.data.distributed import DistributedSampler
import torch_npu
from torchvision import datasets, transforms

DATA_DIR = "./data"  # 数据集目录


def cleanup():
    """清理分布式训练的进程组"""
    dist.destroy_process_group()


class ToyModel(nn.Module):
    """简单的神经网络模型"""
    def __init__(self):
        super(ToyModel, self).__init__()
        self.layer1 = nn.Linear(784, 128)  # 第一层全连接层
        self.relu = nn.ReLU()  # ReLU 激活函数
        self.layer2 = nn.Linear(128, 10)  # 第二层全连接层

    def forward(self, x):
        """前向传播函数"""
        return self.layer2(self.relu(self.layer1(x)))  # 输入经过两层全连接层和一个 ReLU 激活函数


def data_process(inputs, labels):
    """数据预处理函数"""
    inputs = inputs.view(-1, 784)  # 将输入张量展平为 （batch_size, 784）
    labels = labels.view(-1)  # 将标签张量展平为 （batch_size）
    return inputs, labels


def get_train_args():
    """获取训练参数"""
    parser = argparse.ArgumentParser(description="command line arguments")  # 创建参数解析器
    parser.add_argument("--batch_size", type=int, default=64)  # 设置批大小，默认为 64
    parser.add_argument("--epochs", type=int, default=1)  # 设置训练轮数，默认为 1
    parser.add_argument("--learning_rate", type=float, default=0.0001)  # 设置学习率，默认为 0.0001
    args = parser.parse_args()  # 解析命令行参数
    return args


def train(args):
    """训练函数"""
    world_size = int(os.environ["WORLD_SIZE"])  # 获取分布式训练的进程数
    local_rank_idx = int(os.environ["LOCAL_RANK"])  # 获取本地进程索引

    devices_per_node = torch.npu.device_count()  # 获取每个节点的 NPU 设备数量

    dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))  # 初始化分布式进程组
    torch_npu.npu.set_device(local_rank_idx)  # 设置当前使用的 NPU 设备

    device_id = f"npu:{local_rank_idx}"  # NPU 设备标识

    model = ToyModel().to(device_id)  # 实例化模型并移动到指定设备

    transform = transforms.Compose([
        transforms.ToTensor(),  # 将图像转换为张量
        transforms.Normalize((0.5,), (0.5,))  # 归一化
    ])

    train_dataset = datasets.MNIST(root=DATA_DIR, train=True, download=True, transform=transform)  # 加载 MNIST 训练数据集
    train_sampler = DistributedSampler(train_dataset, num_replicas=devices_per_node, rank=local_rank_idx)  # 创建分布式数据采样器
    train_dataloader = DataLoader(train_dataset, batch_size=args.batch_size, sampler=train_sampler)  # 创建数据加载器

    criterion = nn.CrossEntropyLoss().to(device_id)  # 定义损失函数
    optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)  # 定义优化器

    model = DDP(model)  # 包装模型以支持分布式训练

    for epoch in range(args.epochs):
        if local_rank_idx == 0:
            print(f"\nCurrent epoch: {epoch}")  # 打印当前轮数

        train_sampler.set_epoch(epoch)  # 设置数据采样器的轮数

        model.train()  # 设置模型为训练模式

        for i, (inputs, labels) in enumerate(train_dataloader):
            inputs, labels = data_process(inputs, labels)  # 预处理数据
            inputs, labels = inputs.to(device_id), labels.to(device_id)  # 将数据移动到指定设备

            outputs = model(inputs)  # 模型前向传播
            loss = criterion(outputs, labels).to(device_id)  # 计算损失

            optimizer.zero_grad()  # 清除梯度
            loss.backward()  # 反向传播
            optimizer.step()  # 更新参数

            if local_rank_idx == 0:
                print(f"Current step: {i}, loss: {loss.item()}")  # 打印当前步数和损失值

    cleanup()  # 清理分布式训练的进程组


def main():
    """主函数"""
    args = get_train_args()  # 获取训练参数
    train(args)  # 开始训练


if __name__ == "__main__":
    main()
```

更多场景和拉起方式的端到端示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/tree/master/PyTorch/built-in/docs/dist_train_demo)。

## 拉起单机八卡训练（shell脚本方式）<a id="suctom-anchor01"></a>

1. 修改模型脚本，脚本名称为`train_8p_shell.py`，`train_8p_shell.py`仅为示例。
    1. 修改train\(\)函数。
        1. 修改如下代码。

            修改前：

            ```python
            train(args):
            ```

            修改后：

            ```python
            train(local_rank, world_size, args):
            ```

        2. 删除如下代码。

            ```python
            world_size = int(os.environ["WORLD_SIZE"])
            ```

        3. 修改如下代码。

            修改前：

            ```python
            local_rank_idx = int(os.environ["LOCAL_RANK"])
            ```

            修改后：

            ```python
            local_rank_idx = local_rank
            ```

    2. 修改main\(\)函数。

        修改前：

        ```python
        def main():
            args = get_train_args()
            train(args)
        ```

        修改后：

        ```python
        def main():
            args = get_train_args()
            world_size = int(os.environ["WORLD_SIZE"])  # 新增
            local_rank = int(os.environ["LOCAL_RANK"])  # 新增
            train(local_rank, world_size, args)  # 修改  
        ```

2. 新建shell训练脚本，脚本名称为`start_train_8p.sh`，`start_train_8p.sh`仅为示例。

    ```shell
    #!/bin/bash
    
    # 设置环境变量
    export MASTER_ADDR="localhost"
    export MASTER_PORT="12345"
    export WORLD_SIZE=8  # 总的NPU数量
    
    # 启动多个进程
    for ((local_rank=0; local_rank<$WORLD_SIZE; local_rank++))
    do
        export LOCAL_RANK=$local_rank
        python train_8p_shell.py &
    done
    
    wait
    ```

3. 运行训练脚本。

    ```shell
    bash start_train_8p.sh   # 根据实际命名进行修改
    ```

## 拉起单机八卡训练（mp.spawn方式）<a id="suctom-anchor02"></a>

1. 修改模型脚本，脚本名称为`train_8p_spawn.py`，`train_8p_spawn.py`仅为示例。
    1. 导入torch.multiprocessing。

        ```python
        import torch.multiprocessing as mp
        ```

    2. 修改train\(\)函数。
        1. 修改如下代码。

            修改前：

            ```python
            train(args):
            ```

            修改后：

            ```python
            train(local_rank, world_size, args):
            ```

        2. 删除如下代码。

            ```python
            world_size = int(os.environ["WORLD_SIZE"])
            ```

        3. 修改如下代码。

            修改前：

            ```python
            local_rank_idx = int(os.environ["LOCAL_RANK"])
            ```

            修改后：

            ```python
            local_rank_idx = local_rank
            ```

    3. 修改main\(\)函数。

        ```python
        def main():
            args = get_train_args()
            world_size = torch.npu.device_count()  # 新增
            mp.spawn(train, args=(world_size, args), nprocs=world_size)  # 新增
        ```

2. 运行训练脚本。

    ```shell
    export MASTER_ADDR=xxxx  # 将xxxx改为本机IP地址
    export MASTER_PORT=12345
    python train_8p_spawn.py
    ```

## 拉起单机八卡训练（torch.distributed.launch方式）<a id="suctom-anchor03"></a>

1. 修改模型脚本，脚本名称为`train_8p_torch_distributed_launch.py`，`train_8p_torch_distributed_launch.py`仅为示例。
    1. 修改get\_train\_args\(\)函数。

        ```python
        parser.add_argument("--local-rank", type=int, default=0)  # 新增    
        ```

    2. 修改train\(\)函数。

        修改前：

        ```python
        local_rank_idx = int(os.environ["LOCAL_RANK"])
        ```

        修改后：

        ```python
        local_rank_idx = args.local_rank
        ```

2. 运行训练脚本。

    ```shell
    python -m torch.distributed.launch --nproc_per_node 8 --master_addr localhost --master_port 12345 train_8p_torch_distributed_launch.py
    ```

## 拉起单机八卡训练（torchrun方式）<a id="suctom-anchor04"></a>

1. [模型脚本示例](#custom-anchor)以torchrun方式为例，模型脚本无需修改。脚本名称为`train_8p_torchrun.py`，`train_8p_torchrun.py`仅为示例。
2. 运行训练脚本。

    ```shell
    torchrun --nproc_per_node 8 --master_addr localhost --master_port 12345 train_8p_torchrun.py
    ```

## 拉起单机八卡训练（torch\_npu\_run方式）<a id="suctom-anchor05"></a>

1. 修改模型脚本，脚本名称为`train_8p_torch_npu_run.py`，`train_8p_torch_npu_run.py`仅为示例。
    1. 修改get\_train\_args\(\)函数。

        ```python
        parser.add_argument("--master_addr", type=str, default="xxxx")  # 新增，将xxxx改为本机IP地址   
        parser.add_argument("--master_port", type=str, default="12345")  # 新增 
        ```

    2. 修改train\(\)函数。

        修改前：

        ```python
        dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
        ```

        修改后：

        ```python
        dist.init_process_group("hccl", init_method=f"parallel://{args.master_addr}:{args.master_port}", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
        ```

2. 运行训练脚本。

    ```shell
    torch_npu_run --rdzv_backend parallel --master_addr xxxx --master_port 12345 --nproc_per_node 8 train_8p_torch_npu_run.py  # 将xxxx改为本机IP地址
    ```

## 拉起双机16卡训练（shell脚本方式）<a id="suctom-anchor06"></a>

1. 修改模型脚本，脚本名称为`train_16p_shell.py`，`train_16p_shell.py`仅为示例。
    1. 修改train\(\)函数。
        1. 新增如下代码。

            ```python
            global_rank_idx = int(os.environ["RANK"])  # 新增 
            ```

        2. 修改如下代码。

            修改前：

            ```python
            dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

            修改后：

            ```python
            dist.init_process_group("hccl", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

2. 新建shell训练脚本。
    1. 主节点训练脚本，脚本名称为`start_train_16p_master_node.sh`，`start_train_16p_master_node.sh`仅为示例。

        ```shell
        #!/bin/bash
        
        # 设置环境变量
        export MASTER_ADDR="xxxx"  # 将xxxx改为主节点IP地址
        export MASTER_PORT="12345"
        export WORLD_SIZE=16
        export NODE_RANK=0
        
        # 启动多个进程
        for ((local_rank=0; local_rank<8; local_rank++))
        do
            export RANK=$((NODE_RANK * 8 + local_rank))
            export LOCAL_RANK=$local_rank
            python train_16p_shell.py &
        
        done
        
        wait
        ```

    2. 从节点训练脚本，脚本名称为`start_train_16p_slave_node.sh`，`start_train_16p_slave_node.sh`仅为示例。

        ```shell
        #!/bin/bash
        
        # 设置环境变量
        export MASTER_ADDR="xxxx"  # 将xxxx改为主节点IP地址
        export MASTER_PORT="12345"
        export WORLD_SIZE=16
        export NODE_RANK=1
        
        # 启动多个进程
        for ((local_rank=0; local_rank<8; local_rank++))
        do
            export RANK=$((NODE_RANK * 8 + local_rank))
            export LOCAL_RANK=$local_rank
            python train_16p_shell.py &
        
        done
        
        wait
        ```

3. 运行训练脚本。
    1. 主节点：

        ```shell
        bash start_train_16p_master_node.sh  # 根据实际命名进行修改
        ```

    2. 从节点：

        ```shell
        bash start_train_16p_slave_node.sh  # 根据实际命名进行修改
        ```

## 拉起双机16卡训练（mp.spawn方式）<a id="suctom-anchor07"></a>

1. 修改模型脚本，脚本名称为`train_16p_spawn.py`，`train_16p_spawn.py`仅为示例。
    1. 导入torch.multiprocessing。

        ```python
        import torch.multiprocessing as mp
        ```

    2. 修改get\_train\_args\(\)函数。

        ```python
        parser.add_argument("--node_rank", type=int, default=0)  # 新增
        parser.add_argument("--nnodes", type=int, default=-1)  # 新增
        ```

    3. 修改train\(\)函数。
        1. 修改如下代码。

            修改前：

            ```python
            train(args):
            ```

            修改后：

            ```python
            train(local_rank, world_size, args):
            ```

        2. 删除如下代码。

            ```python
            world_size = int(os.environ["WORLD_SIZE"])
            ```

        3. 修改如下代码：

            修改前：

            ```python
            local_rank_idx = int(os.environ["LOCAL_RANK"])
            ```

            修改后：

            ```python
            local_rank_idx = local_rank
            ```

        4. 添加如下代码。

            ```python
            global_rank_idx = args.node_rank * torch.npu.device_count() + local_rank
            ```

        5. 修改如下代码。

            修改前：

            ```python
            dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

            修改后：

            ```python
            dist.init_process_group("hccl", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

    4. 修改main\(\)函数。

        ```python
        def main():
            args = get_train_args()
            world_size = torch.npu.device_count() * args.nnodes  # 新增
            mp.spawn(train, args=(world_size, args), nprocs=torch.npu.device_count())  # 新增
        ```

2. 运行训练脚本。
    1. 主节点：

        ```shell
        export MASTER_ADDR=xxxx   # 将xxxx改为主节点IP地址
        export MASTER_PORT=12345
        python train_16p_spawn.py --nnodes 2 --node_rank 0 
        ```

    2. 从节点：

        ```shell
        export MASTER_ADDR=xxxx   # 将xxxx改为主节点IP地址
        export MASTER_PORT=12345
        python train_16p_spawn.py --nnodes 2 --node_rank 1
        ```

## 拉起双机16卡训练（torch.distributed.launch方式）<a id="suctom-anchor08"></a>

1. 修改模型脚本，脚本名称为`train_16p_python.py`，`train_16p_python.py`仅为示例。
    1. 修改get\_train\_args\(\)函数。

        ```python
        parser.add_argument("--local-rank", type=int, default=0)  # 新增    
        ```

    2. 修改train\(\)函数。
        1. 修改如下代码。

            修改前：

            ```python
            local_rank_idx = int(os.environ["LOCAL_RANK"])
            ```

            修改后：

            ```python
            local_rank_idx = args.local_rank
            ```

        2. 添加如下代码。

            ```python
            global_rank_idx = int(os.environ["RANK"])
            ```

        3. 修改如下代码。

            修改前：

            ```python
            dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

            修改后：

            ```python
            dist.init_process_group("hccl", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

2. 运行训练脚本。
    1. 主节点：

        ```shell
        # 将xxxx改为主节点IP地址
        python -m torch.distributed.launch --nnodes 2 --nproc_per_node 8 --node_rank 0 --master_addr xxxx --master_port 12345 train_16p_python.py  
        ```

    2. 从节点：

        ```shell
        # 将xxxx改为主节点IP地址
        python -m torch.distributed.launch --nnodes 2 --nproc_per_node 8 --node_rank 1 --master_addr xxxx --master_port 12345 train_16p_python.py
        ```

## 拉起双机16卡训练（torchrun方式）<a id="suctom-anchor09"></a>

1. 修改模型脚本，脚本名称为`train_16p_torchrun.py`，`train_16p_torchrun.py`仅为示例。
    1. 修改train\(\)函数。
        1. 添加如下代码。

            ```python
            global_rank_idx = int(os.environ["RANK"])
            ```

        2. 修改如下代码。

            修改前：

            ```python
            dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

            修改后：

            ```python
            dist.init_process_group("hccl", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

2. 运行训练脚本。
    1. 主节点：

        ```shell
        # 将xxxx改为主节点IP地址
        torchrun --nnodes 2 --nproc_per_node 8 --node_rank 0 --master_addr xxxx --master_port 12345 train_16p_torchrun.py  
        ```

    2. 从节点：

        ```shell
        # 将xxxx改为主节点IP地址
        torchrun --nnodes 2 --nproc_per_node 8 --node_rank 1 --master_addr xxxx --master_port 12345 train_16p_torchrun.py
        ```

## 拉起双机16卡训练（torch\_npu\_run方式）<a id="suctom-anchor11"></a>

1. 修改模型脚本，脚本名称为`train_16p_torch_npu_run.py`，`train_16p_torch_npu_run.py`仅为示例。
    1. 修改get\_train\_args\(\)函数。

        ```python
        parser.add_argument("--master_addr", type=str, default=None)  # 新增，将xxxx改为主节点IP地址
        parser.add_argument("--master_port", type=str, default="12345")  # 新增 
        ```

    2. 修改train\(\)函数。
        1. 添加如下代码。

            ```python
            global_rank_idx = int(os.environ["RANK"])
            ```

        2. 修改如下代码。

            修改前：

            ```python
            dist.init_process_group("hccl", rank=local_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

            修改后：

            ```python
            dist.init_process_group("hccl", init_method=f"parallel://{args.master_addr}:{args.master_port}", rank=global_rank_idx, world_size=world_size, timeout=timedelta(minutes=30))
            ```

2. 运行训练脚本。
    1. 主节点：

        ```shell
        # 将xxxx改为主节点IP地址
        torch_npu_run --rdzv_backend parallel --master_addr xxxx --master_port 12345 --nnodes 2 --node_rank 0 --nproc_per_node 8 train_16p_torch_npu_run.py  
        ```

    2. 从节点：

        ```shell
        # 将xxxx改为主节点IP地址
        torch_npu_run --rdzv_backend parallel --master_addr xxxx --master_port 12345 --nnodes 2 --node_rank 1 --nproc_per_node 8 train_16p_torch_npu_run.py
        ```

当屏幕打印/定向日志中出现模型加载、训练等正常运行日志时，说明拉起训练成功。
