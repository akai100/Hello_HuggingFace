```python3
def get_peft_model(
    model: PreTrainedModel,
    peft_config: PeftConfig,
    adapter_name: str = "default",
    mixed: bool = False,
    autocast_adapter_dtype: bool = True,
    revision: Optional[str] = None,
    low_cpu_mem_usage: bool = False,
) -> PeftModel | PeftMixedModel:
```

```get_peft_model``` 的作用是将一个普通的大模型（Base Model）包装成一个PEFT模型（如 LoRA 模型）。它会自动完成以下三件事：

**1. 注入适配器（Inject Adapters）**

根据你在 LoraConfig 中定义的 target_modules（如 q_proj, v_proj），在对应的层旁边“外挂”低秩矩阵。

**2. 冻结主模型（Freeze Base Model）**

自动将原始模型的所有参数设置为 requires_grad = False，确保训练时只更新外挂的插件。

**3. 计算可训练参数**

它会计算出当前配置下，有多少参数是真正参与训练的（通常只有 0.1% ~ 1%）。

## 参数

+ model

+ peft_config

+ adapter_name

  + 类型: str (默认值为 "default")

  + 含义: 给这个适配器（插件）起个名字。

  + 进阶用法: PEFT 支持多适配器管理。你可以为一个模型加载多个不同的 LoRA。比如一个叫 "chat_adapter"，一个叫 "code_adapter"，在推理时动态切换。

+ mixed

  + 含义： 是否启用“混合适配器”模式。

  + 用途： 默认情况下，get_peft_model 返回一个 PeftModel。如果设为 True，它可能返回 PeftMixedModel。

  + 场景

    当你需要在一个模型中同时组合多种不同类型的 PEFT 技术（例如同时使用 LoRA 和 Prefix Tuning），或者需要处理非常复杂的模型架构时使用。对于 95% 的微调任务，保持 False 即可。

  + autocast_adapter_dtype

    + 含义： 自动转换适配器的数据类型。

    + 用途： 这是一个非常重要的稳定性参数。当基座模型是半精度（FP16/BF16）或量化（4-bit）时，PEFT 会尝试自动将新添加的适配器层（LoRA 权重）转换为与基座模型计算类型匹配的精度。

    + 建议： 除非你对手动控制权重精度有极高要求，否则请务必保持 True，以防止因类型不匹配（Type Mismatch）导致的训练崩溃。

+ revision

  + 含义： 指定配置文件的版本。

  + 用途： 这主要用于从 Hugging Face Hub 加载特定的代码版本或分支（Git Revision）。如果你是在本地定义 PeftConfig 并直接传给 model，这个参数通常留空。

+ low_cpu_mem_usage

  + 含义： 低 CPU 内存占用模式。

  + 用途： 借用了 transformers 的优化思想。当模型非常巨大时，加载适配器可能会瞬间撑爆系统内存（RAM）。开启此参数会尝试优化适配器初始化过程中的内存分配。
  
  + 场景： 在内存较小的机器（如只有 16GB RAM 的 PC）上微调 70B 及以上规模的模型时建议开启。
