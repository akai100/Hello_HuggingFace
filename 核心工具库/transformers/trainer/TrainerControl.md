
TrainerControl 是一个 「训练流程控制标志位的容器类」，它的所有属性都是布尔型或枚举型的开关，用于告诉 Trainer：在当前训练节点（如步结束、轮结束），是否需要执行某个操作。

## 核心设计逻辑

+ 单向传递

  Trainer 在触发 TrainerCallback 的钩子方法时，会将当前的 TrainerControl 实例传入方法；回调函数可以修改该实例的属性，但无法创建新实例；

+ 优先级高于配置

  回调中通过 TrainerControl 设置的操作（如 should_save=True），会覆盖 TrainingArguments 中的默认配置（如未到 save_steps 也会强制保存）；

+ 一次性生效

  大部分控制标志位（如 should_save、should_evaluate）是一次性的—— 执行完对应操作后，Trainer 会立即将其重置为默认值（如 False），避免持续触发。


## 核心属性

+ should_stop

  bool	False	是否立即终止整个训练流程	任意节点（步 / 轮 / 评估 / 保存）	最常用的控制属性，实现早停的核心

+ should_epoch_stop

  bool	False	是否立即终止当前 epoch	步结束 / 轮中	仅终止当前轮，下一轮仍会继续

+ should_save

  bool	False	是否强制保存模型 checkpoint	任意节点	覆盖 TrainingArguments 的 save_strategy，即使未到 save_steps 也会保存

+ should_evaluate

  bool	False	是否强制执行一次评估	任意节点	覆盖 TrainingArguments 的 evaluation_strategy，即使未到 eval_steps 也会评估
  
+ should_skip_evaluation

  bool	False	是否跳过本次计划中的评估	评估节点前	与 should_evaluate 相反，用于临时跳过评估

+ should_log

  bool	False	是否强制记录一次日志	任意节点	覆盖 TrainingArguments 的 logging_strategy，补充日志记录

+ force_save_last

  bool	False	是否强制保存最后一次 checkpoint	训练结束前	即使 save_strategy="no"，也会保存最终模型

+ ignore_for_eval

  bool	False	评估时是否忽略当前批次的预测结果	评估循环中	用于过滤无效的评估样本
