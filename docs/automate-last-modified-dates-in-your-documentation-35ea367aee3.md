# 自动化文档中的“最后修改”日期

> 原文：<https://blog.devgenius.io/automate-last-modified-dates-in-your-documentation-35ea367aee3?source=collection_archive---------22----------------------->

## 自动化重复的文档任务将消除人为错误，并保持文档的结构化。

本文的目的是解释如何自动更新 XML 数据，在本例中是最后修改日期。我的希望是，你可以利用这一点来建立自己的自动化，以提高自己工作的生产率和准确性。

![](img/22f546fdeb4482ab2fb0ba1bef836f7f.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

## 场景

假设您正在处理一个发布历史页面。发布历史有一部分包含每个 API 的最后修改。
以下名为`lastmodified.xml`的 XML 片段代表发布历史的结构化 XML:

```
<file name="Release history">
        <update name="API01">
                <type>Create</type>
                <returns>TXT</returns>
                <maxSize>1000</maxSize>
                <lastModified>2021-11-07</lastModified>
        </update>
        <update name="API02">
                <type>Update</type>
                <returns>TXT</returns>
                <maxSize>1000</maxSize>
                <lastModified>2021-10-29</lastModified>
        </update>
</file>
```

一位开发人员已经指示您更新文档，使其达到 API 可以创建的最大大小，并从 1000 千字节更新到 1400 千字节。

## 复杂的情况

这个要求很简单。然而，开发者不确定他们什么时候会公开这个改变。
您为最大尺寸更新准备您的更改。然后，在没有警告的情况下，您的开发人员通知您他们已经更新了 API，您的文档也应该更新了。

恐慌模式来袭。你真的更新了你的文档吗？你还需要添加什么吗？上次修改日期是什么时候？

## 解决方案

只需几行代码，您就可以自动更新您的“最后修改字段”,并帮助缓解一些恐慌。

将以下 python 文件添加到与 XML 文件相同的目录中:

```
import xml.etree.ElementTree as ET
from datetime import datetime class TimestampUpdater:
    # Constructor
    def __init__(self, filepath):
        self.meta_file = filepath
        self.tree = ET.parse(self.meta_file) # To get xml data
    def getMetadataTree(self):
        return self.tree # to get xml data root
    def getMetadataRoot(self):
        return self.tree.getroot() # to compare current date to lastmodified date
    def updateLastModified(self):
        today = datetime.now()
        # add your XML paths here
        for testfile in self.getMetadataRoot().findall("update"):
            lastmodified = testfile.find("lastModified")
            previous_update = datetime.strptime(lastmodified.text, "%Y-%m-%d")
            if previous_update < today:
                lastmodified.text = today.strftime("%Y-%m-%d")
                self.getMetadataTree().write(self.meta_file) # print lastmodified date to file
def print_file_content(filename):
    with open(filename, "r") as fh:
        for line in fh:
            print(line.rstrip()) # print diff to console
if __name__ == "__main__":
    # rename your file name here
    metafile = "lastmodified.xml"
    print("\n====Before updating:====")
    print_file_content(metafile)
    updater = TimestampUpdater(metafile)
    updater.updateLastModified()
    print("\n====After updating:====")
    print_file_content(metafile)
```

保存并运行您的 python 脚本来更新您的`lastmodified.xml`文件。

以下是输出示例:

```
<file name="Release history">
        <update name="API01">
                <type>Create</type>
                <returns>TXT</returns>
                <maxSize>1400</maxSize>
                **<lastModified>2022-01-03</lastModified>**
        </update>
        <update name="API02">
                <type>Update</type>
                <returns>TXT</returns>
                <maxSize>1400</maxSize>
                **<lastModified>2022-01-03</lastModified>**
        </update>
</file>
```

XML 属性被正确地更新为今天的日期。现在，您可以发布 XML，而不必担心日期格式应用不正确、忘记关闭 XML 标记或某些内容已经过时。

让我知道你将如何在你自己的文档中利用自动化。我很乐意听到您使用的任何其他想法和工作流程。

## 来源:

我在这篇文章中使用了以下灵感来源:

*   [日期时间](https://docs.python.org/3/library/datetime.html)文档。
*   [元素树 XML](https://docs.python.org/3/library/xml.etree.elementtree.html) 文档。
*   [堆栈溢出](https://stackoverflow.com/questions/63605557/updating-xml-node-value-using-the-condition-in-python)问答。

你也可以使用我的 [GitHub 库](https://github.com/rachfop/Automate-Last-modified-dates/tree/main)来开始这篇文章。