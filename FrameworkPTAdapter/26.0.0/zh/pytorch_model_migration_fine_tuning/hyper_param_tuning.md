# 超参配置调优

- **batch size配置**：在模型训练过程中，batch size的设定通常较大，旨在充分利用大规模训练数据，增强模型训练过程的稳定性。例如，设置batch size为8196，在样本序列长度为2k的情况下，可使每个批次处理大约16M个token输入。大样本的梯度平均值使得参数更新更精确，从而缓解不精确参数更新导致的loss跳变。GPT-3使用动态调整batch size的方式，使其每步处理的token数从32K逐渐增大到3.2M，这是提高训练稳定性的有效手段。

- **学习率**：学习率一般较小，且包含warm up设置，以确保训练平稳。比如在前0.1%\~0.5%的训练步骤中，设置一个线性的学习率递增。峰值学习率一般在10<sup>-4</sup>以下，比如GPT-3的学习率是6 × 10<sup>-5</sup>。之后，会采用cosine衰减或者线性衰减学习率调度策略，逐渐减小学习率，在收敛前将学习率下降到峰值学习率的10%左右。

- **优化器**：优化器一般采用Adam、AdamW以及Adafactor。其中，Adafactor是Adam的一个节约显存的变体。也有使用SGD作为优化器的例子，但并不常见。

- **权重初始化**：正确初始化权重可以帮助模型更快地收敛并提高性能。例如，通常使用小的高斯噪声或者使用T-fixup初始化。
- **其他稳定训练技术**：
    - 梯度裁剪（Gradient Clipping）：作为一种常用的稳定训练手段，通常设定裁剪阈值为1.0，防止梯度过大引发训练不稳定。
    - Weight Decay（L2正则化）：设置合理的权重衰减率，如0.1，有助于防止过拟合，增强模型泛化能力。
    - 特殊层的调整：GLM研究发现，embedding层往往存在较大的梯度异常情况，故需根据实际情况适度调整相关参数。

以下列出业界各类大模型的超参配置表，以供参考：

**表 1**  业界各类大模型的超参配置表

|**Model**|**Batch Size (#tokens)**|**Learning Rate**|**Warmup**|**Decay Method**|**Optimizer**|**Precision Type**|**Weight Decay**|**Grad Clip**|**Dropout**|
|--|--|--|--|--|--|--|--|--|--|
|GPT3 (175B)|32K→3.2M|$6 × 10^{-5}$|yes|cosine decay to 10%|Adam|FP16|0.1|1|-|
|PanGu-α (200B)|-|$2 × 10^{-5}$|-|-|Adam|-|0.1|-|-|
|OPT (175B)|2M|$1.2 × 10^{-4}$|yes|manual decay|AdamW|FP16|0.1|-|0.1|
|PaLM (540B)|1M→4M|$1 × 10^{-2}$|no|inverse square root|Adafactor|BF16|lr2|1|0.1|
|BLOOM (176B)|4M|$6 × 10^{-5}$|yes|cosine decay to 10%|Adam|BF16|0.1|1|0|
|MT-NLG (530B)|64K→3.75M|$5 × 10^{-5}$|yes|cosine decay to 10%|Adam|BF16|0.1|1|-|
|Gopher (280B)|3M→6M|$4 × 10^{-5}$|yes|cosine decay to 10%|Adam|BF16|-|1|-|
|Chinchilla (70B)|1.5M→3M|$1 × 10^{-4}$|yes|cosine decay to 10%|AdamW|BF16|-|-|-|
|Galactica (120B)|2M|$7 × 10^{-6}$|yes|linear decay to 10%|AdamW|-|0.1|1|0.1|
|LaMDA (137B)|256K|-|-|-|-|BF16|-|-|-|
|Jurassic-1 (178B)|32K→3.2M|$6 × 10^{-5}$|yes|-|-|-|-|-|-|
|LLaMA (65B)|4M|$1.5 × 10^{-4}$|yes|cosine decay to 10%|AdamW|-|0.1|1|-|
|LLaMA2 (70B)|4M|$1.5 × 10^{-4}$|yes|cosine decay to 10%|AdamW|-|0.1|1|-|
|Falcon (40B)|2M|$1.85 × 10^{-4}$|yes|cosine decay to 10%|AdamW|BF16|0.1|-|-|
|GLM (130B)|0.4M→8.25M|$8 × 10^{-5}$|yes|cosine decay to 10%|AdamW|FP16|0.1|1|0.1|
|T5 (11B)|64K|$1 × 10^{-2}$|no|inverse square root|Adafactor|-|-|-|0.1|
|ERNIE 3.0 Titan (260B)|-|$1 × 10^{-4}$|-|-|Adam|FP16|0.1|1|-|
|PanGu-Σ (1.085T)|0.5M|$2 × 10^{-5}$|yes|-|Adam|FP16|-|-|-|
