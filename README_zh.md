<h2 align="center"> <a href="">DeepFake Defenders</a></h2>
<h5 align="center"> 如果您喜欢我们的项目，请在 GitHub 上给我们一个Star ⭐ 以获取最新更新。  </h5>

<h5 align="center">
    
<!-- PROJECT SHIELDS -->
[![License](https://img.shields.io/badge/License-Apache%202.0-yellow)](https://github.com/VisionRush/DeepFakeDefenders/blob/main/LICENSE) 
![GitHub contributors](https://img.shields.io/github/contributors/VisionRush/DeepFakeDefenders)
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FVisionRush%2FDeepFakeDefenders&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=Visitors&edge_flat=false)](https://hits.seeyoufarm.com)
![GitHub Repo stars](https://img.shields.io/github/stars/VisionRush/DeepFakeDefenders)
[![GitHub issues](https://img.shields.io/github/issues/VisionRush/DeepFakeDefenders?color=critical&label=Issues)](https://github.com/PKU-YuanGroup/MoE-LLaVA/issues?q=is%3Aopen+is%3Aissue)
[![GitHub closed issues](https://img.shields.io/github/issues-closed/VisionRush/DeepFakeDefenders?color=success&label=Issues)](https://github.com/PKU-YuanGroup/MoE-LLaVA/issues?q=is%3Aissue+is%3Aclosed)  <br>

</h5>

<p align='center'>  
  <img src='./images/competition_title.png' width='850'/>
</p>

💡 我们在这里提供了[[英文文档 / ENGLISH DOC](README.md)]，我们十分欢迎和感谢您能够对我们的项目提出建议和贡献。

## 📣 News

* **[2024.09.05]**  🔥 我们正式发布了Deepfake Defenders的初始版本，并在Deepfake挑战赛中获得了三等奖 
[[外滩大会](https://www.atecup.cn/deepfake)].

## 🚀 快速开始
### 一、预训练模型准备
在开始使用之前，请将模型的ImageNet-1K预训练权重文件放置在`./pre_model`目录下，权重下载链接如下:
```
RepLKNet: https://drive.google.com/file/d/1vo-P3XB6mRLUeDzmgv90dOu73uCeLfZN/view?usp=sharing
ConvNeXt: https://dl.fbaipublicfiles.com/convnext/convnext_base_1k_384.pth
```

### 二、 训练

#### 1. 更改数据集路径
将训练所需的训练集txt文件、验证集txt文件以及标签txt文件分别放置在dataset文件夹下，并命名为相同的文件名（dataset下有各个txt示例）
#### 2. 更改超参数
针对所采用的两个模型，在main_train.py中分别需要更改如下参数：
```python
RepLKNet---cfg.network.name = 'replknet'; cfg.train.batch_size = 16
ConvNeXt---cfg.network.name = 'convnext'; cfg.train.batch_size = 24
```

#### 3. 启动训练
##### 单机多卡训练：（8卡）
```shell
bash main.sh
```
##### 单机单卡训练：
```shell
CUDA_VISIBLE_DEVICES=0 python main_train_single_gpu.py
```

#### 4. 模型融合
在merge.py中更改ConvNeXt模型路径以及RepLKNet模型路径，执行python merge.py后获取最终推理测试模型。

#### 5. 推理

示例如下，通过post请求接口请求，请求参数为图像路径，响应输出为模型预测的deepfake分数

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import requests
import json
import requests
import json

header = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36'
}

url = 'http://ip:10005/inter_api'
image_path = './dataset/val_dataset/51aa9b8d0da890cd1d0c5029e3d89e3c.jpg'
data_map = {'img_path':image_path}
response = requests.post(url, data=json.dumps(data_map), headers=header)
content = response.content
print(json.loads(content))
```

### 三、 docker
#### 1. docker构建
    sudo docker build  -t vision-rush-image:1.0.1 --network host .
#### 2. 容器启动
    sudo docker run -d --name  vision_rush_image  --gpus=all  --net host  vision-rush-image:1.0.1

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=VisionRush/DeepFakeDefenders&type=Date)](https://star-history.com/#DeepFakeDefenders/DeepFakeDefenders&Date)
