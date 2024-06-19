# Vits笔记

## 原始项目(python3.7)

### 下载项目

```bash
git clone https://github.com/jaywalnut310/vits.git
```

### 创建虚拟环境

```bash
python 
```

### 修改代码（requirements.txt）

注释掉`torch`和`torchvision`

```python
Cython==0.29.21
librosa==0.8.0
matplotlib==3.3.1
numpy==1.18.5
phonemizer==2.2.1
scipy==1.5.2
tensorboard
#tensorboard==2.3.0
#torch==1.6.0
#torchvision==0.7.0
Unidecode==1.1.1
```

### 确认安装cmake

```bash
sudo apt-get install cmake
```

> 报错`E: Package 'cmake' has no installation candidate`
> 
> `sudo apt-get update -y`
> 
> `sudo apt-get update`
> 
> `sudo apt-get install cmake`
> 

### 安装依赖包

```bash
pip install -r ./requirements.txt
```

### 安装pytorch

```python
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu110
```

### 下载音频素材并创建链接素材目录

```python
ln -s /path/to/LJSpeech-1.1/wavs DUMMY1
```

素材要求

采样率22050 Hz

对应代码 `configs/ljs_base.json`

项目结构

text/chaners.py 清洗器

### 训练

```python
pip install urllib3==1.26.7
```

`train.py`

```python
os.environ['MASTER_PORT'] = '8000'

dist.init_process_group(backend='gloo', init_method='file:///sharefile', world_size=n_gpus, rank=rank)
```

`configs/ljs_base.json`

```python
batch_size:16, # 显存
```

```python
# LJ Speech 单人
python train.py -c configs/ljs_base.json -m ljs_base

# VCTK 多人
python train_ms.py -c configs/vctk_base.json -m vctk_base
```

## 中文项目

clone项目

```python
git clone https://github.com/CjangCjengh/vits.git
```

创建虚拟环境

```python

```

- Fill "text_cleaners" in config.json 选择中文清洗器
- Edit text/symbols.py 选择中文
- Remove unnecessary imports from text/cleaners.py

安装依赖（对比英语项目配置）

预训练

```python
# 1 是1列的意思 
python ./preprocess.py —text_index 1 ./file
```

训练

```python
python ./train.py -c ./configs/chinese_base.json -m 数据目录
```

> 报错`ModuleNotFoundError: No module named 'past'`
> 
> `pip install future`

?
> 报错`ModuleNotFoundError: No module named 'torch.cuda.amp'`
> 

> 报错：`OSError: cannot load library 'libsndfile.so': libsndfile.so: cannot open shared object file: No such file or directory`
> 
> `apt install libsndfile1`

> 报错：ImportError: cannot import name 'Annotated' from 'pydantic.typing' 
> 
> 安装pydantic 1.10.12版本
> 
> `pip install pydantic==1.10.12`


--ignore-installed ruamel.yaml


## 资料

- [本地部署vits](https://www.bilibili.com/read/cv24427456/)
- [*在本地（Windows/Linux）从零开始训练VITS中文AI语音模型到TTS推理的避坑教程指南](https://www.bilibili.com/read/cv21153903/)
- [vits声音合成-本地部署（windows）-自己组合ai的第二步](https://www.bilibili.com/read/cv22856225/)
