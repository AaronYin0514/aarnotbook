---
tags: python ai pytorch 
---

# MMDetection

## 简介

[MMDetection](https://github.com/open-mmlab/mmdetection)是商汤科技 (2018 COCO 目标检测挑战赛冠军）和香港中文大学最近开源了一个基于Pytorch实现的深度学习目标检测工具箱。

**mmdetection**，支持`Faster-RCNN`, `Mask-RCNN`, `Fast-RCNN`等主流的目标检测框架，后续会加入`Cascade-RCNN`以及其他一系列目标检测框架。

performance稍高、训练速度稍快、所需显存稍小。

|                     | ResNet | ResNeXt | SENet | VGG | HRNet | RegNetX | Res2Net |
| ------------------- | ------ | ------- | ----- | --- | ----- | ------- | ------- |
| RPN                 | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Fast R-CNN          | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Faster R-CNN        | ✅     | ✅      | -     | ❌  | ✅    | ✅      | ✅      |
| Mask R-CNN          | ✅     | ✅      | -     | ❌  | ✅    | ✅      | ✅      |
| Cascade R-CNN       | ✅     | ✅      | -     | ❌  | ✅    | -       | ✅      |
| Cascade Mask R-CNN  | ✅     | ✅      | -     | ❌  | ✅    | -       | ✅      |
| SSD                 | ❌     | ❌      | ❌    | ✅  | ❌    | ❌      | ❌      |
| RetinaNet           | ✅     | ✅      | -     | ❌  | ✅    | ✅      | -       |
| GHM                 | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Mask Scoring R-CNN  | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Double-Head R-CNN   | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Grid R-CNN (Plus)   | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Hybrid Task Cascade | ✅     | ✅      | -     | ❌  | ✅    | -       | ✅      |
| Libra R-CNN         | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Guided Anchoring    | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| FCOS                | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| RepPoints           | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| Foveabox            | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| FreeAnchor          | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| NAS-FPN             | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| ATSS                | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| ESAF                | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| PAFPN               | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| NAS-FCOS            | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |
| PISA                | ✅     | ✅      | -     | ❌  | ✅    | -       | -       |


## 代码结构

- **configs**：包含了很多网络配置文件
- **tools**：训练＆分析等脚本
- **tests**：测试脚本
- **mmdet**：核心模块
	- **mmdet/apis**: train和inference的検測接口。
	- **mmdet/core**: anchor、bbox、 evaluation等相关实现；
	- **mmdet/datasets**：数据集相关实现。
	- **mmdet/models**：各种检测网络实现函数，基类均来自手pytorch的torch.nn.Module。
	- **mmdet/ops**: roi align、roi pool等相关実現。
- **mmcv**是基础库也是`open-mmlab`项目的一部分
	- 和 deep learning framework无关的一些工具函数，比如`IO/Image/Video`相关的一些操作；
	- 为PyTorch写的一套训练工具，可以大大减少用户需要亏的代码量，同时让整个流程的定制变得容易。
	- mmcv仅有python的实现，它依赖`numpy`, `pyyaml`, `six`, `addict`,`requests`,`opencv-python`;

























