# 适配样例（多模型、损失函数和优化器场景）

多模型、损失函数和优化器场景是指在神经网络中同时存在多个模型、多个损失函数和优化器的场景。以下示例以一个简单的自定义模型为例介绍如何适配NPU。

## 导入AMP模块

导入AMP模块，定义两个简单的神经网络。

```python
import argparse

import torch
import torch_npu
import torch.nn as nn
import torch.optim as optim

from torch.utils.data import DataLoader
from torchvision import datasets, transforms
from torch_npu.npu import amp

# 定义一个简单的神经网络
class ToyModel1(nn.Module):
    def __init__(self):
        super(ToyModel1, self).__init__()
        self.layer1 = nn.Linear(10, 10)
        self.relu = nn.ReLU()
        self.layer2 = nn.Linear(10, 5)

    def forward(self, x):
        return self.layer2(self.relu(self.layer1(x)))

# 定义第二个相似的神经网络，增加一层Linear层。
class ToyModel2(nn.Module):
    def __init__(self):
        super(ToyModel2, self).__init__()
        self.layer1 = nn.Linear(10, 10)
        self.relu1 = nn.ReLU()
        self.layer2 = nn.Linear(10, 10)
        self.relu2 = nn.ReLU()
        self.layer3 = nn.Linear(10, 5)

    def forward(self, x):
        return self.layer3(self.relu2(self.layer2(self.relu1(self.layer1(x)))))
```

## 定义损失缩放参数

在模型、优化器定义之后，定义AMP功能中的GradScaler。

```python
args = parse_args()
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.5,), (0.5,))
])
train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform = transform)   # 数据集获取 
train_dataloader = DataLoader(train_dataset, batch_size=args.batch_size, shuffle=True)    # 定义DataLoader

device = torch.device("npu")
model1 = ToyModel1().to(device)    # 把模型放到指定NPU上
model2 = ToyModel2().to(device)    # 把模型放到指定NPU上
criterion = nn.CrossEntropyLoss()     # 定义损失函数
optimizer1 = optim.Adam(model1.parameters(), lr=args.learning_rate)    # 定义优化器
optimizer2 = optim.Adam(model2.parameters(), lr=args.learning_rate)    # 定义优化器

scaler = amp.GradScaler()    # 在模型、优化器定义之后，定义GradScaler
```

## 适配AMP并训练

在训练代码中添加AMP功能相关的代码开启AMP，对多个损失函数和优化器进行计算。

```python
for epoch in range(args.epochs):
model1.train()
model2.train()
    for inputs, labels in train_dataloader:
        inputs, labels = data_process(inputs, labels)
        inputs, labels = inputs.to(device), labels.to(device)

        with amp.autocast():  # 设置amp
            outputs1 = model1(inputs)  # 前向计算
            outputs2 = model2(inputs) # 前向计算
            loss1 = criterion(outputs1, labels).to(device)  # 损失函数计算 
            loss2 = criterion(outputs2, labels).to(device) # 损失函数计算

        optimizer1.zero_grad()
        optimizer2.zero_grad()

        # 进行反向传播前后的loss缩放、参数更新
        scaler.scale(loss1).backward(retain_graph=True)  # loss缩放并反向传播
        scaler.scale(loss2).backward()
        scaler.step(optimizer1)  # 更新参数（自动unscaling）
        scaler.step(optimizer2) # 更新参数（自动unscaling）
         scaler.update()  # 基于动态Loss Scale更新loss_scaling系数
```

执行命令拉起训练，端到端模型脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/train_with_multi_amp.py)，启动脚本示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/test/run_with_multi_amp.sh)。
