# Hello_HuggingFace

## huggingface 是什么

Hugging Face 是一个开源的 AI 社区和平台，核心定位是让机器学习 / 深度学习更易用，尤其在自然语言处理（NLP）领域几乎成为行业标准，现在也扩展到计算机视觉、语音等多模态领域。

你可以把它理解为：

+ 模型仓库：全球最大的预训练模型库（Hugging Face Hub），包含 ChatGPT 类大模型、BERT、GPT-2/3、Stable Diffusion 等数万种模型，支持直接下载 / 调用；

+ 开发工具库：提供易用的 Python 库（Transformers、Datasets、Accelerate 等），让你无需从零编写复杂的模型代码；
  
+ 协作平台：支持团队共享模型、数据集、代码，还能在线测试模型效果。


## 核心工具库

### Transformers 库

这是 Hugging Face 最经典的库，封装了几乎所有主流的预训练模型，支持快速加载、推理、微调。

### Datasets 库

用于快速加载、处理各类数据集（文本、图像、语音），无需手动写复杂的数据读取代码。


### Accelerate 库

简化模型训练的分布式 / 混合精度配置，新手无需深入理解底层，就能利用 GPU / 多卡加速训练。
