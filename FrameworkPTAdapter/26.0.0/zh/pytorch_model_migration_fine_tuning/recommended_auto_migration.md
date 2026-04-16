# （推荐）自动迁移

## 简介

自动迁移操作简单，且修改内容少，只需在训练脚本中添加from torch_npu.contrib import transfer_to_npu即可完成脚本迁移。

## 使用约束

- 当前自动迁移暂不支持channel\_last特性，建议用户使用contiguous代替。

- 若原脚本中使用的backend为nccl，在init\_process\_group初始化进程组后，backend会被自动迁移工具替换为hccl。如果后续代码逻辑包含backend是否为nccl的判断，例如assert backend in ['gloo', 'nccl']、if backend == 'nccl'，请手动将字符串nccl改写为hccl。

- 由于自动迁移工具使用了Python的动态特性，但torch.jit.script不支持Python的动态语法，因此用户原训练脚本中包含torch.jit.script时使用自动迁移功能会产生冲突，目前自动迁移时会屏蔽torch.jit.script功能，若用户脚本中必须使用torch.jit.script功能，请使用[工具迁移](tool_migration.md)进行迁移。

## 使用方法

1. 在训练脚本中添加from torch_npu.contrib import transfer_to_npu迁移代码。

    > [!NOTE]
    >
    > - 仅PyTorch框架下需要添加from torch_npu.contrib import transfer_to_npu代码。
    > - 自动迁移工具与已适配的《[套件与三方库支持清单](../supported_suites_and_third_party_libraries/supported_suites_and_third_party_libraries.md)》可能存在功能冲突，若发生冲突，请使用[工具迁移](tool_migration.md)。

    - PyTorch 2.4.0及之前版本，添加如下迁移代码：

        ```python
        import torch 
        import torch_npu 
        ... 
        from torch_npu.contrib import transfer_to_npu
        ```

    - PyTorch 2.5.1及之后版本，添加如下迁移代码：

        ```python
        import torch 
        ... 
        from torch_npu.contrib import transfer_to_npu
        ```

        > [!NOTE]
        >
        > PyTorch 2.5.1及之后版本支持设备插件自动加载，无需执行import torch_npu即可使用NPU设备，具体使用方法与限制说明可参见[Autoloading Out-of-Tree Extension](https://docs.pytorch.org/tutorials/unstable/python_extension_autoload.html)。

2. 参考[模型训练](model_training.md)执行训练。如果训练正常进行，开始打印迭代日志，说明训练功能迁移成功。
3. 参考[模型保存与导出](model_saving_and_exporting.md)，如果成功保存权重，说明保存权重功能迁移成功。

## 注意事项

- 如果模型包含评估、在线推理功能，也可在对应脚本中导入自动迁移库代码，并通过对比评估推理结果和日志打印情况，判断与GPU/CPU是否一致，决定迁移是否成功。
- 若训练过程中提示部分CUDA接口报错，可能是部分API（算子API或框架API）不支持引起，用户可参考以下方案进行解决。

    使用PyTorch Analyse工具对模型脚本进行分析，获得支持情况存疑的API列表，进入[昇腾开源社区](https://gitcode.com/Ascend/pytorch)提出ISSUE求助；算子请参考《PyTorch 框架特性指南》中的“[自定义算子适配开发](https://gitcode.com/Ascend/pytorch/blob/v2.7.1-26.0.0/docs/zh/framework_feature_guide_pytorch/adaptation_overview_opplugin.md)”章节进行适配。
