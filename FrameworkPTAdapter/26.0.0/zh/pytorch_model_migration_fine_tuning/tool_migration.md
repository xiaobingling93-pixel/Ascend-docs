# 工具迁移

工具迁移支持使用命令行方式迁移。

## 使用约束

> [!NOTE]
>
> - 若用户训练脚本中包含NPU平台不支持的**amp\_C**模块，需要用户手动删除后再进行训练。
> - 由于转换后的脚本与原始脚本平台不一致，迁移后的脚本在调试运行过程中可能会由于算子差异等原因而抛出异常，导致进程终止，该类异常需要用户根据异常信息进一步调试解决。

## 使用方法

使用命令行方式进行工具迁移的核心步骤如下：

1. 环境准备。
    1. 安装开发套件Ascend-cann-toolkit。具体操作请参考《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（商用版）或《[CANN 软件安装指南](https://www.hiascend.com/document/detail/zh/CANNCommunityEdition/850/softwareinst/instg/instg_0000.html?Mode=PmIns&InstallType=netconda&OS=openEuler&Software=cannToolKit)》（社区版）。
    2. 须安装如下依赖（以root用户安装为例，非root用户需加--user参数）。

        ```shell
        pip3 install pandas         # pandas版本号需大于或等于1.2.4
        pip3 install libcst         # Python语法树解析器，用于解析Python文件
        pip3 install prettytable    # 将数据可视化为图表形式
        pip3 install jedi           # 用于跨文件解析，建议安装
        ```

2. 进入迁移工具所在路径。

    ```shell
    cd Ascend-cann-toolkit安装目录/cann/tools/ms_fmk_transplt/
    ```

3. 进入分析迁移工具所在路径，执行脚本迁移任务。

    ```shell
    ./pytorch_gpu2npu.sh -i 原始脚本路径 -o 脚本迁移结果输出路径 -v 原始脚本框架版本 [-s] [distributed -m 训练脚本的入口文件 -t 目标模型变量名]
    ```

    distributed及其参数-m、-t在语句最后指定。

    示例参考：

    ```shell
    # 单卡
    ./pytorch_gpu2npu.sh -i /home/train/ -o /home/out -v 2.1.0 [-s]
    # 分布式
    ./pytorch_gpu2npu.sh -i /home/train/ -o /home/out -v 2.1.0 [-s] distributed -m /home/train/train.py [-t model]
    ```

    “\[\]”表示可选参数，实际使用可不用添加。

    **表 1:**  参数说明

    |参数|参数说明|取值示例|
    |--|--|--|
    |-i</br>--input|&#8226; 要进行迁移的原始脚本文件所在文件夹路径。</br>&#8226; 必选。|/home/username/fmktransplt|
    |-o</br>--output|&#8226; 脚本迁移结果文件输出路径。</br>&#8226; 不开启distributed即迁移至单卡脚本场景下，输出目录名为xxx_msft；开启distributed即迁移至多卡脚本场景下，输出目录名为xxx_msft_multi，xxx为原始脚本所在文件夹名称。</br>&#8226; 必选。|/home/username/fmktransplt_output|
    |-v</br>--version|&#8226; 待迁移脚本的PyTorch版本。</br>&#8226; 必选。|&#8226; 2.1.0<br>&#8226; 2.6.0<br>&#8226; 2.7.1<br>&#8226; 2.8.0 |
    |-s</br>--specify-device|&#8226; 可以通过环境变量DEVICE_ID指定device作为高级特性，但有可能导致原本脚本中分布式功能失效。</br>&#8226; 可选。|-|
    |distributed|&#8226; 将GPU单卡脚本迁移为NPU多卡脚本，仅支持使用torch.utils.data.DataLoader方式加载数据的场景，具体可参考《CANN 分析迁移工具用户指南》中的“迁移训练 > 典型案例 > 使用torch.utils.data.DataLoader方式加载数据的场景说明”章节。指定此参数时，才可以指定-t/--target_model参数。</br>&#8226; -m，--main：训练脚本的入口Python文件，必选。</br>&#8226; -t，--target_model：待迁移脚本中的实例化模型变量名，默认为“model”，可选。如果变量名不为"model"时，则需要配置此参数，例如"my_model = Model()"，需要配置为-t my_model。|-|
    |-h</br>--help|显示帮助信息。|-|

## 注意事项

如果迁移时启用了“distributed“参数，想将GPU单卡脚本迁移为NPU多卡脚本，需进行如下操作获取结果文件：

> [!NOTE]
>
> 将GPU单卡脚本迁移为NPU多卡脚本之后，若原模型训练命令中包含指定卡号进行单卡运行的参数（如`--gpu`），需要删除该参数，以确保多卡运行不失效。

1. 训练脚本语句替换。

    将执行迁移命令后生成的`run_distributed_npu.sh`文件中的p`lease input your shell script here`语句替换成模型原来的训练shell脚本。例如将`please input your shell script here`替换为模型训练命令`bash  _model_train_script.sh _--data_path  _data_path_`。

    `run\_distributed\_npu.sh`文件如下所示：

    ```shell
    export MASTER_ADDR=127.0.0.1 
    export MASTER_PORT=29688 
    export HCCL_WHITELIST_DISABLE=1    
     
    NPUS=($(seq 0 7)) 
    export RANK_SIZE=${#NPUS[@]} 
    rank=0 
    for i in ${NPUS[@]} 
    do 
        export DEVICE_ID=${i} 
        export RANK_ID=${rank} 
        echo run process ${rank} 
        please input your shell script here > output_npu_${i}.log 2>&1 & 
        let rank++ 
    done
    ```

    **表 2:**  run_distributed_npu.sh参数说明

    |参数|说明|
    |--|--|
    |MASTER_ADDR|指定训练服务器的ip|
    |MASTER_PORT|指定训练服务器的端口|
    |HCCL_WHITELIST_DISABLE|HCCL通信白名单校验|
    |NPUS|指定在特定NPU上运行|
    |RANK_SIZE|指定调用卡的数量|
    |DEVICE_ID|指定调用的device_id|
    |RANK_ID|指定调用卡的逻辑ID|

2. 替换后，进入脚本所在目录执行如下命令，会生成指定NPU的log日志。

    ```shell
    bash run_distributed_npu.sh
    ```

3. 查看结果文件。

    脚本迁移完成后，进入脚本迁移结果输出路径查看结果文件。以GPU单卡脚本迁移为NPU多卡脚本为例，结果文件包含以下内容：

    ```shell
    ├── xxx_msft/xxx_msft_multi              // 脚本迁移结果输出目录
    │   ├── 生成脚本文件                 // 与迁移前的脚本文件目录结构一致
    │   ├── msFmkTranspltlog.txt         // 脚本迁移过程日志文件，日志文件限制大小为1M，若超过限制将分多个文件进行存储，最多不会超过10个
    │   ├── cuda_op_list.csv            //分析出的cuda算子列表
    │   ├── unknown_api.csv             //支持情况存疑的API列表
    │   ├── unsupported_api.csv         //不支持的API列表
    │   ├── change_list.csv              // 修改记录文件
    │   ├── run_distributed_npu.sh       // 多卡启动shell脚本
    ```

更多命令行方式工具迁移内容，请参见《[CANN 分析迁移工具用户指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/devaids/migrationtools/atlasfmkt_16_0001.html)》。
