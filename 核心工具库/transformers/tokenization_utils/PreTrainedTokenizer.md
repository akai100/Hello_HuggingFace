
```PreTrainedTokenizer```是 Transformers 库中**所有预训练分词器（Tokenizer）的基类**，它封装了文本预处理的通用逻辑（分词、ID 映射、特殊 Token 处理、张量转换等），
所有具体的分词器（如```BertTokenizer```、```GPT2Tokenizer```、```LLaMATokenizer```、```CLIPTokenizer```）都继承自这个基类。它的核心作用是**把人类可读的原始文本，
转换为模型能理解的 Token 序列和张量格式**，是连接自然语言和模型输入的 “桥梁”。


## 关键方法

### ```__cal__```

一站式文本预处理：分词->编码->生成张量(input_ids/attention_mask等）。

```python3

```
