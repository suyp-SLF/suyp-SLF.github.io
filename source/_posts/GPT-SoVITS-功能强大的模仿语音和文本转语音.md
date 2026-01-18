---
title: GPT-SoVITS 功能强大的模仿语音和文本转语音,
date: 2026-01-18 07:49:07
tags:
 -Python
 - GPT
 -SoVITS
 -语音
 -文本转语音
 -声音模仿
categories:
 -Python
 -GPT
 -SoVITS
 -语音
 -文本转语音
---
# GPT-SoVITS 功能强大的模仿语音和文本转语音

## 简介
GPT-SoVITS 是一个功能强大的工具，它结合了 GPT 和 SoVITS 的优势，可以模仿语音和实现文本转语音。它使用 GPT 模型来生成语音，并使用 SoVITS 模型来调整语音的音色和情感。这使得 GPT-SoVITS 能够生成逼真的语音，并且可以根据用户的输入调整语音的音色和情感。

## 功能
GPT-SoVITS 的主要功能包括：模仿语音和文本转语音。它可以模仿任何人的声音，并且可以根据用户的输入调整语音的音色和情感。此外，它还可以将文本转换为语音，并且可以根据用户的输入调整语音的音色和情感。

## 使用方法 MAC
要使用 GPT-SoVITS，您需要先安装 Python 和相关的依赖库。然后，您可以从 GitHub 上下载 GPT-SoVITS 的源代码，并按照 README 文件中的说明进行安装和配置。

## 安装
### 1. 环境准备
```bash
conda create -n sovits python=3.10
conda activate sovits
```
### 2. 源码与依赖下载
```bash
git clone https://github.com/RVC-Boss/GPT-SoVITS.git
cd GPT-SoVITS
pip install -r requirements.txt
```

### 3. 模型权重部署
```bash
将下载好的预训练模型放入对应路径：
GPT 权重：.ckpt
SoVITS 权重：.pth
```
## 完整文件如图
![files](/images/posts/GPT-SoVITS-功能强大的模仿语音和文本转语音/1.png)

### 4.启动
```bash
conda activate sovits
python inference_webui.py
```

## 素材预处理 (FFmpeg)
将视频转为音频
```bash
ffmpeg -i "2026-01-18 06-48-55.mkv" -vn -ac 1 -ar 44100 output.wav
```

## 尝试英文推理时提示 LookupError: Resource averaged_perceptron_tagger_eng not found
```bash
python -c "import nltk; nltk.download('averaged_perceptron_tagger_eng')"
```