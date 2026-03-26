# 迁移前分析

模型能否成功迁移至昇腾AI处理器，主要取决于其使用的算子是否被昇腾平台支持。为保证迁移可行性，迁移前可使用如下方法进行分析：

1.  若模型原始代码中调用了模型套件或第三方库，需要关注NPU对其的支持情况。
    -   如果该三方库原生支持NPU，用户需要关注NPU目前对库中特性的支持情况；
    -   如果是昇腾适配的第三方库，用户需要额外安装该库的昇腾适配版本，并关注其适配情况。详细昇腾第三方库支持情况请参考《[套件与三方库支持清单](https://www.hiascend.com/document/detail/zh/Pytorch/730/modthirdparty/modparts/thirdpart_0003.html)》。如果用户希望以上第三方库和模型套件在适配昇腾设备后能达到更高的性能，可以自行调优。

2.  确认是否存在以下已知的不支持场景：

    **表 1**  不支持场景列表

    |场景/组件|支持状态|适配说明|替代方案|
    |--|--|--|--|
    |DP（Data Parallel，数据并行）模型|暂不支持|不兼容torch.nn.parallel.DataParallel接口|需手动修改为torch.nn.parallel.DistributedDataParallel接口，以执行多卡训练原脚本需要在GPU环境下基于Python3.8及以上跑通|
    |APEX库中的FusedAdam融合优化器|部分支持|不支持自动迁移或PyTorch GPU2Ascend工具迁移|需手工进行迁移，具体修改方法可单击[LINK](https://gitcode.com/Ascend/apex#apexoptimizers)|
    |bitsandbytes|部分支持|已支持在昇腾上进行安装，具体可单击[Supported Backends](https://github.com/bitsandbytes-foundation/bitsandbytes/blob/main/docs/source/installation.mdx#supported-backendsmulti-backend-supported-backends)进行参考|仅支持NF4量化/反量化迁移，用于LLM QLoRA微调，其余功能暂不支持|
    |xFormers|暂不支持|不原生支持xFormers|xFormers中的FlashAttentionScore融合算子的迁移可参考[FlashAttentionScore](FlashAttentionScore.md)章节进行替换|
    |bmtrain框架|暂不支持|大模型迁移场景不支持|暂无替代方案|
    |colossalai库中HybridAdam优化器|暂不支持|大模型迁移场景不支持|暂无替代方案|
    |grouped_gemm三方库|暂不支持|NPU不支持安装|暂无替代方案|
    |composer三方库|暂不支持|NPU支持安装但未适配|暂无替代方案|


3.  借助PyTorch Analyse工具，分析基于GPU平台的PyTorch训练脚本中三方库套件、API、动态shape以及亲和API分析在昇腾AI处理器上的支持情况，具体可参见表2，工具使用详细指导可参见《[CANN 分析迁移工具用户指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/devaids/migrationtools/atlasfmkt_16_0001.html)》。

    **表 2**  分析模式介绍

    <table>
      <tbody align="left">
        <tr>
          <th>分析模式</th>
          <th>分析脚本</th>
          <th>分析结果</th>
          <th>调优建议</th>
        </tr>
        <tr>
          <td>三方库套件分析模式</td>
          <td>需用户提供待分析的三方库套件源码。</td>
          <td>
            <p>可快速获得源码中不支持的三方库API和cuda信息。</p>
            <p><strong>说明：</strong><br>三方库API是指在三方库代码中的函数，如果某函数的函数体内使用了不支持的torch算子或者cuda自定义算子，则此函数就是三方库不支持的API。如果第三方库中其他函数调用了这些不支持的API，则这些调用函数也为不支持的API。</p>
          </td>
          <td>-</td>
        </tr>
        <tr>
          <td>API支持情况分析模式</td>
          <td rowspan="3">需用户提供待分析的PyTorch训练脚本。</td>
          <td>可快速获得训练脚本中不支持的torch API和cuda API信息。</td>
          <td>输出训练脚本中API精度和性能调优的专家建议。</td>
        </tr>
        <tr>
          <td>动态shape分析模式</td>
          <td>可快速获得训练脚本中包含的动态shape信息。</td>
          <td>-</td>
        </tr>
        <tr>
          <td>亲和API分析模式</td>
          <td>可快速获得训练脚本中可替换的亲和API信息。</td>
          <td>-</td>
        </tr>
      </tbody>
    </table>

    在迁移可行性分析中如果存在平台未支持的算子，可参考如下方法进行算子适配：

    -   修改模型脚本使用等价支持的算子替换；
    -   算子开发与适配，算子开发相关请参见《[CANN Ascend C算子开发指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/opdevg/Ascendcopdevg/atlas_ascendc_10_0001.html)》或《[CANN TBE&AI CPU算子开发指南](https://www.hiascend.com/document/detail/zh/canncommercial/850/opdevg/tbeaicpudevg/atlasopdev_10_0001.html)》，算子适配请参见《PyTorch 框架特性指南》中的“[自定义算子适配开发](../framework_feature_guide_pytorch/adaptation_description_opplugin.md)”章节；
    -   联系华为工程师提出开发适配诉求。

