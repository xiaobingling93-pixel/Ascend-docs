# 大模型推理精度问题分析案例

## 问题现象

大模型推理过程中精度调优的目标是保障模型在昇腾平台上的推理能力，通常使用数据集校准或与标杆模型的输出进行比较的方式对模型的推理能力进行评估。在精度调优过程中，常见的精度问题主要有以下几种：

- 模型输出无意义内容，无法正常对话。
- 模型与标杆在回答时存在语义偏差，或确定性问题的回答存在明显差异（例如判断题的结果）。
- 数据集评测不通过。

虽然常见问题的现象、根因各异，但都可以通过本文介绍的大模型推理精度问题快速分析方法进行定位分析。

## 定位流程

大模型推理精度问题定位流程如[图1 精度问题定位流程图](#精度问题定位流程图)所示。

图1 精度问题定位流程图<a id="精度问题定位流程图"></a>

![精度问题定位流程图](../figures/llm_inference_accuracy_analysis_case/flowchart.png "精度问题定位流程图")

1. 排查由于配置错误导致的精度问题，可通过检查模型配置、模型结构、参数配置以及自定义算子实现等进行排查。
2. 如果配置无误，则选择存在明显精度问题的Bad Case进一步分析定位。
3. 开启确定性计算，采集模型输出的logits。
4. 将logits和标杆数据进行比对。
    - 如果比对结果一致，则排查“后处理”采样问题，进一步定位是模型自身问题还是“后处理”问题。
    - 如果比对结果不一致，则采集模型输出异常处中间计算结果，逐层比对分析误差来源。

## 问题定位与分析

### 检查配置

**模型配置**

1. 检查昇腾平台是否支持模型推理。

    昇腾平台基于MindIE推理框架适配了多种开源大模型，需要确认模型、设备、数据类型均配套且符合要求。如果需要了解MindIE支持的模型，请参见《[MindIE模型支持列表](https://www.hiascend.com/software/mindie/modellist)》。

2. 检查模型加载的权重来源是否为同一文件。如果模型权重不一致，会直接导致推理计算存在差异，推理精度无法对齐。

3. 检查模型配置文件，确认参数配置与标杆模型配置一致。

    常见的参数配置有pad_token_id、eos_token_id、max_sequence等，这些配置文件通常保存在模型权重路径下，可通过对齐config解决部分精度问题。

**环境配置**

如果模型在某一环境执行时精度正常，但在其他环境执行时精度异常，或更换软件版本后精度异常，则需要排查环境配置，需检查各个组件的版本是否一致，且和硬件环境适配，版本配套问题请查看《MindStudio版本说明》中的“[版本配套关系](https://www.hiascend.com/document/detail/zh/mindstudio/830/releasenote/firstpage_0005_001.html)”。

### 采用msit工具定位

#### 工具简介

如果模型配置和环境配置正确，模型依旧存在精度问题，则可以使用《[大模型推理精度工具（Large Language Model Debug Tool）](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/README.md)》进行精准定位与根因分析，有效提升开发效率，如[表1 大模型推理精度工具](#大模型推理精度工具)所示。

表1 大模型推理精度工具<a id="大模型推理精度工具"></a>

| 工具名称 | 说明 |
| ----- | ----- |
|Bad Case分析工具（msit Bad Case）|提供脚本和命令行两种交互方式来进行自动bad case分析，使用户能够快速定位。使用说明请参见《[msit Bad Case 分析工具](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E5%A4%A7%E6%A8%A1%E5%9E%8B%E7%B2%BE%E5%BA%A6%E5%AE%9A%E4%BD%8D%E4%B9%8BBadCase%E5%88%86%E6%9E%90%E5%B7%A5%E5%85%B7.md)》。|
|数据dump工具（msit llm dump）|提供加速库模型推理过程中产生的中间数据的dump能力，落盘的数据用于进行后续的精度比对。<br> - ATB场景的dump功能使用说明请参见《[加速库模型数据 dump](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-DUMP%E5%8A%A0%E9%80%9F%E5%BA%93%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)》。<br> - PyTorch场景的dump功能使用说明请参见《[PyTorch 场景的精度数据采集](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-Pytorch%E5%9C%BA%E6%99%AF%E6%95%B0%E6%8D%AEdump.md)》。|
|大模型精度比对工具（msit llm compare）|提供一键式精度比对功能，支持快速实现推理场景的整网精度比对。使用说明请参见《[大模型精度比对](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E5%A4%A7%E6%A8%A1%E5%9E%8B%E7%B2%BE%E5%BA%A6%E6%AF%94%E5%AF%B9.md)》。|
|精度预检工具（msit llm opcheck）|提供加速库内置算子的单算子精度预检能力，检测加速库算子精度是否达标。使用说明请参见《[opcheck单算子精度预检功能使用指南](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E7%B2%BE%E5%BA%A6%E9%A2%84%E6%A3%80%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)》。|

#### 选择Bad Case

Bad Case是模型与标杆模型推理结果存在差异的输入（Prompt）。Bad Case是我们进行大模型精度问题分析和定位的前提，可参见《[发现bad case](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%A6%82%E4%BD%95%E8%AF%86%E5%88%AB%20Bad%20Case.md)》发现和选择合适的Bad Case。

例如，在模型A上，输入一个生成代码相关的问题，在外部设备上可以正常回复，但在昇腾平台上却经常回复null，那么这个输入的问题就是一个Bad Case。我们可以将这个问题作为模型推理的输入进一步定位。

#### logits采集比对

**简介**

当选取了合适的Bad Case后，则需要采集其推理过程的中间数据，用于定位引入精度问题的具体token。

在定位精度问题时，通常采用“自下而上”的分层比对方法，从模型每个token最后一层输出的logits开始比对，找到首个与标杆数据比对精度不达标的输出token。

例如，模型A在外部设备上使用PyTorch框架进行推理，在昇腾设备上使用MindIE的加速库（ATB）框架进行推理，首先要分别采集模型A在外部设备和昇腾设备上推理的logits结果，然后比对两份logits结果，找到引入精度问题的具体token。

**logits采集**

1. 使用msit llm dump工具采集标杆模型logits。通过在PyTorch执行脚本中加入dump的逻辑代码实现标杆模型logits的采集，参数解释请参见《[PyTorch场景的精度数据采集](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-Pytorch%E5%9C%BA%E6%99%AF%E6%95%B0%E6%8D%AEdump.md)》。msit llm dump工具的使用请参见《[加速库模型数据 dump](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-DUMP%E5%8A%A0%E9%80%9F%E5%BA%93%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)》。

    1. 在模型A推理脚本中增加dump相关的配置代码，示例如下：

        ```python
        import torch
        from msit_llm import DumpConfig,register_hook
        from transformers import AutoTokenizer,AutoModelForCausalLM

        # 在推理模型完成初始化之前，固定随机种子，启动确定性计算
        from msit_llm import seed_all
        seed_all(seed=2345)

        # 配置dump参数，其中：
        # dump_last_logits=True表示只采集模型最后一个layer的输出，即输出token的logits
        # token_range=list(range(1000))表示前1000个token的logits，不足1000则采集模型输出的所有输出token的logits
        # dump_path="/data/atb_dump_path"，为dump数据保存路径，请根据实际情况替换

        dump_config=DumpConfig(dump_last_logits=True,token_range=list(range(1000)), dump_path="/data/atb_dump_path")

        # 推理模型初始化
        model_weight_path="/data/model_path"    # model_weight_path为模型A权重路径，请根据实际情况替换
        tokenizer=AutoTokenizer.from_pretrained(model_weight_path)
        model=AutoModelForCausalLM.from_pretrained(model_weight_path).cuda()
        register_hook(model,dump_config)        # model是要dump中间tensor的模型实例，在模型初始化后添加代码

        with torch.no_grad():
        # 推理流程代码

        ```

        > [!NOTE] 
        > 
        > 其中，启动确定性计算是为了保证结果的可重现性，避免随机带来的误差。

    2. 脚本执行完成后，标杆模型的logits将保存到dump_path指定目录下，数据目录结构如下所示。

        ```text
        {GOLDEN_DUMP_DIR}/                     # 数据保存路径
        ├── msit_dump_{TIMESTAMP}/             # 数据落盘时间戳目录
        └── torch_tensors/                     # tensor子目录
            ├── cuda{device_id}_{PID}/         # 设备ID_进程号子目录
                ├── model_tree.json            # 模型拓扑结构文件
                ├── {TID}                      # TokenID
                    ├── {LayerName}            # 网络层名称
                        ├── output.pth         # 输出Tensor
        ```

    3. （可选）建议dump两次logits数据，并执行以下命令，比对两次dump相同token的logits（此处比对token 0，即文件夹0下的output.pth数据）。可使用msit llm compare工具进行比对，具体使用方法请参见《[大模型精度比对](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E5%A4%A7%E6%A8%A1%E5%9E%8B%E7%B2%BE%E5%BA%A6%E6%AF%94%E5%AF%B9.md)》。

        ```bash
        msit llm compare -gp {GOLDEN_DUMP_DIR1}/msit_dump_{TIMESTAMP}/torch_tensors/cuda{device_id}_{PID}/0/{logits_Layer_Name}/output.pth -mp {GOLDEN_DUMP_DIR2}/msit_dump_{TIMESTAMP}/torch_tensors/cuda{device_id}_{PID}/0/{logits_Layer_Name}/output.pth
        ```

        两次dump对应相同token的logits所有精度指标如[图1 精度指标](#精度指标)所示，表明已正确开启确定性计算。

        图1 精度指标<a id="精度指标"></a>

        ![精度指标](../figures/llm_inference_accuracy_analysis_case/accuracy_index.png "精度指标")

2. 使用msit llm dump采集ATB模型logits。msit支持通过命令方式对ATB模型的数据进行dump，参数解释请参见《[加速库模型数据dump](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-DUMP%E5%8A%A0%E9%80%9F%E5%BA%93%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)》。

    1. 执行以下命令，获取模型网络结构信息，首次dump时需获取模型网络结构信息。

        ```bash
        msit llm dump --exec "bash run.sh" --type layer -seed 2345 -o {ATB_DUMP_DIR1}
        ```

    2. 执行完成后，默认落盘在当前执行文件夹下，落盘数据目录结构如下。

        ```text
        {ATB_DUMP_DIR1}/                       # 数据保存路径
        ├── msit_dump_{TIMESTAMP}/             # 数据落盘时间戳目录
        └── layer/                             # 网络结构子目录
            ├── {PID}/                         # 进程号
                ├── {LayerName}.json
        ```

        打开模型A的PID文件夹，文件内容如[图2 PID文件夹文件列表](#PID文件夹文件列表)所示。

        图2 PID文件夹文件列表<a id="PID文件夹文件列表"></a>

        ![PID文件夹文件列表](../figures/llm_inference_accuracy_analysis_case/pid_folder_file_list.png "PID文件夹文件列表")

        > [!NOTE]
        > 
        > 如果使用MindIE 1.0.RC1及之后版本，ATB模型由Prefill模型和Decoder模型组成，因此ATB的logits需要分别采集Prefill的logits和Decoder的logits。例如，模型A总共有68层，其中Prefill模型输出的最后一层为LmHead_33，Decoder模型输出的最后一层为LmHead_67，因此需要采集33和67层的logits。

    3. 二次dump时，采集模型A最后一层输出，指定-ids 33,67。

        ```bash
        msit llm dump --exec "bash run.sh" --type model tensor -ids 33,67 -er 0,1000 -child False -stp 1 -seed 2345 -o {ATB_DUMP_DIR2}
        ```

    4. 采集完成后，落盘数据格式如下。

        ```text
        {ATB_DUMP_DIR2}/                           # 数据保存路径
        ├── msit_dump_{TIMESTAMP}/                 # 数据落盘时间戳目录
        └── layer/                                 # 网络结构子目录
        └── model/                                 # 模型信息目录
            ├── {PID}                              # 进程号子目录
                ├── {model_name}_Prefill.json      # 模型Prefill层信息
                ├── {model_name}_Decoder.json      # 模型Decoder层信息
        └── tensors/                               # tensor子目录
            ├── {device_id}_{PID}/                 # 设备ID_进程号子目录
                ├── {TID}                          # TokenID
                    ├── {LayerName}                # 网络层名称
                        ├── after                  # 网络层输出tensor目录
                            ├── outtensor0.bin     # 输出tensor
                        ├── before                 # 网络层输入tensor目录
        ```

**logits比对**

1. 通过msit llm compare工具比对标杆模型logits与ATB模型的logits。

    在采集完标杆模型和ATB模型的logits数据后，即可通过msit llm compare工具实现logits的自动比对，具体使用方法和参数解释请参见《[大模型精度比对](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E5%A4%A7%E6%A8%A1%E5%9E%8B%E7%B2%BE%E5%BA%A6%E6%AF%94%E5%AF%B9.md)》。比对命令示例如下，其中COMPARE_PATH是比对结果保存路径。

    ```bash
    msit llm compare -gp {GOLDEN_DUMP_DIR1}/msit_dump_{TIMESTAMP}/torch_tensors/cuda{device_id}_{PID}/ -mp {ATB_DUMP_DIR1}/msit_dump_{TIMESTAMP}/tensors/{device_id}_{PID}  -o {COMPARE_PATH}
    ```

2. 比对完成后，会生成比对结果msit_cmp_report_{TIMESTAMP}.csv文件，文件保存在比对结果保存路径下，指标结果请参见《[精度比对参数说明](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E7%B2%BE%E5%BA%A6%E6%AF%94%E5%AF%B9%E7%BB%93%E6%9E%9C%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E.md)》。

    > [!NOTE]  
    > 
    > 由于不同业务场景对精度的要求并不一致，通常情况下，重点指标可以参考如下要求：
    > - KL散度：bfloat16<0.005，float16<0.0001
    > - 余弦相似度：>0.999

3. 分析比对结果。

    根据精度标准，可能出现两种情况：

    - 所有logits都满足当前场景的精度要求，则表明是“后处理”的输出异常，此时可以对temperature、topK、topP等“后处理”参数进行复检，或者对“后处理”的代码实现进行调试，从而定位具体原因。
    - logits中存在不满足精度标准的结果，从比较结果中找到首个精度不达标的token，进行整网精度比对。

    模型A的精度比对结果示例如[图3 模型A精度比对结果](#模型A精度比对结果)所示，发现第三个token的logits存在精度劣化问题，cosine_similarity=0.975758，KL散度=19.457，那么将对token 3进行整网精度比对，进一步定位问题。

    图3 模型A精度比对结果<a id="模型A精度比对结果"></a>

    ![模型A精度比对结果](../figures/llm_inference_accuracy_analysis_case/precision_comparison_result_of_modelA.png "模型A精度比对结果")

#### 整网数据采集比对

找到logits精度存在明显差异的token后，通过dump标杆模型、ATB模型的整网精度数据，再使用compare工具进行精度比对，定位问题。

以模型A为例，将dump第3个token的整网精度数据。

1. 采集标杆模型整网数据。代码示例如下。

    ```python
    import torch
    from msit_llm import DumpConfig,register_hook
    from transformers import AutoTokenizer,AutoModelForCausalLM

    # 在推理模型完成初始化之前，启动确定性计算
    from msit_llm import seed_all
    seed_all(seed=2345)

    # 配置dump参数，其中：
    # token_range=list([3]) 表示采集第3个token的整网数据
    # dump_path="/data/golden_dump_all_path"，为dump数据保存路径，请根据实际情况替换

    dump_config=DumpConfig(token_range=list([3]),dump_path="/data/golden_dump_all_path")  

    # 推理模型初始化 
    model_weight_path="/data/model_path"   # model_weight_path为模型A权重路径，请根据实际情况替换
    tokenizer=AutoTokenizer.from_pretrained(model_weight_path)
    model=AutoModelForCausalLM.from_pretrained(model_weight_path).cuda()
    register_hook(model, dump_config)     # model是要dump中间tensor的模型实例，在模型初始化后添加代码

    with torch.no_grad():
    # 推理流程代码
    ```

2. 采集ATB模型整网数据。

    代码示例如下，其中ATB_DUMP_ALL_PATH为dump数据保存路径。

    ```bash
    msit llm dump --exec  "bash run.sh"  -er 3,3 -o {ATB_DUMP_ALL_PATH} -seed 2345
    ```

3. 使用msit llm compare工具比对整网精度数据。

    示例如下，其中GOLDEN_DUMP_ALL_PATH为dump标杆模型整网数据的保存路径，COMPARE_PATH是比对结果保存路径。

    ```bash
    msit llm compare -gp {GOLDEN_DUMP_ALL_PATH}/msit_dump_{TIMESTAMP}/torch_tensors/cuda{device_id}_{PID}/ -mp {ATB_DUMP_ALL_PATH}/msit_dump_{TIMESTAMP}/tensors/{device_id}_{PID} -o {COMPARE_PATH}
    ```

4. 比对完成后，会生成比对结果文件msit_cmp_report_{TIMESTAMP}，保存在比对结果保存路径下。

    打开模型A的精度比对结果，找到首个不满足精度要求的Tensor，如[图1 首个不满足精度要求的Tensor](#首个不满足精度要求的Tensor)所示。

    图1 首个不满足精度要求的Tensor<a id="首个不满足精度要求的Tensor"></a>

    ![首个不满足精度要求的Tensor](../figures/llm_inference_accuracy_analysis_case/first_tensor.png "首个不满足精度要求的Tensor")

    查看my_data_path列可以确认问题引入的算子名称为LinearOperation，如[图2 查看问题引入的算子名称](#查看问题引入的算子名称)所示。

    图2 查看问题引入的算子名称<a id="查看问题引入的算子名称"></a>

    ![查看问题引入的算子名称](../figures/llm_inference_accuracy_analysis_case/viewing_operator_name.png "查看问题引入的算子名称")

5. 确认算子名称后，使用msit llm opcheck工具对算子的精度进行预检，判断加速库算子精度是否达标。

#### 算子精度预检

当定位到可能存在精度问题的算子后，可以使用msit llm opcheck工具对算子的精度进行预检，判断加速库算子精度是否达标。msit llm opcheck工具的使用方法和可选参数内容请参见《[opcheck单算子精度预检功能使用指南](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E7%B2%BE%E5%BA%A6%E9%A2%84%E6%A3%80%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)》。

1. 对LinearOperation算子做精度预检。<a id="步骤1"></a>

    示例如下，其中CHECK_PATH是预检结果数据保存路径。

    ```bash
    msit llm opcheck -i {GOLDEN_DUMP_ALL_PATH}/msit_dump_{TIMESTAMP}/tensors/{device_id}_{PID}/{TID}/ -ids 50_2_4_0 -opname LinearOperation  -o {CHECK_PATH}
    ```

    其中ids为指定预检的tensor索引，取其父目录的前缀，如果tensor路径为：{GOLDEN_DUMP_ALL_PATH}/msit_dump_{TIMESTAMP}/tensors/{device_id}_{PID}/3/50_Decoder_layer/2_MlpGateUpWeightPack/4_LinearNoQuant/0_LinearOperation/after/outtensor0.bin，那么其ids为50_2_4_0。

2. 检测结果如[图1 检测结果](#检测结果)所示。

    图1 检测结果<a id="检测结果"></a>

    ![检测结果](../figures/llm_inference_accuracy_analysis_case/test_result.png "检测结果")

    并保存预检结果至[步骤1](#步骤1)中指定的预检结果数据保存路径，打开预检结果文件，数据如[图2 预检结果数据](#预检结果数据)所示。

    图2 预检结果数据<a id="预检结果数据"></a>

    ![预检结果数据](../figures/llm_inference_accuracy_analysis_case/precheck_result.png "预检结果数据")

3. 确认算子精度是否达标，算子精度标准请参见[精度标准](https://gitcode.com/Ascend/msit/blob/26.0.0/msit/docs/llm/%E5%B7%A5%E5%85%B7-%E7%B2%BE%E5%BA%A6%E9%A2%84%E6%A3%80%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md#33-%E7%B2%BE%E5%BA%A6%E6%A0%87%E5%87%86)。

    - 如果算子精度达标，表明是标杆与NPU算子输入差异导致的误差，可以通过修改模型推理计算时的数据类型，提高模型的数据精度。

    - 如果算子精度不达标：

        - 检查算子输出tensor是否存在数值较小的问题（例如SelfAttentionOperation和PagedAttentionOperation），这类算子通常会使得计算出来的相对误差值偏大，导致精度比对结果不通过。这种情况推荐使用-metric指定其他指标综合判断算子精度情况。

        - 检查并对齐算子入参，如果依旧不满足实际的使用需求，通常需要具有算子开发相关经验的人员进行分析，开发者可参考《应用开发指南 (C&C++)》中的“[问题定位流程](https://www.hiascend.com/document/detail/zh/canncommercial/850/appdevg/acldevg/aclcppdevg_000102.html)”章节内容来尝试解决。
