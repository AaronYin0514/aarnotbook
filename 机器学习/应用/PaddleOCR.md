---
tags:
  - ocr
---
# [PaddleOCR](https://paddlepaddle.github.io/PaddleOCR/index.html)

## 文本检测

### 环境

参考文档[快速开始](https://paddlepaddle.github.io/PaddleOCR/ppocr/quick_start.html)的[飞浆官网安装文档](https://www.paddlepaddle.org.cn/install/quick?docurl=undefined)安装对应系统的paddlepaddle

```python
# mac cpu
python -m pip install paddlepaddle==3.0.0b1 -i https://www.paddlepaddle.org.cn/packages/stable/cpu/

# 安装paddleocr
pip install paddleocr
```

## 测试

可以下载[官方demo](https://paddleocr.bj.bcebos.com/dygraph_v2.1/ppocr_img.zip)

```python
from paddleocr import PaddleOCR, draw_ocr

# Paddleocr目前支持的多语言语种可以通过修改lang参数进行切换
# 例如`ch`, `en`, `fr`, `german`, `korean`, `japan`
ocr = PaddleOCR()  # need to run only once to download and load model into memory
img_path = './ppocr_img/ch/ch.jpg'
result = ocr.ocr(img_path, cls=False)
for idx in range(len(result)):
    res = result[idx]
    for line in res:
        print(line)
```

显示结果，**一定要有font文件**：

```python
# 显示结果
from PIL import Image
result = result[0]
image = Image.open(img_path).convert('RGB')
boxes = [line[0] for line in result]
txts = [line[1][0] for line in result]
scores = [line[1][1] for line in result]
im_show = draw_ocr(image, boxes, txts, scores, font_path='./fonts/simfang.ttf')
im_show = Image.fromarray(im_show)
im_show.save('./result.jpg')
```

![](pp-ocr.jpg)


