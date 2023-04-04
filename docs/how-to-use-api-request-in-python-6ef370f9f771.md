# 如何在 Python 中使用 API 请求

> 原文：<https://blog.devgenius.io/how-to-use-api-request-in-python-6ef370f9f771?source=collection_archive---------3----------------------->

## API 请求 GitHub 使用 Python 获取用户的存储库。

![](img/330e3e27e46b58131d3f0581746d310b.png)

照片由 [AltumCode](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在本文中，我将一步一步地解释处理从 GitHub 获取数据的 API 请求的过程。首先，您需要在计算机上安装 Python。使用下面的链接将 Python 下载到您的本地机器。

[](https://www.python.org/downloads/) [## 下载 Python

### 关于特定端口的信息、开发人员信息来源和二进制可执行文件由发布经理或……

www.python.org](https://www.python.org/downloads/) 

下载并设置好开发环境后，创建您的`main.py`文件。对于本练习，您需要一个名为' **requests** '的第三方 python 包来处理 HTTP 请求。您可以通过下面的命令*安装这个包【使用*[*PyPI*](https://pypi.org/)*查找第三方 python 包】*。

```
$ pip install requests
```

继续之前，请确保您已经在计算机上安装了`pip`。如果您使用的是 Mac，请使用下面的命令在您的 Mac 电脑上安装`pip`。

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py $ python get-pip.py $ pip --version
```

然后，您可以使用下面的命令将包导入到您的`main.py`文件中。

```
import requests
```

在本练习中，我将获取 GitHub 用户**库名**和**库 URL**。为了处理 HTTP 请求，我们可以在包中使用 **get()** 函数。使用下面的命令，并将其分配给一个变量，如下所示。

```
response = requests.get('https://api.github.com/user/repos', auth=('<user>', '<pass>')) <user> : GitHub username
<pass> : GitHub personal access token
```

您可以通过下面给定的路径生成 GitHub 个人访问令牌。

转到您的 GitHub 配置文件 **>** **的**设置**(左侧面板中) **>** **个人访问令牌**>>**生成新令牌**。**

提供一个令牌名称，并根据您的偏好赋予权限，最后，您将能够生成一个访问令牌。在<pass>字段中使用那个令牌 ***【不要使用< >标签】*** 。</pass>

您可以将这个响应作为 JSON 输出，然后将它分配给一个变量，如下所示。

```
my_projects = response.json() # print(type(my_projects)) = <class 'list'>
```

一旦你打印了`my_projects`变量类型，你会看到它是一个列表。然后使用一个简单的 python `for loop`就可以获得用户的 GitHub 库名称和 web URLs，如下所示。

```
for project in my_projects:
    print(f"Project Name: {project['name']}\nProject URL    {project['html_url']}\n")
```

这里，`[‘name’ ]`是返回回购名称的字段，`[‘html_url’]`是从 HTTP 响应*返回回购 web URL 的字段【您可以使用* [*POSTMAN*](https://www.postman.com/downloads/) *应用程序来检查 HTTP 响应及其输出。].*

最后，完整的代码将如下所示。

```
import requestsresponse = requests.get('https://api.github.com/user/repos', auth=('<user>', '<pass>'))my_projects = response.json()for project in my_projects:
    print(f"Project Name: {project['name']}\nProject URL: {project['html_url']}\n")
```

感谢您的阅读！