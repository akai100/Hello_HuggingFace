
## 方法

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
  
  要处理的原始文本（单句 / 批量文本），最核心的输入参数

  类型：str/List[str]，默认值：None
  
+ text_pair

  可选的第二文本（用于句子对任务，如文本匹配、问答），示例：text_pair="How are you?"（配合 text="Hello"）

  类型：str/List[str]，默认值：None

+ text_target
+ text_pair_target
+ add_special_tokens

  是否自动添加模型所需的特殊 Token（如 BERT 的[CLS]/[SEP]、GPT 的<bos>/<eos>）。

  类型：bool，默认值：True

+ padding

  是否对文本做 Padding 补全，取值分三类：

  - False：不补全
  - True：补到批次最长长度
  - "max_length"：补到max_length指定长度

  类型：bool/str，默认值：False
  
+ truncation

  是否截断超长文本，取值分三类：

  - False：不截断（超长会报错）

  - True：截断到max_length/
  
  模型默认长度

  - "only_first"：仅截断第一个句子（句子对任务）

  类型：bool/str，默认值：False

+ max_length

  文本的最大长度（截断 / 补全的目标长度），None 则用模型默认长度（如 BERT=512）

  类型：int/None，默认值：None

+ stride

  截断超长文本时的滑动窗口步长（仅对truncation=True生效），用于处理超长文本的上下文保留

  类型：int，默认值：0

+ is_split_into_words

  是否已将文本拆分为单词（无需分词器再分词）
  
+ pad_to_multiple_of

  将补全后的长度对齐到指定倍数（如 8/16），适配硬件加速（Tensor Core）

  类型：int/None，默认值：None

+ padding_side
+ return_tensors

  是否返回张量，以及返回哪种框架的张量：
  - None：返回 Python 列表
  - "pt"：PyTorch 张量
  - "tf"：TensorFlow 张量
  - "np"：NumPy 数组

  类型：str/None，默认值：None

+ return_token_type_ids

  是否返回token_type_ids（分句张量，如 BERT 区分句子 1/2）

  类型：bool，默认值：True

+ return_attention_mask

  是否返回attention_mask（掩码张量，1 表示有效 Token，0 表示 PAD）

  类型：bool，默认值：True

+ return_overflowing_tokens

  是否返回超长文本截断后的溢出 Token（用于长文本分片）

  类型：bool，默认值：False
  
+ return_special_tokens_mask

  是否返回特殊 Token 掩码（1 表示特殊 Token，0 表示普通 Token）

  类型：bool，默认值：False

+ return_offsets_mapping

  是否返回 Token 与原始文本的字符偏移映射（用于定位 Token 在原文的位置）

  类型：bool，默认值：False

+ return_length

  是否返回处理后文本的长度（仅对列表输出生效）

  类型：bool，默认值：False
