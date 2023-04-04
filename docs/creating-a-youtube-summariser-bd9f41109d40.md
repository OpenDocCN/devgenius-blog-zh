# 创建 Youtube 摘要器

> 原文：<https://blog.devgenius.io/creating-a-youtube-summariser-bd9f41109d40?source=collection_archive---------9----------------------->

![](img/7f3011c59a62abb031d698f853bba94a.png)

自然语言处理，简称 NLP，是目前发展最快的领域之一。NLP 应用广泛，从垃圾邮件过滤到通过聊天机器人进行医疗诊断。文本摘要、聊天机器人、机器翻译、文本生成和 NLP 的其他应用现在很流行。

你有没有想象过在看视频之前，有一个简短的大型 YouTube 教程或视频的摘要，用于快速阅读？通过在短时间内为您提供对视频的快速理解或总结，这无疑会为您节省大量时间。在本文中，我们将看到一个名为 YouTube Summarizer 的小型 NLP 项目，它将总结 YouTube 视频的内容(字幕)。因为许多视频的主要材料只占整个时长的 50-60 %,我们的 YouTube 摘要器将通过保留所有重要部分并使其简短易懂来浓缩视频内容。这将有利于快速获得各种课程视频的摘要。

让我们回顾一下 NLP 研究的基础。

首先，我们来定义总结。摘要是在不遗漏文章主要信息的情况下，为给定的长文本文档创建简洁、易懂的笔记的过程。在自然语言处理中，有两种摘要:抽取式摘要和抽象式摘要。

该系统将从给定的段落中提取重要的段落和内容，并将这些提取的段落组合起来，以在摘要中构建摘要文本。

在抽象摘要中，系统将根据输入内容使用自己的术语生成摘要。这是一个比摘要更复杂的过程。

我们正在为我们的 YouTube 摘要器使用摘要。我们使用各种各样的摘要方法来提取摘要，例如 BART 或双向和自回归转换器、TFIDF 矢量器等等。

**实施**

youtube 摘要生成器的基本结构是，我们使用 python 模块 youtube-Transcript-API 下载所提供的 Youtube 视频的字幕，然后在运行几个摘要算法来总结给定文本之前执行文本预处理技术。

```
import youtube_transcript_api
from youtube_transcript_api import YouTubeTranscriptApi
import nltk
import re
from nltk.corpus import stopwords
import sklearn
from sklearn.feature_extraction.text import TfidfVectorizer
```

第一步是获得被概括的视频的字幕。我们正在用 Python 的 youtube 脚本 api 包做这件事。每个 YouTube 视频都有一个唯一的 Id。例如，如果一个视频的 YouTube 链接是“【https://youtu.be/KIcg738D4L4】T2”，那么唯一的 id 将是“KIcg738D4L4”。这个独一无二的 ID 是我们用来获取字幕的。

```
link = "https://www.youtube.com/watch?v=[KIcg738D4L4](https://youtu.be/KIcg738D4L4)" 
unique_id = link.split("=")[-1]
sub = YouTubeTranscriptApi.get_transcript(unique_id)  
subtitle = " ".join([x['text'] for x in sub])
```

**使用 TF-IDF**

术语频率-逆文档频率(TF-IDF)矢量器将文本转换为矢量。里面有两个词:词频和逆文档频。这两个分量的乘积就是 TF-IDF 值。一个句子中术语的重复次数除以该句子中的总字数就是术语频率。通过将句子数量除以包含所提供单词的句子数量来计算逆文档频率。

```
from nltk.tokenize import sent_tokenize
```

标记化由 nltk 库的句子标记器处理。

```
subtitle = subtitle.replace("n","")
sentences = sent_tokenize(subtitle)
```

现在，我们将标记化的句子放入字典，将句子作为键，将值作为匹配索引。

```
organized_sent = {k:v for v,k in enumerate(sentences)}
```

接下来，我们将使用 tf-idf 矢量器来获取我们在标记化过程中构建的每个句子的得分。

```
tf_idf = TfidfVectorizer(min_df=2, 
                                    strip_accents='unicode',
                                    max_features=None,
                                    lowercase = True,
                                    token_pattern=r'w{1,}',
                                    ngram_range=(1, 3), 
                                    use_idf=1,
                                    smooth_idf=1,
                                    sublinear_tf=1,
                                    stop_words = 'english')sentence_vectors = tf_idf.fit_transform(sentences)
sent_scores = np.array(sentence_vectors.sum(axis=1)).ravel()
```

让我们看看得分较高的前 N 个句子。

```
N = 3
top_n_sentences = [sentences[index] for index in np.argsort(sent_scores, axis=0)[::-1][:N]]
```

现在让我们按照字幕中出现的顺序排列上面的句子。

```
# mapping the scored sentences with their indexes as in the subtitle
mapped_sentences = [(sentence,organized_sent[sentence]) for sentence in top_n_sentences]
# Ordering the top-n sentences in their original order
mapped_sentences = sorted(mapped_sentences, key = lambda x: x[1])
ordered_sentences = [element[0] for element in mapped_sentences]
# joining the ordered sentence
summary = " ".join(ordered_sentences)
```

**使用 BART**

BART(双向自回归变压器)是一种越来越多地用于解决序列间困难的变压器。它的架构主要由一个双向编码器和一个左右解码器组成。BART 可用于摘要、机器翻译、问题回答和其他任务。

首先，我们正在安装变压器和图书馆。

```
!pip install transformersimport transformers
from transformers import BartTokenizer, BartForConditionalGeneration
```

现在，为了进行总结，让我们导入 Bart 预训练标记器和模型。

```
tokenizer = BartTokenizer.from_pretrained('facebook/bart-large-cnn')
model = BartForConditionalGeneration.from_pretrained('facebook/bart-large-cnn')
```

在文章的第一部分，我们下载了字幕。让我们使用 Bart 标记器来编码这个字幕。

```
input_tensor = tokenizer.encode( subtitle, return_tensors="pt", max_length=512)
```

现在，让我们使用 Bart 总结模型来构建输出总结。

```
outputs_tensor = model.generate(input_tensor, max_length=160, min_length=120, length_penalty=2.0, num_beams=4, early_stopping=True)
outputs_tensor
```

输出将是一个张量，必须使用与之前相同的 Bart 记号化器模型进行解码。

```
print(tokenizer.decode(outputs_tensor[0]))
```

**结论**

总的来说，Youtube summarizer 会给我们提供一个简洁的视频梗概，这样可以节省我们很多时间(注意，它适用于有字幕的视频)。我希望你喜欢这个博客以及这个简短的 NLP 应用程序。