## TrainerState 简介

### 本质定位
TrainerState 是一个 「训练状态的持久化容器类」，它的核心特点是：

+ 只读性

  训练过程中，Trainer 会自动更新 TrainerState 的属性，自定义回调只能读取，不能修改（与 TrainerControl 的「可写性」形成鲜明对比）；

+ 持久化

  每次保存 checkpoint 时，Trainer 会自动将 TrainerState 序列化并保存到 trainer_state.json 文件中，恢复训练时会自动加载，保证训练状态的连续性；

+ 全生命周期覆盖

  记录了从训练开始到结束的所有关键状态，包括步数、轮次、损失、指标、checkpoint 路径、训练时长、随机种子等。

### 核心设计逻辑

+ 单向更新

  TrainerState 的属性只能由 Trainer 内部的训练循环更新，外部（包括 TrainerCallback）无法直接修改；
  
+ 数据依据

  TrainerCallback 中实现自定义逻辑的所有数据都来自 TrainerState（如判断早停需要读取损失，保存最优模型需要读取评估指标）；

+ 持久化保障

  通过 trainer_state.json 文件，实现训练状态的持久化，即使训练中断，恢复后也能保持与中断前完全一致的状态。
### 与其他组件的关系

```plaintext
Trainer → 训练循环 → 自动更新 TrainerState 属性 → 触发 TrainerCallback 钩子 → 回调读取 TrainerState → 根据状态修改 TrainerControl → Trainer 执行控制操作
```
+ 生成者

  Trainer 是 TrainerState 的唯一生成者和更新者；

+ 使用者

  TrainerCallback 是 TrainerState 的主要使用者；
+ 对应关系

  与 TrainerControl 是 「状态 - 控制」的一一对应关系：

  + TrainerState：回答 「训练现在在哪里？」（当前状态）；
  + TrainerControl：回答 「训练下一步要做什么？」（控制行为）。

## 核心属性

### 基础训练状态

| 属性名	| 类型	| 默认值	| 核心作用	| 更新时机	| 工程要点|
|-------|-------|-------|---------|---------|-----|
| global_step	| int	| 0	| 全局训练步数	| 每完成一个 training step 后	| 训练的核心进度指标，与 TrainingArguments.max_steps 对应|
| epoch	| float	| 0.0	| 当前训练轮次	| 每完成一个 batch 后（小数表示轮次的进度，如 1.5 表示第 2 轮的一半）	| 与 TrainingArguments.num_train_epochs 对应|
| step	| int	| 0	| 当前 epoch 内的步数	| 每完成一个 batch 后	| 仅用于记录当前轮次的进度，优先级低于 global_step|

### 日志与指标状态

| 属性名	 | 类型	| 默认值 |	核心作用	| 更新时机	| 工程要点 |
|--------|------|-------|---------|---------|----------|
| log_history	| list[dict] |	[]	| 训练日志历史列表	| 每触发一次日志记录后	| 最核心的属性！每个元素是一个字典，包含 step、loss、eval_loss、eval_accuracy 等指标，是自定义回调的主要数据来源 |
| best_metric	| float	| None	| 历史最优评估指标值	| 每完成一次评估后 |	与 TrainingArguments.metric_for_best_model 对应，如 eval_loss、eval_accuracy |
| best_model_checkpoint	| str	| None	| 历史最优模型的 checkpoint 路径	| 每保存一次最优模型后	| 训练结束后，load_best_model_at_end=True 会加载该路径下的模型 |

### Checkpoint 与保存状态

| 属性名	| 类型	| 默认值	| 核心作用	| 更新时机 |	工程要点 |
|-------|-------|-------|---------|---------|-------|
| checkpoint_path |	str |	None	| 当前最新的 checkpoint 路径	| 每保存一次 checkpoint 后 |	恢复训练时，会从该路径加载模型 |
| saved_checkpoints |	list[str] |	[]	| 所有已保存的 checkpoint 路径列表 |	每保存一次 checkpoint 后 |	与 TrainingArguments.save_total_limit 对应，用于删除旧的 checkpoint |
| is_hyper_param_search |	bool	| False	| 是否处于超参数搜索模式 |	训练开始前	| 超参数搜索时，会自动设置为 True |

### 训练控制与中断状态

| 属性名	| 类型	| 默认值	| 核心作用	| 更新时机	| 工程要点 |
|-------|--------|------|---------|---------|------|
|should_stop	| bool	| False	| 训练是否应该停止	| 每完成一个 step 后 |	由 TrainerControl.should_stop 触发，Trainer 会根据该属性判断是否终止训练 
|trial	| Optional[Trial]	| None	| 超参数搜索的试验对象	| 超参数搜索时 |	仅用于超参数搜索，普通训练时为 None |

### 时间与性能状态

| 属性名	 | 类型	| 默认值	| 核心作用	| 更新时机	| 工程要点 |
| seed	| int	| 42	| 训练的随机种子	| 训练开始前	| 与 TrainingArguments.seed 对应，保证训练的可复现性 |
| world_size	| int	| 1	| 分布式训练的总进程数	| 训练开始前	| 与 TrainingArguments.local_rank 对应，用于分布式训练 |
