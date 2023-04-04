# Pipenv 怎么入门？

> 原文：<https://blog.devgenius.io/how-to-get-started-with-pipenv-d574101247d4?source=collection_archive---------11----------------------->

![](img/3c707cdb91d36e3907e72d33849754e1.png)

## 在这篇博文中，我将讨论如何开始使用 [Pipenv](https://pipenv.pypa.io/en/latest/) —一个 python 打包工具。这篇博客遵循 Ubuntu 的工作流程，可以很容易地在 macOS 和 Windows 中复制。让我们开始吧。

# 什么是 [Pipenv](https://pipenv.pypa.io/en/latest/) ？

Pipenv 是 python 的 Python 打包工具，是对使用 [Pip](https://pip.pypa.io/en/stable/) 、 [Venv](https://docs.python.org/3/library/venv.html) 和 requirements.txt 的升级。Pipenv 是将包管理与虚拟环境相结合的一个好方法。

# 为什么我们需要包管理和虚拟环境？

根据维基百科，

> *软件包管理器或软件包管理系统是一组软件工具，它们以一致的方式为计算机操作系统自动执行安装、升级、配置和删除计算机程序的过程。*

软件包管理器自动化了软件包的安装、卸载和维护过程。这有助于开发人员轻松管理项目的依赖关系。

你可以在这里阅读更多关于包管理器的内容。

现在让我们讨论一下虚拟环境，

> *虚拟环境是一个自包含的目录树，包含特定版本 Python 的 Python 安装，以及许多附加包。*

虚拟环境使我们能够为每个项目安装特定的 python。这防止我们过载全局 python 安装，并使我们能够为每个项目使用不同版本的 python。
python 虚拟环境还有助于分离每个项目的独立依赖关系，并在任何项目被专门配置为 Python 版本的情况下防止代码中断。
你可以在这里详细了解虚拟环境[。](https://docs.python.org/3/tutorial/venv.html)

现在，我们已经了解了什么是包管理器以及我们为什么需要它们，让我们开始安装 Pipenv。

# 如何安装 Pipenv？

要安装 pipenv，请打开终端窗口并运行以下命令:

```
$ pip install pipenv
```

# 如何使用 PIpenv 创建虚拟环境？

导航到要在其中创建虚拟环境的目录，打开终端窗口并键入以下命令。

```
$ mkdir my_project
$ cd my_project/
$ pipenv install
```

# 如何使用 PIpenv 启动虚拟环境？

要启动虚拟环境，请在目录中键入以下命令。

```
$ pipenv shell
```

您将看到括号内的项目名称，表明我们已经成功进入所需的 python 虚拟环境。

要退出虚拟环境，我们可以键入:

```
$ exit
```

# 如何检查哪个 Python 安装正在使用中？

要检查正在使用的 python 安装，我们可以使用以下 3 种方法:

## 方法 1:

当 python shell 处于活动状态时，键入以下命令，

```
$ which python
```

这将返回当前正在使用的 python 环境的路径。

## 方法二:

在活动的 python shell 中键入此内容，

```
$ import sys
$ sys.executable
```

这将返回正在使用的 python 安装路径。

## 方法三:

要在不激活 shell 的情况下找到可执行文件的路径，可以使用以下命令:

```
$ pipenv --venv
```

# 如何使用 Pipenv 安装包？

键入以下代码来安装软件包 usign Pipenv。

```
$ pipenv install <package-name>
```

# 如何在不激活当前环境中的虚拟环境的情况下运行 Python 命令？

```
$ pipenv run python
```

要运行文件，请使用以下命令:

```
$ pipenv run  python <file-name>
```

# Pipenv 如何使用 requirements.txt 文件？

要使用 pip 的 requirements.txt 安装依赖项和包，请使用以下命令:

```
$ pipenv install -r <path-of-requirements.txt>
```

# 如何使用 Pipenv 创建 requirements.txt？

以下命令可用于生成 requirements.txt 的内容:

```
$ pipenv lock -r
```

要创建 requirements.txt，我们可以将此输出重定向到 requirements.txt:

```
$ pipenv lock -r > requirements.txt
```

# 如何使用 Pipenv 卸载一个包？

以下命令可用于使用 pipenv 卸载软件包:

```
$ pipenv uninstall <package-name>
```

使用`-all`标志卸载所有软件包。

# 如何使用 Pipenv 删除虚拟环境？

以下命令可用于使用 pipenv 安全删除软件包:

```
$ pipenv -rm
```

# 关于 Pipenv 需要了解的其他几点:

1.  默认情况下，pipenv 在`~/.local/share/virtualenvs/`安装虚拟环境。
2.  要安装一个不应该包含在产品版本中的包，我们可以在安装命令的末尾使用`--dev`标志。
3.  为了检查虚拟环境中的安全漏洞，我们可以使用下面的命令:
    `$ pipenv check`。
4.  可以使用以下命令跟踪项目的所有依赖关系:
    `$ pipenv graph`。

这是 pipenv 上的第一篇博客，我将在更多的博客中详细介绍 pipenv。

如果这有帮助，请竖起大拇指。你可以在 twitter 上关注我，关注我未来的博客。
感谢阅读！