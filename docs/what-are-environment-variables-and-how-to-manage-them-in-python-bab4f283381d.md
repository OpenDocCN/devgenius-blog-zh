# 什么是环境变量，如何在 Python 中管理它们？

> 原文：<https://blog.devgenius.io/what-are-environment-variables-and-how-to-manage-them-in-python-bab4f283381d?source=collection_archive---------8----------------------->

# 作为软件开发初学者，我们经常会遇到“环境变量”这个术语。在这篇博文中，我将尝试解释什么是环境变量，以及如何在 Python 项目开发环境中管理它们。让我们开始吧。
注意:这个博客遵循 Ubuntu 的工作流程，可以很容易地在 MacOS/Windows 中复制，稍有变化。

# 什么是环境变量？

快速的谷歌搜索会让你找到维基百科对环境变量的定义，

> *环境变量是一个动态命名的值，它可以影响计算机上正在运行的进程的行为方式。它们是流程运行环境的一部分。*

将环境变量视为系统范围的变量，它可以存储系统所拥有的资源的位置或值。环境变量可以是系统定义的，也可以是用户定义的。

如果您使用的是 MacOS/Linux，请打开您的终端并运行命令，

```
$ printenv
```

这将快速显示您系统中存在的所有环境变量。这些是系统上运行的程序使用的一些资源值。

# 环境变量有什么用？

在开发我们的项目时，我们的项目可能包含各种不能公开的机密信息，以防止违反我们应用程序的完整性。例如，如果您使用过 Django，您可能知道它用于签署会话 cookies 的密钥，这是应用程序安全性的基础。在处理 web 授权范例中的 JWT 令牌时使用秘密密钥/签名，这应该是保密的。
这些值不应存储在代码库或程序文件中，因为它们可能最终被检查/提交到版本控制系统中，从而对系统完整性构成威胁。
在生产环境中，这些变量在运行应用实例的系统/服务器上设置。我们可以在本地开发系统中将这些变量设置为环境变量，但这是一个不必要的麻烦，可以避免。

# 绕过它的方法是什么？

在本地开发环境中，有多种方法来管理环境变量。在这里，我将展示其中一个方法。

*   创建一个. env 文件，并将您的秘密存储在其中。
*   包括。env 在你的。gitignore 来防止它被签入到您的版本控制系统中。
*   一个例子。env 文件可能如下所示:

```
ALGORITHM=HS256
SECRET_KEY=fajhfkaheufahfjbavaeua26472647yhf93h84go84g7fefaefavaegaebartttb
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

*   现在我们需要一种方法来访问 Python 程序文件中的这些值。这里，我们将使用一个名为 [Pydantic](https://pydantic-docs.helpmanual.io/) 的 Python 库来管理我们的环境变量。
*   继续执行命令`pip install pydantic`，安装“Pydantic”库。
*   创建一个名为`config.py`的文件。
*   在`config.py`内部，从 pydantic 库导入 BaseSettings，并创建一个继承自`BaseSettings`的类`Settings`。
*   在这个类中，键入环境变量名和它们的数据类型，中间用:隔开。
*   在类设置中创建一个名为`Config`的类，并定义一个名为`env_file = ".env"`的变量，该变量存储包含我们之前创建的秘密值的文件名。-`config.py`文件示例如下所示:

```
from pydantic import BaseSettingsclass Settings(BaseSettings):
    secret_key: str
    algorithm: str
    access_token_expire_minutes: int class Config:
        env_file = ".env" settings = Settings()
```

*   您也可以为这些参数定义默认值。示例:`access_token_expire_minutes: int = 30`
*   如上例所示实例化类设置。
*   现在打开包含需要这些秘密值的文件，并通过键入`from .config import settings`将这个`settings`对象导入该文件。
*   现在，我们可以将变量赋值为:

```
ALGORITHM = settings.algorithm
```

*   类似地，对项目中的所有其他变量也这样做。
*   你完了！瞧啊。

您已经成功地避免了将您的项目秘密推给版本控制系统，并且在不影响 OS 安装的情况下建立了您的本地开发环境。

如果这有帮助，请竖起大拇指。随时欢迎建议！感谢阅读。