TrainingArguments 封装了模型训练的所有超参数和运行配置，涵盖训练流程（批次、学习率、步数）、硬件适配（设备、混合精度）、日志 / 保存（checkpoint、日志工具）、评估 / 早停等维度，总计约 80 + 属性。

## 核心基础配置

| 属性名 | 类型 | 默认值 | 核心作用 |  工程面试要点 |
|--------|------|------|---------|-----------------|
| output_dir | str |	必填	| 训练结果保存路径（checkpoint、日志、模型） |	必须指定，否则报错 |
| per_device_train_batch_size	| int |	8	| 单设备（GPU/CPU）的训练批次大小 |	显存不足时调小，如 4/2 |
| per_device_eval_batch_size	| int	| 8	| 单设备的评估批次大小 |	通常与训练批次一致或翻倍 |
| num_train_epochs	| float	| 3.0 |	训练总轮数 |	大模型微调常用 1-3 轮，避免过拟合 |
| learning_rate |	float |	5e-5	| 初始学习率	| 大模型微调常用 2e-5/1e-5，需配合调度器 |
| seed |	int |	42	| 随机种子 |	保证训练可复现，所有随机操作都基于此 |
| max_steps |	int |	-1 |	训练总步数（优先级高于 epoch） |	设为正数时，按步数停止而非 epoch |
| gradient_accumulation_steps |	int |	1	| 梯度累积步数 | 显存不足时调大（如 8），等价于增大有效批次 |


## 学习率调度配置

| 属性名 | 类型 | 默认值 | 核心作用 | 关联知识点 |
|--------|-----|--------|---------|--------------|
| lr_scheduler_type | str | "linear" | 学习率调度器类型 | 可选：linear/cosine/cosine_with_restarts/polynomial/constant/constant_with_warmup |
| warmup_ratio | float | 0.0 | 预热步数占总步数的比例 | 大模型常用 0.05-0.1，避免初始学习率过高 | 
| warmup_steps | int | 0 | 预热步数（优先级高于 ratio） | 与你之前学的τ（时间常数）关联，控制学习率上升阶段 |
| learning_rate_end | float | None | 调度器最终学习率（仅 cosine/polynomial） | 避免学习率降到 0，常用 1e-7 | powerfloat1.0多项式调度器的幂次控制学习率下降速度 |
| power | float | 1.0 | 多项式调度器的幂次 | 控制学习率下降速度 |

## 硬件与性能优化配置

| 属性名 | 类型 | 默认值 | 核心作用 | 工程要点 |
|--------|-----|--------|---------|-------|
| fp16 | bool | False | 是否启用 FP16 混合精度训练 | 需 GPU 支持（如 NVIDIA Turing/Ampere 架构），节省显存 |
| bf16 | bool | False | 是否启用 BF16 混合精度训练 | 需 GPU 支持（如 A100/H100），稳定性优于 FP16 |
| fp16_full_eval | bool | False | 评估时是否用 FP16 | 加速评估，需与训练精度一致 |
| gradient_checkpointing | bool | False | 是否启用梯度检查点 | 大模型训练必开，节省显存（牺牲～20% 速度） |
| gradient_checkpointing_kwargs | dict | None | 梯度检查点额外参数 | 如{"use_reentrant": False}（新版 PyTorch 推荐） |
| tf32 | bool | True | 是否启用 TF32 精度（NVIDIA Ampere+） | 加速矩阵运算，不损失精度 |
| load_best_model_at_end | bool | False | 训练结束后加载最优模型 | 需配合evaluation_strategy使用 | 
| optim | str | "adamw_hf" | 优化器类型 | 可选：adamw_hf/adamw_torch/adamw_apex/fused_adam（大模型推荐 fused_adam） |
| optim_args | dict | None | 优化器额外参数 | 如{"betas": (0.9, 0.999), "eps": 1e-8}（你学的ϵ） |


##  日志与监控配置

| 属性名 | 类型	| 默认值 |	核心作用 |	面试考点 |
|-------------|-----|-----------------|-------------------------|---------------------------------------|
| logging_dir |	str |	output_dir/runs |	TensorBoard 日志保存路径	| 可视化训练曲线（损失 / 学习率 / 准确率） |
| logging_strategy |	str |	"steps" |	日志记录策略	| 可选：steps/epoch/no（no = 不记录） } |
| logging_steps| 	int	| 500	| 每多少步记录一次日志	| 大模型常用 100-1000 步，避免日志过多 |
| logging_first_step| 	bool	| False	| 是否记录第一步的日志	| 调试时开启，验证训练是否正常 |
| report_to| 	list[str]	| ["tensorboard"]	 | 日志上报工具	| 可选：tensorboard/wandb/comet/mlflow（wandb 是工业界主流） |
| run_name| str	| None	| 训练任务名称（wandb/TensorBoard）	| 方便区分不同实验（如 "llama-7b-lora-lr5e-5"） |


##  Checkpoint 与模型保存配置

| 属性名	类型	| 默认值	| 核心作用	| 工程要点 |
|--------------|------|--------|--------------|
| save_strategy	| str	| "steps"	模型保存策略	| 可选：steps/epoch/no（no = 不保存） |
| save_steps	| int	| 500	| 每多少步保存一次 checkpoint	| 大模型常用 1000-5000 步，避免保存过频占空间 |
| save_total_limit	| int	| None	| 最多保存的 checkpoint 数量	| 设为 3-5，自动删除旧 checkpoint，节省磁盘 |
| save_safetensors	| bool	| True	| 是否用 safetensors 格式保存权重	| 安全、加载快，替代 pytorch_model.bin |
| overwrite_output_dir	| bool	| False	| 是否覆盖输出目录	| 调试时开启，避免手动删除旧文件 |
| resume_from_checkpoint	| str	| None	| 从指定 checkpoint 恢复训练	| 路径如./output/checkpoint-1000 |
