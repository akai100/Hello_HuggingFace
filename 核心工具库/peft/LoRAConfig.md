| 参数名	| 类型	| 作用	| 取值范围 |
|-------|-------|-------|-----------|
| r	| int	| LoRA 低秩矩阵的秩（核心参数），决定适配器的容量： - 越小：参数越少，训练速度快，但表达能力弱 - 越大：参数越多，表达能力强，但易过拟合	| 1~128（常用 8/16/32） |
| lora_alpha |	int/float	| 缩放系数，alpha/r 是 LoRA 适配器的学习率缩放因子，平衡适配器与原模型的贡献	| 通常为 r 的 2~8 倍 |
| lora_dropout	| float	| Dropout 概率，随机丢弃适配器的部分输出，防止过拟合	| 0.0~0.5 |
| bias	| str	| 是否训练模型的偏置项，可选值：- "none"：不训练任何偏置（推荐）- "all"：训练所有偏置- "lora_only"：仅训练 LoRA 适配器的偏置	| "none"/"all"/"lora_only" |
| task_type	| str	| 指定微调任务类型，PEFT 会根据任务适配适配器插入逻辑	核心取值：- "CAUSAL_LM"（因果语言建模，如 Qwen3 生成）- "SEQ_CLS"（文本分类）- "QUESTION_ANSWERING"（问答）- "SEQ2SEQ_LM"（翻译 / 摘要）|

##  进阶可选参数（适配大模型 / 定制化场景）

| 参数名	| 类型	| 作用	| 取值范围 |
|-------------|-------|------------|---------|
| target_modules |	list[str]/str |	指定要插入 LoRA 适配器的模型模块名： - 列表：精准指定模块（如 ["q_proj", "v_proj"]） - "all-linear"：所有线性层 - None：默认适配对应模型的典型模块	 | 模块名列表 / 特殊值 |
| target_linear_suffix	| list[str]	| 通过模块后缀匹配要微调的线性层（补充 target_modules）	| 后缀列表（如 ["proj", "fc"]）|
| layers_to_transform	| list[int]/int	指定要微调的模型层索引：- 列表：仅微调指定层（如 [10,20,30]）- None：微调所有层	| 层索引列表 / None	|
| layers_pattern	| str	| 层匹配的正则表达式，适配不同模型的层命名规则	| 正则字符串	|
| rank_pattern	| dict	| 为不同模块设置不同的 r 值（精细化控制）	{模块名: r 值}	 |
| alpha_pattern	| dict	| 为不同模块设置不同的 lora_alpha 值	| {模块名: alpha 值}	 |
| modules_to_save	| list[str]	 | 指定要额外保存的模块（如分类头、生成头）	| 模块名列表	|

## 兼容性 / 底层参数（一般无需修改）

| 参数名	| 类型	| 作用	| 默认值	何时修改 |
|-------|-------|------|--------------------|
| init_lora_weights	| bool/str	| LoRA 权重初始化方式： - True：默认初始化（正态分布） - "gaussian"：高斯初始化 - "loftq"：LoFTQ 量化初始化（适配 4/8bit）	| True	| 4/8bit 量化微调时设为 "loftq" |
| use_rslora	| bool	| 是否启用 RS-LoRA（秩稳定 LoRA），提升低秩适配性	| False |	小样本微调 / 低 r 值（如 r=4）时启用 |
| use_dora	| bool	| 是否启用 DoRA（动态 LoRA），优化缩放因子	| False	| 大模型（72B+）微调时启用，提升效果 |
| dora_alpha	| float	| DoRA 的缩放系数（仅 use_dora=True 时生效）	| 1.0	| 与 lora_alpha 保持一致 |
| fan_in_fan_out	| bool	| 是否适配「输入输出维度不同」的线性层	| False	| 仅适配 T5/MT5 等模型，Qwen3 设为 False |
| enable_lora	| list[bool]	| 为 LoRA 矩阵的不同部分启用适配器（仅底层使用）	| [True, True, True]	| 无需修改 |
| merge_weights	| bool	| 是否自动融合 LoRA 权重到原模型	| False	| 推理时手动调用 merge_and_unload() 更灵活 |
| base_model_name_or_path	| str	| 原预训练模型路径（PEFT 自动填充，无需手动设）	| None	| 加载适配器时自动读取 |
| peft_type	| str	| PEFT 类型（固定为 "LORA"）	| "LORA"	| 无需修改 |
| revision	| str	| 模型版本（如分支 / 提交哈希）	| None	加载特定版本模型时设置 |
| tokenizer_name_or_path	| str	| 分词器路径（自动填充）	| None	无需手动设 |
