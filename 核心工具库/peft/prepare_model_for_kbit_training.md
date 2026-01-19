```python3
```

当你使用 bitsandbytes 将模型加载为 4-bit 或 8-bit 时，模型的权重被冻结且精度极低。如果你直接在其上添加 LoRA 适配器并开始训练，通常会遇到以下问题：

+ 梯度无法流回： 某些层可能不支持梯度计算

+ 精度不稳定： 低精度训练容易导致模型崩溃

+ 显存溢出： 激活值（Activations）占据大量内存

2. 它具体做了什么？

当你执行这行代码时，PEFT 在幕后完成了四件大事：

+ 强制转换 LayerNorm 为 FP32

  将所有的层归一化（Layer Normalization）层转换为 32 位浮点数。这对于训练稳定性至关重要，因为量化的 LayerNorm 会引入巨大的舍入误差。

+ 开启输出层梯度

  使输出层（Output Embedding）能够产生梯度，确保 LoRA 适配器能接收到更新信号。

+ 支持梯度检查点 (Gradient Checkpointing)

  如果你开启了梯度检查点（为了省显存），它会确保模型兼容 model.gradient_checkpointing_enable()。

+ 冻结原始参数

  确保基础模型的量化权重保持不变（Requires Grad = False），只让后续添加的 LoRA 插件可训练。
