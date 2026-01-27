Data Collator（数据整理器） 是一个非常关键的组件。它的核心作用是：将训练集中的多个样本（List of Samples）打包成一个模型可以接受的批次（Batch）。

## 1. 为什么需要 Data Collator？

在处理文本数据时，不同句子的长度通常不同。由于 GPU 运算需要数据形状对齐（矩形矩阵），我们不能直接把长度不一的 List 丢进去。

+ 动态填充 (Dynamic Padding)：Data Collator 可以只根据当前 Batch 中最长句子的长度进行填充，而不是根据全数据集最长句子填充。这能显著提高训练速度，减少冗余计算。

+ 格式转换：自动将 Python 字典转换成 torch.Tensor 或 tf.Tensor。

+ 特定任务处理：比如在掩码语言模型（MLM）中，它负责随机把 15% 的词替换成 [MASK]


## 2. 常见的 Data Collator 类型

| 类名 | 适用场景 | 核心功能 |
|------|---------|-----------|
| DefaultDataCollator | 通用任务 | 最基础的整理，不包含填充功能。要求所有输入长度必须一致 |
| DataCollatorWithPadding | 文本分类、句子对任务| 最常用。自动根据 Batch 内最长序列进行动态填充 |
| DataCollatorForLanguageModeling | MLM (如 BERT) / CLM (如 GPT) | 自动生成 labels。如果是 MLM，会自动处理随机 Mask 逻辑 |
| DataCollatorForTokenClassification | 序列标注 (NER) | 除了填充 input_ids，还会同步填充标签 labels（通常填充 -100 以忽略计算） |
| DataCollatorForSeq2Seq | 翻译、摘要 (T5/BART) | 处理 Encoder-Decoder 结构，专门处理 labels 的填充和右移逻辑 |
