# 进行命名实体识别的最佳方式(NER)

> 原文：<https://blog.devgenius.io/the-best-way-to-do-named-entity-recognition-ner-d1d7905bc8ba?source=collection_archive---------3----------------------->

## 自然语言处理

## 在 Python 中进行命名实体识别的两种方法

*最初发表在 PythonAlgos 上的为* [*如何用 Python 做命名实体识别 NER*](https://pythonalgos.com/the-best-way-to-do-named-entity-recognition-ner/)

![](img/bfe20bd0d6d2efe7aa0799657436198f.png)

命名实体识别(NER)是一种常见的自然语言处理技术。它经常被使用，以至于它出现在[空间](https://spacy.io/)的基本管道中。NER 可以帮助我们快速解析出许多不同类型的所有命名实体的文档。例如，如果我们正在阅读一篇文章，我们可以使用命名实体识别来立即了解文章的作者/内容/时间/地点。

在本帖中，我们将介绍用 Python 实现 NER 的三种不同方式。我们将回顾:

*   [什么是命名实体识别(NER)](https://pythonalgos.com/the-best-way-to-do-named-entity-recognition-ner/#what-is-named-entity-recognition)
*   可识别的命名实体的列表
*   我如何实现 NER？
*   [空间命名实体识别](https://pythonalgos.com/the-best-way-to-do-named-entity-recognition-ner/#spacy-named-entity-recognition)
*   [使用 NLTK 进行命名实体识别](https://pythonalgos.com/the-best-way-to-do-named-entity-recognition-ner/#nltk-named-entity-recognition)
*   实现 NER 的最佳方式

# 什么是命名实体识别？

命名实体识别，简称 NER，是关于识别文本文档或语音文件中实体的自然语言处理(NLP)主题。当然，这是一个相当循环的定义。为了理解 NER 到底是什么，我们必须定义实体是什么。出于 NLP 的目的，实体本质上是一个名词，它定义了一个个体、一组个体或一个可识别的对象。虽然对于存在哪种类型的实体还没有完全一致的意见，但我已经编译了一个相当完整的列表，列出了流行的 NLP 库，如 [spaCy](https://spacy.io/) 或自然语言工具包( [NLTK](https://www.nltk.org/) )可以识别的实体的可能类型。你可以在这里找到 [GitHub 回购。](https://github.com/ytang07/intro_nlp)

# 常见命名实体列表

实体类型 NER 对象的描述人物通常被认为是名和姓民族或宗教/政治团体设施的名称组织的名称地理政治实体的名称当地位置产品产品事件的名称事件艺术品的名称艺术品的名称已公布的法律(据我所知仅限美国)语言名称日期日期不一定是确切的日期， 可能是一个相对日期，如“一天前”TIMEA 时间，如日期不一定要精确，可能是“一天当中”百分比金额，如“100 美元”数量重量或距离的计量单位卡号，类似于数量但不是计量单位序数，但表示相对位置，如“第一”或“第二”

# 如何用 Python 实现 NER？

前面，我提到过你可以用 spaCy 和 NLTK 实现 NER。这些库之间的区别在于 NLTK 是为学术/研究目的而构建的，而 spaCy 是为生产目的而构建的。两者都可以免费使用开源库。利用这些开源库，NER 非常容易实现。在本文中，我将向您展示如何开始实现您自己的命名实体识别程序。

# 空间命名实体识别(NER)

我们将从 spaCy 开始，首先在您的终端中运行下面的命令来安装库并下载一个初学者模型。

```
pip install spacy
python -m spacy download en_core_web_sm
```

我们可以用几行代码在空间中实现 NER。我们需要做的就是导入 spacy 库，加载一个模型，给它一些要处理的文本，然后调用处理过的文档来获得我们的命名实体。在本例中，我们将使用之前下载的“en_core_web_sm”模型，这是一个针对 web 文本的“小型”模型。我们将使用的文本只是一些我随机编造的句子，我们应该期待 NER 识别莫莉·穆恩是一个人(NER 还没有先进到检测到她是一头牛)，识别联合国是一个组织，而气候行动委员会是第二个组织。

```
spacy named entity recognitionimport spacy

nlp = spacy.load("en_core_web_sm")

text = "Molly Moon is a cow. She is part of the United Nations Climate Action Committee."

doc = nlp(text)

for ent in doc.ents:
    print(ent.text, ent.label_)
```

运行后，我们应该会看到如下结果。我们看到，这种空间模式无法将联合国及其气候行动委员会作为独立的组织分开。

![](img/a2800a57f8440985748809fa2fd2c7c3.png)

# 基于 NLTK 的命名实体识别

让我们看看如何用 NLTK 实现 NER。和 spaCy 一样，我们将从安装 NLTK 库和下载我们需要的扩展开始。

```
pip install nltk
```

在我们运行初始 pip 安装之后，我们需要下载四个扩展来运行我们的命名实体识别程序。我建议在您的终端启动 Python 并运行这些命令，因为这些库只需要下载一次就可以工作，所以将它们包含在您的 NER 程序中只会降低速度。

```
python
>>> import nltk
>>> nltk.download(“punkt”)
>>> nltk.download(“averaged_perceptron_tagger”)
>>> nltk.download(“maxent_ne_chunker”)
>>> nltk.download(“words”)
```

Punkt 是一个识别标点符号的标记器包。平均感知器标签是 NLTK 的默认词性标签。Maxent NE Chunker 是 NLTK 的命名实体 Chunker。单词库是一个 NLTK 单词语料库。我们在这里已经可以看到，NLTK 的可定制性更强，因此设置起来也更复杂。让我们深入这个程序，看看如何提取我们的命名实体。

我们再次简单地从导入我们的库和声明我们的文本开始。然后，我们将标记文本，标记词性，并使用命名实体分块器将其分块。最后，我们将遍历我们的块并显示那些被标记的块。

```
named entity recognition nltkimport nltk

text = "Molly Moon is a cow. She is part of the United Nations' Climate Action Committee."

tokenized = nltk.word_tokenize(text)
pos_tagged = nltk.pos_tag(tokenized)
chunks = nltk.ne_chunk(pos_tagged)
for chunk in chunks:
    if hasattr(chunk, 'label'):
        print(chunk)
```

当您在终端上运行这个程序时，您应该会看到如下所示的输出。

![](img/b0bc7dc13cc55af3e2a243efd4cf6e66.png)

请注意，NLTK 将“气候行动委员会”标识为一个人，将 Moon 标识为一个人。这显然是不正确的，但这都是基于预先训练的数据。这一次，我让它打印出整个程序块，它显示了词类。NLTK 将所有这些标记为“NNP ”,表示专有名词。

# 更简单、更精确的 NER 实现

好了，现在我们已经讨论了如何使用开源库实现 NER，让我们来看看如何在不下载额外的包和机器学习模型的情况下实现它！我们可以简单地 ping 一个 web API，它已经有了一个预先训练好的模型和管道来满足大量的文本处理需求。我们将使用[文本 API](http://www.thetextapi.com/) 的公开测试版，向下滚动到页面底部并获得您的 API 密钥。

我们需要安装的唯一库是请求库，我们只需要能够发送 API 请求，如[如何发送 Web API 请求](https://pythonalgos.com/2021/11/03/how-to-send-a-web-api-request-in-python/)中所述。那么，让我们来看看代码。

我们所需要的就是构造一个发送到端点的请求，发送请求，并解析响应。API 键应该作为“API key”在头中传递，我们还应该指定内容类型是 json。主体只需要传递文本。我们将要到达的终点是 https://app.thetextapi.com/text/ner 的。一旦我们得到请求，我们将使用 json 库(Python 的原生库)来解析我们的响应。

```
named entity recognition with a web apiimport requests
import json
from config import apikey

text = "Molly Moon is a cow. She is part of the United Nations' Climate Action Committee."
headers = {
    "Content-Type": "application/json",
    "apikey": apikey
}
body = {
    "text": text
}
url = "https://app.thetextapi.com/text/ner"

response = requests.post(url, headers=headers, json=body)
ner = json.loads(response.text)["ner"]
print(ner)
```

一旦我们发送了这个请求，我们应该会看到如下所示的输出。

![](img/def599756924a19fd894a5d568499a40.png)

哇哦。我们的 API 实际上成功地识别了所有三个命名实体！使用文本 API 不仅比下载多个模型和库更简单，而且在这个用例中，我们可以看到它也更准确。

# 进一步阅读

*   [Firebase Auth + FastAPI 与 Pyrebase 和 Firebase Admin](https://pythonalgos.com/python-firebase-authentication-integration-with-fastapi/)
*   [自然语言处理——什么是文本极性？](https://pythonalgos.com/natural-language-processing-what-is-text-polarity/)
*   [用 Python 从头开始编写神经网络代码](https://pythonalgos.com/create-a-neural-network-from-scratch-in-python-3/)
*   [如何创建高级设计文档](https://pythonalgos.com/how-to-create-a-high-level-design-document/)
*   [用 Python 构建你自己的人工智能文本摘要器](https://pythonalgos.com/build-your-own-ai-text-summarizer-in-python/)

如果你喜欢这篇文章，请在 Twitter 上分享！为了无限制地访问媒体文章，今天就注册成为[媒体会员](https://www.medium.com/@ytang07/membership)！别忘了关注我，[唐](https://www.medium.com/@ytang07)，获取更多关于增长、技术等方面的文章！