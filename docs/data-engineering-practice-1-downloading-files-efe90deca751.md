# 数据工程实践 1 —下载文件

> 原文：<https://blog.devgenius.io/data-engineering-practice-1-downloading-files-efe90deca751?source=collection_archive---------9----------------------->

![](img/cebd323856e8646f82929fc4bb0f41d1.png)

[西格蒙德](https://unsplash.com/es/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

作为数据工程师，你需要很多不同的技术技能来提高日常工作的效率。当你刚开始学习它们时，有时会感到不知所措。如果你一直在努力寻找开始学习和实施事情的方法，那么这篇文章就是为你写的。

数据无处不在，构建任何数据管道的第一步是从不同的来源获取数据。

这是数据工程实践系列的第 1 部分，我们将看看如何能够从一个`HTTP`源下载多个文件并解压缩它们，用`Python`将它们存储在本地。

我发现这个 github 库很有帮助，里面有所有的练习题，我将在这篇文章和接下来的文章中分享我的解决方案。

[](https://github.com/danielbeach/data-engineering-practice) [## GitHub-Daniel beach/数据工程实践:数据工程实践问题

### 数据工程的主要障碍之一是大量和各种各样的技术技能，可以要求在一个

github.com](https://github.com/danielbeach/data-engineering-practice) 

## 问题

我们得到了一些 URL，我们必须将这些 URL 下载到本地的一个*下载*文件夹中。下载的内容是一个 zip 文件，我们必须从这个 zip 文件中提取 csv 文件并删除这个 zip 文件。

初始代码

```
import requestsdownload_uris = [
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2018_Q4.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2018_Q4.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q1.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q1.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q2.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q2.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q3.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q3.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q4.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q4.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2020_Q1.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2020_Q1.zip')
]def main():
    # your code here
    passif __name__ == '__main__':
    main()
```

## 步骤 1 —在本地创建一个下载文件夹。

```
import os def main():
    directory = "downloads"
    os.mkdir(directory) os.chdir(directory) if __name__ == '__main__':
    main()
```

为了创建一个目录，我们使用 python 的 os 模块，
*os.mkdir* 来创建一个目录，然后我们需要使用 *os.chdir.
将当前目录更改为指向这个位置，如果目录已经存在，将会抛出一个错误。*

## 步骤 2 —从 URL 获取内容

我们使用 requests HTTP 库并使用 get()方法发送 get 请求。get 方法返回一个 requests.response 对象。

```
import osdef main():
    directory = "downloads"
    os.mkdir(directory) os.chdir(directory)
    for uri in download_uris:
        response = requests.get(uri)if __name__ == '__main__':
    main()
```

## 步骤 3-获取文件名

为了从 URL 中提取文件名，我们创建了一个名为 get_file_name 的独立函数，并将 URL 传递给它。

```
file_name = get_file_name(uri)
```

这个函数首先将 URL 字符串分成两部分，并用斜线分开。
rsplit 的语法

> *字符串*。rsplit( *分隔符，最大拆分*)

```
def get_file_name(url):
    li = url.rsplit("/",1)[1]
    file_name = li.split(".")[0]
    return file_name
```

rsplit 的第一个参数是一个斜杠，这意味着它必须根据找到斜杠的时间进行拆分，我们只是对字符串的最后一部分感兴趣，以获得文件名，因此我们将它拆分为两部分。这种拆分的结果将存储在一个列表中，我们需要从这个列表中取出第二个元素。
第一阶段后的输出为

```
[Divvy_Trips_2018_Q4.zip](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2018_Q4.zip')
```

我们还删除了。因此，我们再次根据点分隔符分割这个字符串，并返回文件名。

## 步骤 4 —将 csv 文件写入本地

为了在本地写内容，我们使用 open 方法，其中我们指定第一个参数作为文件名，第二个参数是模式。这里，“w”代表写入，“b”代表字节。

```
open(file_name+'.zip','wb').write(response.content)
```

## 步骤 5 —从 zip 文件中提取 CSV 文件

Python 提供了一个 **zipfile** 模块。

**zipfile** 模块具有根据条件解压单个或多个文件的全部功能。

我们首先获取 zip 对象中所有文件名的列表，并循环每个文件名。
接下来，我们检查文件名是否以'结尾。csv '扩展名，如果是，我们提取该文件。

```
with ZipFile(file_name+'.zip', 'r') as zipObject:
           listOfFileNames = zipObject.namelist()
           for fileName in listOfFileNames:
               if fileName.endswith('.csv'):
                   # Extract a single file from zip
                   zipObject.extract(file_name+'.csv')
```

## 第 6 步—删除 zip

最后，我们通过使用 remove 函数删除所有 zip 文件来清理本地文件。

```
if os.path.exists(file_name+'.zip'):
        os.remove(file_name+'.zip')
```

## 完整的代码—

```
import requests
import os
from zipfile import ZipFiledownload_uris = [
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2018_Q4.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2018_Q4.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q1.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q1.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q2.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q2.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q3.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q3.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q4.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2019_Q4.zip'),
    '[https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2020_Q1.zip'](https://divvy-tripdata.s3.amazonaws.com/Divvy_Trips_2020_Q1.zip')
]def get_file_name(url):
    li = url.rsplit("/",1)[1]
    file_name = li.split(".")[0]
    return file_namedef main():
    directory = "downloads"
    os.mkdir(directory) os.chdir(directory)
    for uri in download_uris:
        response = requests.get(uri) file_name = get_file_name(uri) open(file_name+'.zip','wb').write(response.content) with ZipFile(file_name+'.zip', 'r') as zipObject:
           listOfFileNames = zipObject.namelist()
           for fileName in listOfFileNames:
               if fileName.endswith('.csv'):
                   zipObject.extract(file_name+'.csv')

        if os.path.exists(file_name+'.zip'):
            os.remove(file_name+'.zip') if __name__ == '__main__':
    main()
```

## 结论

我们学习了如何使用 Python 中的请求库，并使用 get 方法获取给定 URL 的内容。我们讲述了操作系统模块的基础知识，它帮助我们执行创建、更改和删除目录和文件等任务。我们还提到了 python 提供的帮助我们提取某些文件的 zipfile 模块。

练习 1 的参考链接

[](https://github.com/danielbeach/data-engineering-practice/tree/main/Exercises/Exercise-1) [## 数据工程实践/练习/练习-1 在主丹尼尔海滩/数据工程实践

### 在第一个练习中，您将练习 Python 技能，并了解一个非常常见的任务...正在下载…

github.com](https://github.com/danielbeach/data-engineering-practice/tree/main/Exercises/Exercise-1) 

数据工程实践—第二部分—[https://medium . com/@ sameekshabhatia 6/data-Engineering-practice-ii-web scraping-and-file-download-with-python-cc2b 89554 d0f](https://medium.com/@sameekshabhatia6/data-engineering-practice-ii-webscraping-and-file-downloading-with-python-cc2b89554d0f)

谢谢👋🏻