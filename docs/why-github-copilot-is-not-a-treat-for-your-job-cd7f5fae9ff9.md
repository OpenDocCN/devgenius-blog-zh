# 为什么 GitHub Copilot 不会对你的工作构成威胁

> 原文：<https://blog.devgenius.io/why-github-copilot-is-not-a-treat-for-your-job-cd7f5fae9ff9?source=collection_archive---------1----------------------->

## 如果你是一个优秀的软件设计师，Copilot 对你没有太大的帮助。

![](img/706c5934932b7fd58383ed8f6162de0a.png)

# 什么是 GitHub Copilot？

GitHub Copilot 是一个 AI 对程序员。
它是用庞大的常用小套路编码数据库训练出来的。它还可以识别不好的注释，并根据它们创建命令性代码。

[](/code-smell-05-comment-abusers-feec3aeb926) [## 代码气味 05 —评论滥用者

### 代码有很多注释。注释与实现联系在一起，很难维护。

blog.devgenius.io](/code-smell-05-comment-abusers-feec3aeb926) 

GitHub copilot 是一个类似于 GPT-3 的文本转换器。

它是由同一家公司开发的:OpenAI。

[](https://maximilianocontieri.com/ive-recently-learned-about-gpt3-this-is-my-journey) [## 我最近了解了 GPT 3——这是我的旅程

### 您的浏览器不支持音频元素。自从大约一年前 GPT 3 发布以来，我一直非常兴奋。我…

maximilianocontieri.com](https://maximilianocontieri.com/ive-recently-learned-about-gpt3-this-is-my-journey) 

# 它是如何工作的？

OpenAI Codex 引擎为 GitHub Copilot 提供动力。
它接受了大量源代码和自然语言的训练。

要使用它，我们必须向他们的[等候名单](https://copilot.github.com/)申请。审批过程很快。

我们将它添加为一个 Visual Studio 代码扩展，与 GitHub 实时交互。

# 好处(？)

## 自动填充

一旦我们描述了它们的偶然数据，副驾驶就能预测贫血的结构。

[](/code-smell-70-anemic-model-generators-99aca77391fc) [## 代码气味 70 —贫血模型发电机

### TL；大卫:不要创造贫血的物体。更不用说自动化工具了。

blog.devgenius.io](/code-smell-70-anemic-model-generators-99aca77391fc) 

它们适合于实现性的和缺乏活力的代码生成。

[](/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

blog.devgenius.io](/code-smell-01-anemic-models-f9fb5a1323b3) 

代码向导是一个现实问题。副驾驶是全新的。

[](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) [## 懒惰 II:代码向导

### 代码生成器完成了我们的艰苦工作。但是我们不再需要它们了。

codeburst.io](https://codeburst.io/lazyness-ii-code-wizards-18cc5672b642) 

## 对代码的错误注释

它将不好的注释(那些不应该出现在我们的代码中的注释)转换成简单的算法。

我们可以假设训练集充满了不好的实现注释代码。
我们不应该太依赖算法的声明性。

## 结构测试

CodePilot 可以在 setters 上生成测试。这些测试与实现相关联，并且很脆弱。

[](/code-smell-52-fragile-tests-b917de33fd53) [## 代码气味 52 —易碎测试

### 测试是我们的安全网。如果我们不信任他们的正直，我们将处于极大的危险之中。

blog.devgenius.io](/code-smell-52-fragile-tests-b917de33fd53) 

它们测试我们的 getters，所以它们对验证我们系统的行为没有多大价值。

[](/code-smell-68-getters-68571a0f7fa8) [## 代码气味 68 —吸气剂

### 得到的东西是广泛而安全的。但这是一种非常糟糕的做法。

blog.devgenius.io](/code-smell-68-getters-68571a0f7fa8) 

更多见解[此处](https://goldedem.hashnode.dev/github-co-pilot-in-a-nutshell)。

# 我们应该担心吗？

现在不行。

如果你看了上面的好处，大部分的 Copilot 代码都属于代码气味区。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) 

很快，像 Copilot 这样的变形金刚将会取代懒惰和有执行力的程序员。

[](https://maximilianocontieri.com/most-programmers-are-losing-our-jobs-very-soon) [## (大多数)程序员很快就会失业

### 你是一个声明式程序员还是一个“接近机器”的程序员？如果你在第二组，你会输的…

maximilianocontieri.com](https://maximilianocontieri.com/most-programmers-are-losing-our-jobs-very-soon) 

# 现在应该做什么？

我们需要比它更聪明。

我们需要创造伟大的行为模型，远离可实施的结构数据

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

副驾驶正在解决的问题是解决软件的主要错误。认为编程只是处理数据而不是行为。

[](https://codeburst.io/what-is-software-9a78c1172cf9) [## 软件有什么问题？

### 软件正在吞噬世界。我们这些工作、生活和热爱软件的人通常不会停下来思考它的…

codeburst.io](https://codeburst.io/what-is-software-9a78c1172cf9) 

一旦我们决定长大并构建严肃的软件，而不是处理字符串和日期，我们将推动我们的工作远离这个花哨的机器人几年。

请给我写一行你对此的想法。