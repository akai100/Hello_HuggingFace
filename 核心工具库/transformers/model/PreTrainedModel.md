```python3
PreTrainedModel(config: PretrainedConfig, *inputs, **kwargs)

```

## 主要功能

### 权重加载/保存

#### ```from_pretrained``

加载预训练权重。

```python
def from_pretrained(
        cls: type[SpecificPreTrainedModelType],
        pretrained_model_name_or_path: Optional[Union[str, os.PathLike]],
        *model_args,
        config: Optional[Union[PretrainedConfig, str, os.PathLike]] = None,
        cache_dir: Optional[Union[str, os.PathLike]] = None,
        ignore_mismatched_sizes: bool = False,
        force_download: bool = False,
        local_files_only: bool = False,
        token: Optional[Union[str, bool]] = None,
        revision: str = "main",
        use_safetensors: Optional[bool] = None,
        weights_only: bool = True,
        **kwargs,
    ) -> SpecificPreTrainedModelType:
```

+ config

  自定义配置，覆盖预训练配置；
  

#### ```save_pretrained```

保存微调后的模型

### 设备迁移

```python3
to
```

```python3
cuda()
```

```python3
cpu()
```

### 模型并行

#### ```device_map``

#### ```parallelize```

### 混合精度训练

#### ```half```

#### ```bfloat16```

#### ```float```

### 梯度检查点

#### ```gradient_checkpointing_enable```

### 权重冻结

#### ```requires_grad_```

### 前向传播

#### ```forward```

### 生成

#### ```generate```


