
## from_pretrained

```python3
def from_pretrained(cls, pretrained_model_name_or_path, *inputs, **kwargs):
    ......
```

+ pretrained_model_name_or_path

  指定预训练分词器的来源，支持两种形式：

  1. HF Hub 模型名（如Qwen/Qwen3-7B）；

  2. 本地分词器文件路径（如./local_qwen3_tokenizer）

  类型：str/os.PathLike，默认值：None

+ revision

  指定 HF Hub 上模型的版本（分支 / 提交哈希 / 标签），适配不同版本的分词器，常用取值：main（默认分支）、v1.0（版本标签）、abc123def（提交哈希）

  类型：str，默认值：None

+ trust_remote_code

  是否允许加载模型的自定义代码（关键！Qwen3/LLaMA3/GLM 等模型依赖自定义分词逻辑）

  类型：bool，默认值：False

+ force_download

  强制重新下载分词器文件（忽略本地缓存），用于缓存损坏 / 更新分词器时

  类型：bool，默认值：False

+ resume_download

  断点续传下载分词器文件，适合大文件（如多语言分词器）下载

  类型：bool，默认值：False

+ cache_dir

  指定分词器文件的缓存目录（默认：~/.cache/huggingface/hub），适配企业级存储

  类型：str/os.PathLike，默认值：None

+ local_files_only

  仅从本地缓存加载，不联网访问 HF Hub（离线场景核心）

  类型：bool，默认值：False

+ token

  HF Hub 的访问令牌：

  1. 字符串：直接传入令牌（如hf_xxxxxx）；

  2. True：从环境变量HUGGING_FACE_HUB_TOKEN读取
 
  类型：str/bool，默认值：None

+ proxies

  设置网络代理（访问 HF Hub 需翻墙时），键为协议（http/https），值为代理地址，示例：{"http":"http://127.0.0.1:7890", "https":"http://127.0.0.1:7890"}

  类型：dict，默认值：None

+ download_timeout

  下载文件的超时时间（秒），避免无限等待

+ mirror

+ special_tokens_map

  自定义特殊 Token 映射（覆盖默认），键为 Token 类型，值为 Token 字符串

+ tokenizer_file

  	指定自定义分词器配置文件路径（覆盖自动匹配的文件）

+ unk_token

  自定义未知 Token（覆盖模型默认的 `<

+ pad_token

  自定义 PAD Token（GPT2/Qwen3 默认无 PAD，需手动指定）

+ bos_token

  自定义句首 Token（适配生成模型）

+ eos_token

  自定义句尾 Token（生成模型结束符）

+ mask_token

  自定义掩码 Token（MLM 任务，如 BERT 的[MASK]）
