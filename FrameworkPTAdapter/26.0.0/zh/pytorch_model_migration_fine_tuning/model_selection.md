# 模型选取

建议用户在选取迁移模型时，尽可能选取权威PyTorch模型实现仓库，包括但不限于PyTorch（[imagenet](https://github.com/pytorch/examples/tree/master/imagenet)/[vision](https://github.com/pytorch/vision)等）、Meta Research（[Detectron](https://github.com/facebookresearch/Detectron)/[detectron2](https://github.com/facebookresearch/detectron2)等）、open-mmlab（[MMDetection](https://github.com/open-mmlab/mmdetection)/[mmpose](https://github.com/open-mmlab/mmpose)等）。

对于大模型，使用较为广泛的资源仓库是[HuggingFace](https://github.com/huggingface)、[Megatron-LM](https://github.com/NVIDIA/Megatron-LM)、[Llama-Factory](https://github.com/hiyouga/LLaMA-Factory)等仓库，可以在其中选取目标模型。

评估选取模型迁移的可行性：

- 保证选取的模型能在三方平台（如GPU）可成功运行。
- 从权威网站或数据平台获取选定模型的精度和性能基线，或在三方平台实测精度和性能基线。
