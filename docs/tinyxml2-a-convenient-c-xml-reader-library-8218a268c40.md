# TinyXML2:一个方便的 C++ XML 库

> 原文：<https://blog.devgenius.io/tinyxml2-a-convenient-c-xml-reader-library-8218a268c40?source=collection_archive---------12----------------------->

## 高层次的解释，随后是带有错误检查的便捷代码片段的示例

# 它的作用。

简而言之，TinyXML-2 解析一个 XML 文档，并从中构建一个可以读取、修改和保存的文档对象模型(DOM)。[1]

## 什么是 XML？

XML 代表“可扩展标记语言”它是一种通用的人和机器可读的标记语言，用来描述任意数据。[1]

## 为什么使用 XML？

所有那些为存储应用程序数据而创建的随机文件格式都可以用 XML 替换。一个解析器处理所有事情。[1]

![](img/4c18a54834126aaa8808b8d73b8e8eaa.png)

[2]标准是如何扩散的。[https://xkcd.com/927/](https://xkcd.com/927/)

## TinyXML2 怎么用？

有不同的方法来访问 XML 数据并与之交互。TinyXML-2 使用文档对象模型(DOM ),这意味着 XML 数据被解析成可以浏览和操作的 C++对象，然后写入磁盘或另一个输出流。您还可以用 C++对象从头开始构造一个 XML 文档，并将其写入磁盘或另一个输出流。您甚至可以使用 TinyXML-2 从代码中以编程方式流式传输 XML，而无需先创建文档。[1]

## 什么是 DOM(文档对象模型)？

文档对象模型(DOM)是有效的 [*HTML*](https://www.w3.org/TR/DOM-Level-2-Core/glossary.html#dt-HTML) 和格式良好的 [*XML*](https://www.w3.org/TR/DOM-Level-2-Core/glossary.html#dt-XML) 文档的应用程序编程接口( [*API*](https://www.w3.org/TR/DOM-Level-2-Core/glossary.html#dt-API) )。它定义了文档的逻辑结构以及访问和操作文档的方式。在 DOM 规范中，术语“文档”是在广义上使用的——XML 越来越多地被用作表示可能存储在不同系统中的许多不同种类的信息的一种方式，并且这些信息中的大部分在传统上被视为数据而不是文档。然而，XML 将这些数据表示为文档，DOM 可以用来管理这些数据。[3]

使用文档对象模型，程序员可以构建文档、浏览文档结构、添加、修改或删除元素和内容。HTML 或 XML 文档中的任何内容都可以使用文档对象模型来访问、更改、删除或添加，但也有一些例外——特别是，用于 XML 内部和外部子集的 DOM [*接口*](https://www.w3.org/TR/DOM-Level-2-Core/glossary.html#dt-interface) 尚未指定。[3]

## 错误报告

TinyXML-2 报告 XML 文档中无法正确解析的任何错误的行号。此外，所有节点(元素、声明、文本、注释等。)和属性在解析时会记录一个行号。这允许对解析的 XML 文档执行附加验证的应用程序(例如，应用程序实现的 DTD 验证)报告错误消息的行号信息。[1]

## 只需打开并加载 XML 文件

首先，在我们接触 XML 库之前，让我们用纯 C++做一个操作系统检查，看看这个文件是否存在。这是 C++ 98，所以它甚至可以在最简单的 Linux 发行版和最小的 docker 映像上工作。这里没什么特别的。

```
#include <fstream>bool DoesFileExist (const std::string& name);int main (int argc, char *argv[]) {
  std::string file = "path/to/file.xml";
  std::string error = ""; if(DoesFileExist(file) == 0) {
    error= "Error: file " + file+ " does not exist. Exiting.";
    std::cout << error<< std::endl;
    return -1;
  }
}bool DoesFileExist (const std::string& name) {
 std::ifstream f(name.c_str());
 return f.good();
}
```

# 如何获得根节点

```
#include "tinyxml2.h"
#include "tinyxml2.cpp" // I know this is weird, just do itusing namespace tinyxml2;void main() {
    XMLDocument doc;
    doc.LoadFile("file.xml");
    std::string errorString = "Error: " + std::string(doc.ErrorIDToName(doc.ErrorID()));
    if(doc.ErrorID() != XML_SUCCESS) {
        std::cout << errorString << std::endl;
    }
    XMLElement* root = doc.FirstChildElement();}
```

## 关于错误检查和文件 IO 的说明

我发现只要查一下文件就很方便了。if 语句中的 ErrorID()并使用 doc 打印。ErrorIDToName()在我使用 XML lib 做任何事情之后。原因是，对于文件 IO，在任何和每个操作之后检查错误状态通常是非常关键的，这是很好的实践，等等。

文件 io 中可能会出现很多问题，例如，您可能会在加载文件时突然耗尽 RAM，您可能会在写入文件时突然耗尽硬盘空间，等等。

## 错误代码示例:从节点获取属性

```
std::string type = root->Attribute("type"); 
 errorIDString = "Get Type Error: " + std::string(doc.ErrorIDToName(doc.ErrorID()));
 NS_LOG_UNCOND (errorIDString);
 std::cout << "Type: " << type << std::endl;
```

可能会返回以下错误消息:

```
terminate called after throwing an instance of 'std::logic_error'
  what():  basic_string::_M_construct null not valid
```

如果根节点缺少“类型”属性。

在 XML 中，你想要的是:

```
<Root type="myType">
</Root>
```

因此，如果您缺少“类型”属性，它将如下所示:

```
<Root>
</Root>
```

如果没有找到具有指定名称的属性，则`Attribute()`返回`null`。您的代码找不到类型属性，所以它将一个空值传递给值为空的`const char*`构造函数`std::string`。[5]

您应该这样做:

```
// Check event.xml for an attribure called "type" inside the <Event> node
// for production:
//<Event Timespan="3" type="production">
// for staging:
//<Event Timespan="3" type="staging">const char* pType = nullptr;
pType = root->Attribute("standard");
if(pType == nullptr) {
 errorIDString = "Get Type Error: No such attribute 'type'";
 std::cout << errorIDString << std::endl;
} else {
 errorIDString = "Event type: " + std::string(pType);
 std::cout << errorIDString << std::endl;
}
```

## **将数据放入 XMLDocument**

XML 将数据存储在元素中，每个数据单元一个元素。让我们创建另一个名为“版本”的元素。

```
XMLElement * pVersionElement = doc.NewElement("Version");
```

在 XML 文档中创建一个“<version>”节点。</version>

XMLElement 提供了许多重载函数来设置元素的值。XML 是一种基于文本的语言，所以这个函数的名称是 SetText()，尽管各种重载接受任何基本类型来设置元素的值。在这里，我们将使用它来设置一个字符串值。

```
pElement->SetText(10);
```

## 在末尾添加一个新元素

# 实体

TinyXML-2 识别预定义的“字符实体”，即特殊字符。即:

```
&amp;   &
&lt;    <
&gt;    >
&quot;  "
&apos;  '
```

这些在 XML 文档被读取时被识别，并被翻译成它们的 UTF-8 对等物。例如，具有以下 XML 的文本:

```
Far &amp; Away
```

当从 XMLText 对象查询时，将具有“Far & Away”的值()，并将作为&符号写回 XML 流/文件。

## 来自我的示例代码:

我有一个回购协议，在那里我收集代码片段，我做的例子等:[https://github.com/sitting-duck/simple-cpp-xml-parser](https://github.com/sitting-duck/simple-cpp-xml-parser)

目前“主”分支有最新的 Windows 信息,“主”分支有最新的 MacOS 信息和例子，但是我会尽快合并这两个分支。

请随意在这里发表 Github 问题，提出请求或在这里留下评论。

## 限制

目前，我能够使用这个库轻松地解析 C++中的许多 xml 文件，但是由于我还不明白的原因，我无法解析这个文件:

只有 255 行长，我对此感到困惑，但我会寻找解决方法，并尝试另一种解决方案。

# 参考

[1]tinyxml 2 是做什么的？[https://leethomason . github . io/tinyxml 2/#:~:text = In % 20 brief % 2C % 20 tiny XML % 2d 2% 20 parses，language % 20 to % 20 description % 20 arbitrary % 20 data](https://leethomason.github.io/tinyxml2/#:~:text=In%20brief%2C%20TinyXML%2D2%20parses,language%20to%20describe%20arbitrary%20data)。

[2]标准是如何扩散的。【https://xkcd.com/927/ 

[3]什么是文档对象模型？[https://www.w3.org/TR/DOM-Level-2-Core/introduction.html](https://www.w3.org/TR/DOM-Level-2-Core/introduction.html)

[4] TinyXML2 教程。[https://Shiloh James . WordPress . com/2014/04/27/tinyxml 2-tutorial/](https://shilohjames.wordpress.com/2014/04/27/tinyxml2-tutorial/)

[5]如何避免错误:在引发“std::logic_error”的实例后调用 termin ate what():basic _ string::_ S _ construct null 无效。[https://stack overflow . com/questions/11705886/how-to-avoid-the-error-termin ate-called-after-throwing-an-instance-of-STD log](https://stackoverflow.com/questions/11705886/how-to-avoid-the-error-terminate-called-after-throwing-an-instance-of-stdlog)

[6]检查 TinyXML 中是否存在元素。[https://stack overflow . com/questions/62359685/check-for-element-existence-in-tinyxml](https://stackoverflow.com/questions/62359685/check-for-element-existence-in-tinyxml)