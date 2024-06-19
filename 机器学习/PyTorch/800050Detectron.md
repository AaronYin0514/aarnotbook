---
tags: python ai pytorch 
---

# [Detectron](https://github.com/facebookresearch/detectron2)

- 2018年初，Facebook AI研究院（FAIR）公开了一个目标（视觉）检测平台，名叫Detectron（目前已经是V2）。它是一个软件系统，由Python语言和Caffe2深度学习框架构建而成。
- ﻿﻿Mask RCNN, RetinaNet, Faster RCNN, RPN,  
    Keypoints，Panoptic Segmentation等一系列优秀算法框架
- ﻿﻿支持数据集：COCO，PasscalVOC,Cityscapes,LVIS等不同数据集

## 结构介绍

```mermaid
graph LR
A[detectron2 _master] --> B[configs] --> BA(储存各种网络的yaml配置文件)
A--> C[datasets] --> CA[存放数据集的地方]
A--> D[detectron2] --> DA(运行代码的核心组件)
D --> DB[config] --> DBA[compat.py]
DB --> DBB[config.py] --> DBBA[定义CfgNode类]
DBB --> DBBB[提供get.cfg0方法,会返回一个含有默认配置的CfgNode,该默认配置值在default.py中定义]
DB --> DBC[default.py]
DBC --> DBCA[默认配置值]
A--> E[tools] --> EA[提供运行代码的入口以及一切可视化的代码文件]
A--> F[projects] --> FA[提供真实项目代码示例]
```

## 配置

使用[yacs](https://github.com/facebookresearch/detectron2/blob/master/ configs/Base-RCNN-FPN.yaml)来定义配置文件内容

```mermaid
flowchart LR
A["创建配置信息cfg及基本设置setup(args)"]
A --> AA["cfg=get_cfgo"] --> AAA("加载默认参数")
A --> AB["cfg.merge_from_file(args.config_file)"] --> ABA("加载个性化参数配置")
A --> AC["cfg.merge_from_list(args.opts)"]
A --> AD["cfg.freeze0"] --> ADA("冻结所有配置参数，防止程序对其修改")
A --> AE["default setup(cfg,args)"] --> AEA("设置logger信息及输出目录")
```

## 环境配置

## 训练 & 测试

```shell
./train_net.py -﻿-config-file /configs/COCO-InstanceSegmentation/mask_ronn_R_50_FPN_1x.yaml -num-gpus 1 SOLVER.IMS PER _BATCH 2 SOLVER.BASE_LR 0.0025

./train_net.py --config-file./configs/COCO-InstanceSegmentation/mask_ronn_R 50_FPN_1x.yaml --eval-only MODEL.WEIGHTS /path/to/checkpoint_file

.﻿﻿/train_net.Dy -h
```

## 推理

```shell
python demo.py --config-file ../configs/cOCO-InstanceSegmentation/mask_renn_R_50_FPN_3x.yaml --input input1.jpg input2.jpg [--other-options] --opts MODEL.WEIGHTS detectron2://COCO-InstanceSegmentation/mask_ronn_R_50_FPN_3x/137849600/model_final_f10217.pkl
```











