# 模型结构与算法

业界主流大语言模型整体结构相似，但是局部设计上各有不同，如表1所示：

**表 1**  业界主流大模型结构

|模型|参数量|Hidden size|Attention Heads Num|Layers Num|Decoder|PE|Normalization|Activation|
|--|--|--|--|--|--|--|--|--|
|LLaMA|7B|4096|32|32|Causal|RoPE|RMSNorm|SwiGLU|
|LLaMA|13B|5120|40|40|Causal|RoPE|RMSNorm|SwiGLU|
|LLaMA|33B|6656|52|60|Causal|RoPE|RMSNorm|SwiGLU|
|LLaMA|65B|8192|64|80|Causal|RoPE|RMSNorm|SwiGLU|
|ChatGLM|6B|4096|32|28|Prefix|RoPE|Layer Norm|GeLU|
|GLM|130B|12288|96|70|Prefix|RoPE|DeepNorm|GeGLU|
|GPT|13B|5140|40|40|Causal|Learned|Pre Layer Norm|GeLU|
|GPT|175B|12288|96|96|Causal|Learned|Pre Layer Norm|GeLU|
|Bloom|175B|14336|112|70|Causal|ALiBi|Pre Layer Norm|GeLU|


一般来说，不同的算法选择不影响模型收敛，只会对最终收敛的Loss水平和模型训练效果有影响。业界常使用scaling law来预测一个算法在大模型上的收敛趋势。同时要注意的是，由于大模型代码结构越来越复杂，算法实现错误时有发生，而这会对大模型收敛性有决定性影响。如果使用新设计的实验性模型结构，需要经过反复调试和实验，确保实现没有错误，才能使得模型Loss收敛，并且达到和主流模型匹配甚至更好的性能。

