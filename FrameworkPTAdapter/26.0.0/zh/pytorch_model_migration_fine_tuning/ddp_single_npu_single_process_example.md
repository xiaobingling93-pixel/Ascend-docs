# 适配样例（DDP单NPU单进程场景）

原生PyTorch框架的AMP（Automatic Mixed Precision，自动混合精度）支持3种多卡训练场景，NPU上适配的AMP（Automatic Mixed Precision，自动混合精度）当前仅支持DDP单NPU单进程多卡训练场景。除了对样例代码添加AMP相关的改动外，还需要对样例代码进行必要的使能多卡场景改动。以下示例以一个简单的自定义模型为例介绍如何适配NPU。

## 导入AMP模块

导入AMP模块。进行多卡训练的初始化。

```python
import os
import argparse
import tempfile

import torch
import torch_npu
import torch.distributed as dist
import torch.nn as nn
import torch.optim as optim

from torch.utils.data import DataLoader
from torchvision import datasets, transforms
from torch.utils.data.distributed import DistributedSampler
from torch.nn.parallel import DistributedDataParallel as DDP
from torch_npu.npu import amp  # 导入AMP模块
```

## 定义损失缩放参数

从torchvision中获取训练数据集，设置训练相关分布式超参数，定义模型，设置train\_sampler，定义优化器，定义AMP功能中的GradScaler。

```python
args = parse_args() # 获取分布式超参数
# 添加分布式逻辑
local_rank = int(os.environ.get("LOCAL_RANK", 0))
world_size = int(os.environ.get("WORLD_SIZE", 1))
dist.init_process_group("hccl", rank=(args.node_rank)*(args.nproc_per_node) + local_rank, world_size=world_size) # 将通信方式设置为hccl
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5,), (0.5,))
])
torch_npu.npu.set_device(local_rank)
device = torch.device(f'npu:{local_rank}')

model = ToyModel().to(device) # 把模型放到指定NPU上
model = DDP(model, device_ids=[local_rank], find_unused_parameters=True)   #开启DDP模式


train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform = transform)
train_sampler = DistributedSampler(train_dataset, num_replicas=world_size, rank=local_rank)
train_loader = DataLoader(train_dataset, batch_size=args.batch_size, sampler=train_sampler) # 定义DataLoader

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=args.learning_rate) # 定义优化器
scaler = amp.GradScaler() # 在模型、优化器定义之后，定义GradScaler
```

## 适配AMP并训练

在训练代码中添加AMP功能相关的代码开启AMP。

```python
for epoch in range(args.epochs):
    model.train()
    for inputs, labels in train_loader:
        inputs, labels = data_process(inputs, labels) # 数据预处理，将数据集的数据转成需要的shape
        inputs, labels = inputs.to(device), labels.to(device)
        with amp.autocast():   # 设置amp 
            outputs = model(inputs)
            loss = criterion(outputs, labels).to(device)
        optimizer.zero_grad()
        # 进行反向传播前后的loss缩放、参数更新 
        scaler.scale(loss).backward()  # loss缩放并反向传播 
        scaler.step(optimizer)  # 更新参数（自动unscaling）
        scaler.update()  # 基于动态Loss Scale更新loss_scaling系数
```

执行命令拉起多卡训练，端到端模型脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/tree/master/PyTorch/contrib/others/ToyModel_for_PyTorch/train_multi_with_amp.py)，启动脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/tree/master/PyTorch/contrib/others/ToyModel_for_PyTorch/test/run_16p_with_amp.sh)。
