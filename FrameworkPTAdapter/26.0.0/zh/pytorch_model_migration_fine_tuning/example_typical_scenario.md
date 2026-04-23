# 适配样例（典型场景）

典型场景是开启混合精度进行一般模型训练的场景。以下示例以一个简单的自定义模型为例介绍如何适配NPU。

## 导入AMP模块

在构建神经网络前，导入AMP模块。

```python
# 引入模块 
import argparse

import torch
import torch_npu
import torch.nn as nn
import torch.optim as optim

from torch.utils.data import DataLoader
from torchvision import datasets, transforms
# 导入AMP模块
from torch_npu.npu import amp
```

## 定义损失缩放参数

在模型、优化器定义之后，定义AMP功能中的GradScaler。

```python
args = parse_args()
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5,), (0.5,))
])
train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform=transform)   # 数据集获取 
train_loader = DataLoader(train_dataset, batch_size=args.batch_size, shuffle=True)    # 定义DataLoader

device = torch.device("npu")
model = ToyModel().to(device)    # 把模型放到指定NPU上
criterion = nn.CrossEntropyLoss()     # 定义损失函数
optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)     # 定义优化器
scaler = amp.GradScaler()    # 在模型、优化器定义之后，定义GradScaler 

```

## 适配AMP并训练

在训练代码中添加AMP功能适配代码。最后运行整个脚本进行混合精度训练。

```python
for epoch in range(args.epochs):
    model.train()
    for inputs, labels in train_loader:
        inputs, labels = data_process(inputs, labels) # 数据预处理，将数据集的数据转成需要的shape
        inputs, labels = inputs.to(device), labels.to(device) # 将数据转到NPU处理

        with amp.autocast():   # 设置amp
            outputs = model(inputs)    # 前向计算
            loss = criterion(outputs, labels).to(device)  # 损失函数计算
        optimizer.zero_grad()
        # 进行反向传播前后的loss缩放、参数更新
        scaler.scale(loss).backward()    # loss缩放并反向传播
        scaler.step(optimizer)    # 更新参数（自动unscaling）
        scaler.update()    # 基于动态Loss Scale更新loss_scaling系数
```

执行命令拉起训练，端到端模型脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/train_1p_with_amp.py)，启动脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/test/run_1p_with_amp.sh)。
