
```EvalPrediction``` 是 ```transformers``` 库中用于**封装模型评估阶段的预测结果和真实标签**的专用类，主要配合 ```Trainer/TrainerCallback``` 使用，让评估指标的计算更标准化、更便捷。

## 介绍

### 核心定位与作用
在使用 ```Trainer``` 进行模型训练时，当执行评估（```evaluation```）步骤时：

1. ```Trainer``` 会先让模型对评估数据集做出预测，得到 predictions；

2. 从评估数据集中提取对应的真实标签 ```label_ids```；

3. 将这两部分数据（以及可选的 ```inputs```）封装成 ```EvalPrediction``` 对象；

4. 你可以基于这个对象自定义评估函数，计算准确率、F1、BLEU 等任意指标。
   
简单来说：```EvalPrediction``` 是预测结果 + 真实标签的 “容器”，是自定义评估逻辑的核心入参。

## 核心属性与初始化

### 核心属性（只读）

| 属性名	 | 类型	| 说明 |
|--------|------|--------|
| predictions	| np.ndarray / tuple	| 模型的预测结果： - 分类任务：通常是形状为 (n_samples, n_classes) 的概率 / 对数几率数组；- 生成任务：可能是元组（如 (logits, hidden_states)）；- 问答任务：可能是起始 / 结束位置的预测数组 |
| label_ids	| np.ndarray / None	| 真实标签（标签 ID），形状通常为 (n_samples,) 或适配任务的多维数组；无标签时为 None |
| inputs	| dict / None	| 可选，模型的输入数据（如 input_ids、attention_mask），仅当需要基于输入计算指标时使用|

### 初始化方式

EvalPrediction 通常不需要你手动初始化（由 Trainer 自动生成），但了解其构造方式有助于理解：

```python3
执行评估（Trainer 会自动生成 EvalPrediction 并传入 compute_metrics）
eval_results = trainer.evaluate()
```
