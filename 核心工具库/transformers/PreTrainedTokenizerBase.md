## 功能

### ```__call__```

```python3
def __call__(
        self,
        text: Union[TextInput, PreTokenizedInput, list[TextInput], list[PreTokenizedInput], None] = None,
        text_pair: Optional[Union[TextInput, PreTokenizedInput, list[TextInput], list[PreTokenizedInput]]] = None,
        text_target: Union[TextInput, PreTokenizedInput, list[TextInput], list[PreTokenizedInput], None] = None,
        text_pair_target: Optional[
            Union[TextInput, PreTokenizedInput, list[TextInput], list[PreTokenizedInput]]
        ] = None,
        add_special_tokens: bool = True,
        padding: Union[bool, str, PaddingStrategy] = False,
        truncation: Union[bool, str, TruncationStrategy, None] = None,
        max_length: Optional[int] = None,
        stride: int = 0,
        is_split_into_words: bool = False,
        pad_to_multiple_of: Optional[int] = None,
        padding_side: Optional[str] = None,
        return_tensors: Optional[Union[str, TensorType]] = None,
        return_token_type_ids: Optional[bool] = None,
        return_attention_mask: Optional[bool] = None,
        return_overflowing_tokens: bool = False,
        return_special_tokens_mask: bool = False,
        return_offsets_mapping: bool = False,
        return_length: bool = False,
        verbose: bool = True,
        **kwargs,
    ) -> BatchEncoding:
```

+ text

+ padding

  True："longest"（补齐到批次最长）、"max_length"（补齐到 max_length）；

+ truncation

  True 表示截断超过 max_length 的文本

+ max_length

  最大序列长度（需 ≤ 模型的 max_position_embeddings）
