Transformers旨在快速且易于使用，以便每个人都能开始学习或使用Transformer模型进行构建。


## 加载预训练模型

每个预训练模型都继承自三个几类。

### PreTrainedConfig

```PreTrainedConfig``` 是 Transformers 库中所有**模型配置类**的基类（父类），它的核心作用是：

+ 存储预训练模型的所有超参数和配置信息（比如模型层数、隐藏层维度、注意力头数、vocab 大小、是否使用偏置等）；
  
+ 统一不同模型（如 BERT、GPT、RoBERTa 等）的配置加载 / 保存逻辑，让所有模型的配置管理方式保持一致；

+ 支持从预训练模型的配置文件（config.json）加载配置，也支持手动修改配置参数。

### PreTrainModel

由配置文件中的模型属性定义的模型（或架构）。预训练模型仅返回原始隐藏状态。对于特定任务，请使用适当的模型头将原始隐藏状态转换为有意义的结果

### Preprocessor

## 使用 Pipeline 进行推理

## 使用 Trainer 对模型进行微调

