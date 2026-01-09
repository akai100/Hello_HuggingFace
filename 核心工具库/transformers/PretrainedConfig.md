
```python3
transformers.(output_hidden_states: bool = False,
              output_attentions: bool = False,
              return_dict: bool = True,
              torchscript: bool = False,
              dtype: Optional[Union[str, "torch.dtype"]] = None,
              pruned_heads: Optional[dict[int, list[int]]] = None,
              tie_word_embeddings: bool = True,
              chunk_size_feed_forward: int = 0,
              is_encoder_decoder: bool = False,
              is_decoder: bool = False,
              cross_attention_hidden_size: Optional[int] = None,
              add_cross_attention: bool = False,
              tie_encoder_decoder: bool = False,
              # Fine-tuning task arguments
              architectures: Optional[list[str]] = None,
              finetuning_task: Optional[str] = None,
              id2label: Optional[dict[int, str]] = None,
              label2id: Optional[dict[str, int]] = None,
              num_labels: Optional[int] = None,
              task_specific_params: Optional[dict[str, Any]] = None,
              problem_type: Optional[str] = None,
              # Tokenizer kwargs
              tokenizer_class: Optional[str] = None,
              prefix: Optional[str] = None,
              bos_token_id: Optional[int] = None,
              pad_token_id: Optional[int] = None,
              eos_token_id: Optional[int] = None,
              sep_token_id: Optional[int] = None,
              decoder_start_token_id: Optional[int] = None,)
```

## 属性

+ vocab_size, int

  词表大小，模型能处理的唯一 token 数量

+ hidden_size, int

  隐藏层维度（核心维度，如 BERT 为 768，Llama-7B 为 4096），是每一层神经元的输出维度，在 Transformers 架构中，
  通常特指Encoder/Decoder 的 Transformer Block（包含多头注意力、前馈网络、残差连接、LayerNorm），num_hidden_layers 就是这类 Block 的数量。

+ num_attention_heads, int

  注意力头数，需能被 hidden_size 整除

+ num_hidden_layers, int

  隐藏层 / Transformer 层的数量

+ hidden_act, str

  默认值："gelu"，隐藏层激活函数（如 gelu、relu、silu、gelu_new）

+ max_position_embeddings, int, 默认值：521

  模型支持的最大序列长度（上下文窗口大小），是模型能处理的输入序列的最大 token 数量 —— 超过这个长度的文本，模型要么无法处理，要么会丢失信息（截断），要么位置编码失效导致性能暴跌。

  模型的最大序列长度由位置编码的设计决定，这是硬性约束：

  + 固定位置编码（如 BERT 的正弦编码）：位置编码的维度是max_position_embeddings，超过该长度后无对应的位置编码，模型无法识别 token 的位置；
  
  + RoPE 编码（如 LLaMA/GPT）：理论上可扩展，但原生训练的max_position_embeddings决定了模型的最优性能区间（扩展后需通过 NTK/RoPE 缩放补偿）；

  + ALiBi 编码：无严格的最大长度限制，但训练时的序列长度决定了注意力偏置的有效性。

+ initializer_range, float, 0.02

  权重初始化的标准差

+ layer_norm_eps, float 1e-12

  LayerNorm 层的 ϵ（避免除零，保证数值稳定性）

+ pad_token_id, int, None

  padding token 的 ID（必填，否则模型无法处理变长序列）

+ bos_token_id, int, None

  句子开始（Begin of Sentence）token 的 ID

+ eos_token_id, int, None

  句子结束（End of Sentence）token 的 ID

+ sep_token_id, int, None

  句子分隔 token 的 ID（如 BERT 的 [SEP]）

+ cls_token_id, int, None

  分类 token 的 ID（如 BERT 的 [CLS]）

+ unk_token_id, int, None

  未知 token 的 ID

+ gradient_checkpointing, bool, False

  是否启用梯度检查点（节省显存，大模型训练常用）

+ use_cache, bool, True

  是否使用 KV 缓存（加速推理，生成式模型必备）

+ architectures, list[str], None

  模型架构名称（如 ["BertForMaskedLM"]）

+ model_type, str, -(必填)

  模型类型（如 "bert"、"gpt2"、"llama"）

+ torch_dtype, str/torch.dtype, None

  模型权重的数据类型（如 "float32"、"bfloat16"）

+ tie_word_embeddings,	bool,	True

  是否共享输入输出词嵌入层权重（生成式模型常用）

+ resid_pdrop	float	0.1

  残差连接的 dropout 概率

+ attn_pdrop	float	0.1

  注意力层的 dropout 概率

+ embd_pdrop	float	0.1

  嵌入层的 dropout 概率

+ num_labels	int	2

  分类任务的标签数量（如分类、回归任务）

+ id2label	dict	None

  标签 ID 到标签名的映射（如 {0: "neg", 1: "pos"}）

+ label2id	dict	None

  标签名到标签 ID 的映射（与id2label互逆）

+ finetuning_task	str	None

  微调任务名称（如 "sst2"、"ner"）

+ transformers_version	str	None

  适配的 Transformers 库版本

+ is_decoder	bool	False

  是否为解码器（如 GPT、Llama 为 True，BERT 为 False）

+ is_encoder_decoder	bool	False

  是否为编码器 - 解码器架构（如 T5、BART）

+ cross_attention_hidden_size	int	None

  跨注意力层的隐藏维度（仅编码器 - 解码器架构用）

+ add_cross_attention	bool	False

  是否添加跨注意力层（仅解码器用）

## 功能

### ```name_or_path```

### ```save_pretrained```

```python3
def save_pretrained(self, save_directory: Union[str, os.PathLike], push_to_hub: bool = False, **kwargs)
```

### ```from_pretrained```

```python3
def from_pretrained(
        cls: type[SpecificPretrainedConfigType],
        pretrained_model_name_or_path: Union[str, os.PathLike],
        cache_dir: Optional[Union[str, os.PathLike]] = None,
        force_download: bool = False,
        local_files_only: bool = False,
        token: Optional[Union[str, bool]] = None,
        revision: str = "main",
        **kwargs,
    ) -> SpecificPretrainedConfigType:
```

### ```get_config_dict```

```python3
def get_config_dict(
        cls, pretrained_model_name_or_path: Union[str, os.PathLike], **kwargs
    ) -> tuple[dict[str, Any], dict[str, Any]]:
```

### ```from_dict```

```python3
def from_dict(
        cls: type[SpecificPretrainedConfigType], config_dict: dict[str, Any], **kwargs
    ) -> SpecificPretrainedConfigType:
```

### ```from_json_file```

```python3
def from_json_file(
        cls: type[SpecificPretrainedConfigType], json_file: Union[str, os.PathLike]
    ) -> SpecificPretrainedConfigType
```

### ```to_diff_dict```

```python3
def to_diff_dict(self) -> dict[str, Any]:
```

### ```to_dict```

```python3
def to_dict(self) -> dict[str, Any]:
```

### ```to_json_string```

```python3
def to_json_string(self, use_diff: bool = True) -> str:
```
  

  

  

  

  

  
