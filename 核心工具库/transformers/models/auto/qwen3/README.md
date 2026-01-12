+ Qwen3PreTrainedModel

+ Qwen3Model

  纯 Transformer 解码器架构（含嵌入层、GQA 注意力、FFN/MoE、层归一化等），输入文本输出 hidden states（[batch, seq_len, hidden_size]），无生成或损失计算能力，仅负责特征编码；

+ Qwen3ForCausalLM

  在 Qwen3Model 基础上增加 lm_head，实现「特征→词表概率」映射，并集成 generate () 与损失计算，是训练 / 推理的**直接入口**。

+ Qwen3DecoderLayer

  是 Qwen3Model（主干网络）的最小重复模块 —— 整个 Qwen3 解码器由 ```num_hidden_layers`` 个 Qwen3DecoderLayer 堆叠而成，负责实现单一层的自注意力计算、前馈网络变换、残差连接与层归一化等核心逻辑

+ Qwen3RMSNorm

  ```Qwen3RMSNorm``` 是 Qwen3 模型中定制化的均方根归一化层（Root Mean Square Normalization），是对标准 RMSNorm 的轻量化适配，也是 Qwen3 解码器层（Qwen3DecoderLayer）中核心的归一化组件 —— 替代
  了传统的 LayerNorm，大幅降低计算开销的同时保证模型训练稳定性，尤其适配大模型的深层网络（如 32 层 / 80 层 Qwen3）。

+ Qwen3MLP

  ```Qwen3MLP``` 是 Qwen3 模型解码器层（Qwen3DecoderLayer）中核心的前馈网络模块，是对传统 Transformer 前馈网络的定制化优化 —— 采用 SwiGLU 激活函数、动态维度缩放与轻量化实现，在保证模型表达能力的同时，
  大幅提升计算效率，是 Qwen3 大模型（8B/72B）高效运行的关键组件之一

+ Qwen3Attention


+ Qwen3RotaryEmbedding

  wen3RotaryEmbedding 是 Qwen3 模型中定制化的**旋转位置编码（RoPE）模块**，是实现 Transformer 解码器「位置感知」的核心组件 —— 替代了传统的绝对位置编码，在保持计算高效的同时，让模型具备更好的长文本建模
  能力（Qwen3 支持最长 128K 上下文窗口），也是 ```Qwen3DecoderLayer``` 中自注意力模块的前置关键组件。
