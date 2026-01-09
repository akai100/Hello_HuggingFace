它是 Transformers 库中所有 Tokenizer（分词器）的通用父类，负责文本与 token 之间的转换，是大模型输入处理的核心组件。

## 核心属性

+ vocab_size

+ bos_token

+ eos_token

+ pad_token

+ unk_token

+ model_max_length

+ padding_side

## 主要功能

### ```__call__```

核心作用是将批量文本转换成模型可直接输入的 tensor 格式，自动完成 “分词 → 转 ID → 截断 / 补齐 → 生成 attention_mask” 全流程。

```python3

```
