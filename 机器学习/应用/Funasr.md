# [Funasr语音识别](https://github.com/modelscope/FunASR)

### 环境

```shell
pip3 install -U funasr
# 下载模型用
pip3 install modelscope
```

### 非实时语音识别

```python
from funasr import AutoModel
from funasr.utils.postprocess_utils import rich_transcription_postprocess

model_dir = "iic/SenseVoiceSmall"

model = AutoModel(
    model=model_dir,
    vad_model="fsmn-vad",
    vad_kwargs={"max_single_segment_time": 30000},
    device="cuda:0",
)

# en
res = model.generate(
    input=f"{model.model_path}/example/en.mp3",
    cache={},
    language="auto",  # "zn", "en", "yue", "ja", "ko", "nospeech"
    use_itn=True,
    batch_size_s=60,
    merge_vad=True,  #
    merge_length_s=15,
)
text = rich_transcription_postprocess(res[0]["text"])
print(text)
```

参数说明：

- `model_dir`：模型名称，或本地磁盘中的模型路径。
- `vad_model`：表示开启VAD，VAD的作用是将长音频切割成短音频，此时推理耗时包括了VAD与SenseVoice总耗时，为链路耗时，如果需要单独测试SenseVoice模型耗时，可以关闭VAD模型。
- `vad_kwargs`：表示VAD模型配置,`max_single_segment_time`: 表示`vad_model`最大切割音频时长, 单位是毫秒ms。
- `use_itn`：输出结果中是否包含标点与逆文本正则化。
- `batch_size_s` 表示采用动态batch，batch中总音频时长，单位为秒s。
- `merge_vad`：是否将 vad 模型切割的短音频碎片合成，合并后长度为`merge_length_s`，单位为秒s。
- `ban_emo_unk`：禁用emo_unk标签，禁用后所有的句子都会被赋与情感标签。