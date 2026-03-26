# 保存模型

PyTorch在训练过程中，通常使用torch.save\(\)来保存Checkpoint文件，根据模型文件的后续用途会保存为两种格式的模型文件（pth文件和pth.tar文件），以便用于在线推理。

-   .pth或.pt扩展名的文件：用于在线推理或导出ONNX格式模型。仅保存模型参数，不保存模型结构，以便压缩文件的体积，可以用Netron等可视化工具打开，样例如图1所示。

    **图 1** .pth文件  
    ![](./figures/save_model_fig_01.png)

    通过state\_dict来保存和加载模型。

    保存模型，示例如下，端到端示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/train_1p.py#L75)：

    ```python
    # 创建保存路径
    save_pt_path = "state_dict_model.pt"
    # 保存模型
    torch.save(model.state_dict(), save_pt_path) # model为前面训练定义的模型变量
    ```

    加载模型以用于在线推理，示例如下：

    ```python
    # 模型文件保存路径
    save_pt_path = "state_dict_model.pt"
    
    model = TheModelClass(*args, **kwargs)        #根据实际模型定义填写函数和参数
    # 以模型脚本与启动脚本配置中介绍的简单模型为例: model = ToyModel()
    
    # 加载模型
    model.load_state_dict(torch.load(save_pt_path))
    model.eval()
    ```

    > [!NOTE]
    >
    > 保存为后缀是.pth/.pt的文件时，需要提供模型定义文件，否则后续模型无法部署。

-   .pth.tar扩展名的文件：可用于在线推理或重新加载后继续训练。文件中保存了多个组件，以字典形式保存。常见的组件包括模型和优化器的state\_dict、停止时的epoch、最新记录的训练损失以及外部的torch.nn.Embedding层等。如果仅用于部署推理模型，推荐只在.pth.tar扩展名的文件中保存权重信息即模型的state\_dict。

    保存模型，示例如下，端到端示例可参考[LINK](https://gitee.com/ascend/ModelZoo-PyTorch/blob/master/PyTorch/contrib/others/ToyModel_for_PyTorch/train_1p.py#L67)：

    ```python
    checkpoint_path = "checkpoint.pth.tar"
    torch.save({
        'epoch': epoch,
        'loss': loss,
        'state_dict': model.state_dict(),
        'optimizer' : optimizer.state_dict(),
        ...
    }, checkpoint_path)
    ```

    加载模型用于推理或恢复训练，示例如下：

    ```python
    model = TheModelClass(*args, **kwargs)            #根据实际模型定义填写函数和参数
    optimizer = TheOptimizerClass(*args, **kwargs)    #根据实际优化器填写函数和参数
    
    checkpoint = torch.load(checkpoint_path)
    model.load_state_dict(checkpoint['state_dict'])
    optimizer.load_state_dict(checkpoint['optimizer'])
    epoch = checkpoint['epoch']
    loss = checkpoint['loss']
    
    model.eval()
    # - or -
    model.train()
    ```

    > [!NOTICE]
    >
    > 通常情况下，训练图和推理图中对同一个算子处理方式不同（例如BatchNorm和dropout等算子），在输入格式上也有差别。因此在运行推理或导出ONNX模型之前，必须调用model.eval()将dropout和batch normalization层设置为推理模式。

