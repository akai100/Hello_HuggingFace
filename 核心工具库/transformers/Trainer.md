Trainer 是 Transformers 库中最高层的训练封装类，它将模型、数据、训练参数、优化器、学习率调度器等核心组件整合在一起，提供了开箱即用的训练、评估、预测功能。

```python3
transformers.traniner.Trainer(model: Union[PreTrainedModel, nn.Module, None] = None,
                              args: Optional[TrainingArguments] = None,
                              data_collator: Optional[DataCollator] = None,
                              train_dataset: Optional[Union[Dataset, IterableDataset, "datasets.Dataset"]] = None,
                              eval_dataset: Optional[Union[Dataset, dict[str, Dataset], "datasets.Dataset"]] = None,
                              processing_class: Optional[
                                Union[PreTrainedTokenizerBase, BaseImageProcessor, FeatureExtractionMixin, ProcessorMixin]
                              ] = None,
                              model_init: Optional[Callable[..., PreTrainedModel]] = None,
                              compute_loss_func: Optional[Callable] = None,
                              compute_metrics: Optional[Callable[[EvalPrediction], dict]] = None,
                              callbacks: Optional[list[TrainerCallback]] = None,
                              optimizers: tuple[Optional[torch.optim.Optimizer], Optional[torch.optim.lr_scheduler.LambdaLR]] = (None, None),
                              optimizer_cls_and_kwargs: Optional[tuple[type[torch.optim.Optimizer], dict[str, Any]]] = None,
                              preprocess_logits_for_metrics: Optional[Callable[[torch.Tensor, torch.Tensor], torch.Tensor]] = None,)
```

+ model

  待训练 / 微调的模型实例，必须是 PreTrainedModel 子类（如 LlamaForCausalLM），或自定义 nn.Module

+ args, TrainingArguments

  训练超参数与运行配置

+ data_collator

  批次数据整理器

  核心作用：将零散样本拼接为模型可输入的批次（自动处理 padding、生成 labels 等）

  常用实现：

  - 生成式模型：DataCollatorForLanguageModeling（mlm=False）

  - 分类任务：DataCollatorWithPadding

+ train_dataset(Dataset / torch.utils.data.Dataset)

  训练数据集	需是 datasets.Dataset 或 PyTorch Dataset 格式，需提前完成文本编码（转 token ID）

+ eval_dataset(Dataset / torch.utils.data.Dataset)

  评估数据集,	开启评估（evaluation_strategy≠no）时必须指定，建议与训练数据同格式

+ processing_class

  用于处理数据的处理类。如果提供了该类，它将用于自动处理模型的输入，并且会与模型一起保存，以便更轻松地重新运行中断的训练或重用微调后的模型。此参数取代了tokenizer参数，后者现已弃用。

+ model_init

  模型初始化函数	用于多轮超参数搜索（如网格搜索），每次搜索重新初始化模型

+ compute_loss_func

  一个接收原始模型输出、标签以及整个累积批次中的项目数量（批次大小乘以梯度累积步数）并返回损失的函数。

+ compute_metrics

  自定义评估指标计算函数	输入是 EvalPrediction（包含 predictions 和 labels），输出是字典（如 {"accuracy": 0.95}）

  常用场景：分类任务（准确率 / F1）、生成任务（BLEU/ROUGE）

+ callbacks

  训练回调函数	自定义训练过程中的行为（如早停、日志增强、模型导出）

  常用内置回调：

  - EarlyStoppingCallback（早停）
  - WandbCallback（wandb 日志）
  - ProgressCallback（进度条）

+ optimizers

  自定义优化器和学习率调度器	覆盖 TrainingArguments 中的 optim 和 lr_scheduler_type，适合定制化优化策略（如用 AdamW 并手动设置 betas）

+ preprocess_logits_for_metrics

  预处理 logits 后再计算指标	生成式模型常用：将 logits 转为预测 ID（argmax），再传入 compute_metrics。

## 核心功能

### ```train```

启动训练 / 微调流程，核心方法，调用后开始训练

### ```evaluate```

评估模型性能，训练结束后或训练中手动评估

### ```predict```

对测试集进行预测	生成预测结果，用于后续分析。

### ```save_model```

保存模型权重, 手动保存模型，补充自动保存

### ```log```

手动记录日志, 自定义日志内容，如评估指标

### ```compute_loss```

计算损失函数，可重写，实现自定义损失（如对比损失、蒸馏损失）

### ```get_train_dataloader```

获取训练数据加载器，可重写，实现自定义数据加载逻辑

### ```get_eval_dataloader```

获取评估数据加载器，	可重写，实现自定义评估数据逻辑
