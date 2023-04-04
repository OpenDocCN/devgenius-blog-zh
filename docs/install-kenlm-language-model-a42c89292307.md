# 安装 KenLM 语言模型

> 原文：<https://blog.devgenius.io/install-kenlm-language-model-a42c89292307?source=collection_archive---------1----------------------->

语言模型和 KenLM 安装

# 为什么是这个教程？？？

每一个博客都提出了一些提高我们技能的想法。所以，这个博客会帮助你在 Ubuntu 中“安装 KenLM 库”。在此之前，我们先来看看“什么是语言模型”

语言建模是确定单词序列概率的艺术。这在许多领域都很有用，包括语音识别、光学字符识别、手写识别、机器翻译和拼写纠正。语言模型被广泛应用于自然语言处理中，并且诸如机器翻译之类的应用进行非常频繁的查询。

语言模型能够根据以前的数据预测序列中的下一个单词。语言模型的最好例子是“谷歌根据你以前的搜索推荐下一个单词”。Google 使用的是“BERT”语言模型，它是在几个语料库上训练出来的。

![](img/8c915ed6c7e4c6ea8a54170bd2856c6f.png)

KenLM 语言模型有助于在 Deepspeech 中创建您自己的计分器。

[](https://github.com/mozilla/DeepSpeech) [## mozilla/DeepSpeech

### DeepSpeech 是一个开源的嵌入式(离线、设备上)语音转文本引擎，可以在设备上实时运行…

github.com](https://github.com/mozilla/DeepSpeech) 

Deepspeech 是流行的语音到文本转换引擎之一，可以跨设备实时或离线运行。

![](img/f94519f8955b25a646dbaad44e4a9478.png)

照片由[梅尔特塔拉伊](https://unsplash.com/@merttly?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 安装 KenLM 的步骤

安装 KenLM 库的步骤

## 步骤 1:安装依赖项

```
sudo apt-get update*sudo apt-get install build-essential libboost-all-dev cmake zlib1g-dev libbz2-dev liblzma-dev**sudo apt-get install libboost-all-dev libeigen3-dev*
```

## 步骤 2:克隆 KenLM Git Repo

```
git clone [https://github.com/kpu/kenlm](https://github.com/kpu/kenlm)
```

## **第三步:遍历下载的路径，创建目录**

```
cd kenlm
mkdir build
cd build
```

## 第四步:编译并安装在 Ubuntu 中

```
cmake ..
make -j 4
sudo make install
```

你的 KenLM Bins 在 **"/home/ <用户名> /kenlm/build/bin"**

**参考**

[](https://github.com/kpu/kenlm) [## kpu/kenlm

### 语言模型推理代码由肯尼斯·希菲尔德(肯姆在 kheafield.com)网站…

github.com](https://github.com/kpu/kenlm) 

如果你喜欢这篇文章，对你有帮助，请尽可能地鼓掌并分享

![](img/03aa705a036bfa3d1518215bafbe19b1.png)