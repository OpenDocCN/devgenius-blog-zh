# python——使用 Pip 的 10 个技巧

> 原文：<https://blog.devgenius.io/python-10-tips-for-using-pip-3126537e8a6b?source=collection_archive---------9----------------------->

## 在 Python 中使用 pip 的有用技巧

![](img/90b771b0f88b81ce38bf29b4d58b1349.png)

众所周知，`pip`可以安装、更新、卸载 python 的第三方库，非常方便。你们中的许多人可能已经使用`pip`很长时间了，但是不清楚它有什么好的特性。希望我今天分享的技巧能让你从使用 Python `pip`中受益。

> 如果你对更好版本的`pip` —诗歌感兴趣，可以在这里查看我的文章:“[诗歌，更好版本的 Python Pipenv](https://python.plainenglish.io/poetry-a-better-version-of-python-pipenv-561611a029d1) ”。

# Python Pip

先说 Python 的语言。Python 之所以受欢迎，不仅因为它简单易学，还因为它拥有成千上万的宝库。

这些库相当于已经集成的工具，只要安装好就可以在 Python 中使用。他们可以处理各种各样的问题，而不用你另起炉灶，而且随着社区的不断更新和维护，一些库越来越强大，几乎可以媲美企业级应用。

那么如何下载安装这些工具库呢？它们被放在一个名为`PyPi` ( [Python 包索引](https://pypi.org/))的统一“仓库”中，所有的库安装都从这里分发出去。

有了存储库之后，还需要一个管理员，`pip`就是这样一个角色。`pip`将库从 PyPi 中取出，安装到 Python 中。它还可以管理已安装的库，如更新，查看，搜索，卸载等。

下面总结了 10 个 pip 使用的常识和技巧，供大家参考。

# 1.安装 Pip

从 Python 3.4 开始，`pip`已经内置在 Python 中，所以不需要重新安装。

如果您的 Python 版本没有 pip，您可以使用以下两种方法安装它。

*   在命令行上输入`easy_install pip`，非常快。
*   从以下 URL 下载 pip 安装文件，然后将其解压缩到 python 脚本目录，并执行`python setup.py install`命令。

`pip`下载网址:[https://pypi.org/project/pip/#files](https://pypi.org/project/pip/#files)

但是，如果你还在使用 Python3.4 及更早的版本，请升级到 Python 的最新稳定版本([https://www.python.org/downloads/](https://www.python.org/downloads/))。否则，你每天都在增加更多的科技债务。

# 2.升级 pip

如果 pip 版本太低，可以升级当前版本
`pip install --upgrade pip`或`pip install -U pip`。

```
$ pip install -U pip
Looking in indexes: [https://pypi.python.org/simple](https://pypi.python.org/simple)
Requirement already satisfied: pip in ./test/lib/python3.8/site-packages (21.1.1)
Collecting pip
  Using cached pip-22.0.4-py3-none-any.whl (2.1 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 21.1.1
    Uninstalling pip-21.1.1:
      Successfully uninstalled pip-21.1.1
Successfully installed pip-22.0.4
```

# 3.安装库

使用 pip 安装第三方库并执行下面的语句
`pip install package_name`

指定软件包版本:
`pip install package_name==1.1.2`

比如我想安装 3.4.1 版的 matplotlib
`pip install matplotlib==3.4.1`

# 4.库的批量安装

如果一个项目需要安装多个库，可以批量安装:
`pip install -r requirements.txt`

文件内容格式如下:

```
# This is a comment# Specify a diffrent index
-i http://dist.repoze.org/zope2/2.10/simple# Package with versions
tensorflow==2.3.1
uvicorn==0.12.2
fastapi==0.63.0
pkg1
pkg2
pkg3**>=1.0,<=2.0**# *It is possible to refer to specific local distribution paths.*
**./**downloads**/**numpy**-1.9.2-**cp34**-**none**-**win32**.**whl*# It is possible to refer to other requirement files or constraints files.*
**-**r other**-**requirements**.**txt
**-**c constraints**.**txt*# It is possible to specify requirements as plain names.*
pytest
pytest**-**cov
beautifulsoup4
```

# 5.卸载和升级软件包

已安装的库可以再次卸载:
`$ pip uninstall package_name`

和当前库的版本升级

```
$ pip install --upgrade package_name
or
$ pip install -U package_name
```

# 6.**冻结巨蟒皮普依赖**

有时，您希望输出当前环境中所有已安装的软件包，或者生成一个需求文件，然后在另一个环境中安装。你可以使用`pip freeze`命令:

```
# List packages
$ pip freeze
*docutils==0.11*
*Jinja2==2.7.2*
*MarkupSafe==0.19*
*Pygments==1.6*
*Sphinx==1.2.2*# Generate requirements.txt file
$ pip freeze > requirements.txt
```

请注意，包是以不区分大小写的排序顺序列出的。如果您只想列出非全局安装的软件包，请使用`-l/--local`。

# 7.查看库信息

您可以使用`pip show -f package_name`列出包装信息:

```
$ pip show -f pyyaml
Name: PyYAML
Version: 5.4.1
Summary: YAML parser and emitter for Python
Home-page: [https://pyyaml.org/](https://pyyaml.org/)
Author: Kirill Simonov
Author-email: [xi@resolvent.net](mailto:xi@resolvent.net)
License: MIT
Location: /private/tmp/test/lib/python3.8/site-packages
Requires:
Required-by: awscli
Files:
  PyYAML-5.4.1.dist-info/INSTALLER
  PyYAML-5.4.1.dist-info/LICENSE
  PyYAML-5.4.1.dist-info/METADATA
  PyYAML-5.4.1.dist-info/RECORD
  PyYAML-5.4.1.dist-info/WHEEL
  PyYAML-5.4.1.dist-info/top_level.txt
...
```

# 8.查看需要升级的库

在当前安装的库中，查看哪些库需要版本升级:

```
$ pip list -o
Package    Version Latest Type
---------- ------- ------ -----
docutils   0.15.2  0.18.1 wheel
PyYAML     5.4.1   6.0    wheel
rsa        4.7.2   4.8    wheel
setuptools 56.0.0  62.1.0 wheel
```

# 9.检查兼容性问题

验证安装的库的兼容性依赖关系，您可以使用`pip check package-name`:

```
$ pip check awscli
No broken requirements found.
```

如果你不指定软件包名称，它将检查所有软件包的兼容性。

```
$ pip check
*pyramid 1.5.2 requires WebOb, which is not installed.*
```

# 10.将库下载到本地

将库下载到本地指定文件，并以`whl`格式保存:
`pip download package_name -d "path"`

```
$ pip download PyYAML  -d "/tmp/"
Looking in indexes: [https://pypi.python.org/simple](https://pypi.python.org/simple)
Collecting PyYAML
  Downloading PyYAML-6.0-cp38-cp38-macosx_10_9_x86_64.whl (192 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 192.2/192.2 KB 4.7 MB/s eta 0:00:00
Saved ./PyYAML-6.0-cp38-cp38-macosx_10_9_x86_64.whl
Successfully downloaded PyYAML$ ls /tmp/PyYAML-6.0-cp38-cp38-macosx_10_9_x86_64.whl
/tmp/PyYAML-6.0-cp38-cp38-macosx_10_9_x86_64.whl
```