# 为什么是. env？

> 原文：<https://blog.devgenius.io/why-a-env-7b4a79ba689?source=collection_archive---------9----------------------->

为什么 dotenv 文件对您的编码项目很重要？

![](img/acc6cdcd975365b9b0b37a191acacd05.png)

[马修·史密斯](https://unsplash.com/@whale?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

。env 文件，也称为 dotenvs 或环境文件，是其他开发人员的常见建议，许多编码教程在没有完全解释原因的情况下使用它们。在这篇文章中，我们将回顾为什么我们应该使用它们以及它们是如何工作的。

# 这是什么？

dotenv 用于存储“环境变量”,也就是我们配置代码环境所需的变量。这可能包括我们的项目是运行在“生产”模式还是“开发”模式，但最重要的是，API 和数据库密钥。我们需要这些变量来访问我们项目的各种服务，但是不希望其他人能够看到和访问。这方面的一个例子包括您创建的 discord bot 令牌——如果它可以在您的代码中查看，任何人都可以作为您的 bot 登录。

你应该一直这样。使用排除的 env 文件。git 忽略旨在阻止您访问文件的文件或其他文件类型。

# 为什么要用？

dotenv 有助于保持敏感信息的安全，并允许您以一致和有组织的方式管理和使用这些变量。此外，使用. env 文件可以更容易地与他人共享您的代码，因为您可以排除。env 文件，并让每个团队成员或协作者设置他们自己的环境变量。

# 它是如何工作的？

答。env 文件是一个简单的文本文件，它以`KEY=VALUE`对的形式包含一系列特定于环境的变量。在与相同环境中运行的应用程序或服务可以访问和使用这些变量。环境文件。

您可以使用包(比如 python 中的 dotenv)将键值对临时加载到系统环境变量中。如果你使用的是 MacOS，你可以通过输入`export a=b && echo $a`来查看这是如何工作的，这将向你显示你的终端返回`b`。我们本质上是在你的项目规模上这样做，你会注意到当你退出终端并再次加载时，`echo $a`不再做任何事情。

# 我如何设置一个？

“好吧，我被卖了，”我听到你说。让我们用 dotenv 建立一个 python 环境，并在代码中使用它。

首先，我们为 python 包设置了一个环境，并通过运行以下命令下载它们:

```
python3 -m venv venv
source venv/bin/activate
pip3 install python-dotenv
touch main.py
touch .env
```

有了这些代码，我们现在已经创建了一个 python 项目，其中包含 dotenv 包和两个文件 main.py 和. env。

```
PASSWORD=super_secret_password0_0
```

我们的 main.py 文件可能看起来像这样:

```
import os
from dotenv import load_dotenv

load_dotenv() # runs "export key=value" on all our key/value pairs

SECRETPASSWORD=os.environ["PASSWORD"]
print(SECRETPASSWORD) # do not do this in any real projects!!!
```

好了，现在我们的代码输出应该是“super_secret_password0_0 ”,这表明我们的 dotenv 成功了！

当然，在您的项目中，您不应该打印出任何环境文件，只是简单地使用它们来访问各种服务！

# 结论

总之，. env 文件是在一个简单的文本文件中存储特定于环境的变量(如 API 键和数据库密码)的一种便捷方式。这使您能够一致地管理敏感信息，同时维护其安全性。通过使用类似于`dotenv`的包，您可以轻松地将变量从. env 文件加载到您的应用程序或服务的环境中，并使用标准的环境变量语法来访问它们。Dotenvs 使得没有必要将敏感信息硬编码到源代码中，并且便于代码共享。