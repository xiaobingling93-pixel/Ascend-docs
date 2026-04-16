# 单机单卡场景

## 环境准备

1. 请参见《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（商用版）或《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（社区版）手册，安装硬件与OS，NPU驱动和固件以及CANN软件；请参见《[Ascend Extension for PyTorch 软件安装指南](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/installation_guide/installation_description.md)》手册，安装PyTorch框架、torch\_npu插件以及APEX模块（可选）。
2. 根据实际需求准备模型环境，如conda、docker以及三方库依赖。

## 模型脚本配置

- **GPU转NPU**

    通常模型源码支持GPU单机单卡训练，用户根据[模型迁移](model_migration.md)章节的指导，将模型源码映射至NPU，即可完成模型脚本配置。以下示例以一个简单的自定义模型为例，适配NPU需要注意和修改的内容已注释。

    ```python
    import argparse
    
    import torch
    import torch_npu
    import torch.nn as nn
    import torch.optim as optim
    
    from torch.utils.data import DataLoader
    from torchvision import datasets, transforms
    
    
    class ToyModel(nn.Module):
        def __init__(self):
            super(ToyModel, self).__init__()
            self.net1 = nn.Linear(10, 10)
            self.relu = nn.ReLU()
            self.net2 = nn.Linear(10, 5)
    
        def forward(self, x):
            return self.net2(self.relu(self.net1(x)))
    
    def parse_args():
        parser = argparse.ArgumentParser(description="command line arguments")
        parser.add_argument('--batch_size', type=int, default=64)
        parser.add_argument('--epochs', type=int, default=10)
        parser.add_argument('--learning_rate', type=float, default=0.0001)
        return parser.parse_args()
    
    def data_process(inputs, labels):
        squeezed_tensor = inputs.squeeze(0).squeeze(0)
        inputs = squeezed_tensor[:, :10]
        labels = labels.repeat(28, 5) * (1/140)
        return inputs, labels
    
    
    def main():
        # 获取传参
        args = parse_args() 
        transform = transforms.Compose([
            transforms.ToTensor(),
            transforms.Normalize((0.5,), (0.5,))
        ])
        train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform=transform)
        # 加载数据集
        train_loader = DataLoader(train_dataset, batch_size=args.batch_size, shuffle=True) 
        # 将模型转移到NPU上
        device = torch.device("npu")
        model = ToyModel().to(device)
    
        # 定义损失函数
        criterion = nn.CrossEntropyLoss() 
        optimizer = optim.Adam(model.parameters(), lr=args.learning_rate)
        step = 0
    
        for epoch in range(args.epochs):
            model.train()
            for inputs, labels in train_loader:
                # 数据预处理，将数据集的数据转成需要的shape
                inputs, labels = data_process(inputs, labels)   
                # 将数据转到NPU处理
                inputs, labels = inputs.to(device), labels.to(device) 
    
                optimizer.zero_grad()
                outputs = model(inputs)
                # 将loss转到NPU处理
                loss = criterion(outputs, labels).to(device) 
                loss.backward()
                optimizer.step()
                # 添加每个step的打印，用户可自行修改
                print(f"step == {step}") 
                step += 1
    
    if __name__ == "__main__":
        main()
    ```

- **CPU转NPU**

    如果模型源码仅使用了CPU编程，不支持GPU单机单卡训练，用户可使用如下命令，将数据、模型放置NPU上训练。

    ```python
    import torch
    import torch_npu
    
    device = torch.device("npu")
    # 将数据加载至npu
    for data in dataloader:
      if torch.npu.is_available():
           data.to(device)
    
    # 将模型加载至npu
    model = Model()
    if torch.npu.is_available():
         model.to(device)
    
    # 将损失函数加载至npu
    loss_fn = nn.CrossEntropyLoss()
    if torch.npu.is_available():
         loss_fn=loss_fn.to(device)
    ```

## 拉起训练

执行如下命令拉起单机训练，以下参数为举例，用户可根据实际情况自行改动。

- **GPU转NPU**

    ```shell
    python3 main.py \               # main.py为上述模型脚本配置示例代码名称，用户可自行定义
          --learning_rate 0.0001 \  # 学习率
          --batch_size 1 \          # 训练批次大小
          --epochs 1                # 训练迭代轮数
    ```

- **CPU转NPU**

    ```shell
    python3 main.py \                                             # main.py为上述模型脚本配置示例代码名称，用户可自行定义
                      --batch_size 128 \                          # 训练批次大小 
                      --data_path /home/data/resnet50/imagenet \  # 数据集路径
                      --lr 0.1 \                                  # 学习率 
                      --epochs 90 \                               # 训练迭代轮数 
                      --arch resnet50 \                           # 模型架构 
                      --world_size 1 \                            # NPU数量
                      --rank 0 \                                  # 本节点编号
                      --workers 40 \                              # 加载数据进程数 
                      --momentum 0.9 \                            # 动量   
                      --weight_decay 1e-4 \                       # 权重衰减 
                      --gpu 0                                     # device号, 这里参数名称仍为gpu, 但迁移完成后实际训练设备已在代码中定义为npu
    ```
