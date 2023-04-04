# 使用 Python | Asyncio & Aiohttp 创建异步 Web 请求

> 原文：<https://blog.devgenius.io/creating-asynchronous-web-requests-with-python-asyncio-aiohttp-d5469d37cc23?source=collection_archive---------6----------------------->

![](img/4895eb8db4137f140230f36f425550f7.png)

因此，在我的[上一篇文章](https://medium.com/@katchyemma/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1)中，我谈到了异步是如何加速任务和减少执行时间的。今天，我们的手有点脏。
我们将使用我最喜欢的 API 之一， [DiceBear Avatar API](https://avatars.dicebear.com/docs) 。它接受一个种子字符串(实际上是任何随机文本)，并返回一个带有随机特征的个人资料图片。有多种风格可供选择，我们可以微调我们想要看到的功能。
使用`curl`的示例用法如下:

```
curl -X GET [https://avatars.dicebear.com/api/big-smile/:seed.png](https://avatars.dicebear.com/api/big-smile/:seed.png) --output DiceBearAvatar.png
```

我们将创建一个 Python 程序来自动化这个过程，并异步生成尽可能多的个人资料图片。为了有一点背景知识，我们将创建该程序的同步版本。

首先，我们需要安装几个库:

```
pip install aiohttp requests
```

我们将需要一些助手函数，我们将把它们放在`utils.py`中。它们将处理像将图像数据写入文件、为 API 调用格式化 URL 以及为我们的程序生成随机字符串这样的事情。

```
# ./utils.py
# --------------------------------
import secrets
from os import path

RANDOM_SEED_SIZE = 10

def generate_random_seed():
 return secrets.token_urlsafe(RANDOM_SEED_SIZE)

def generate_api_url(seed, avatar_style, base_url, extension=".png"):
 url = base_url.format(avatar_style=avatar_style, seed=seed+extension)
 return url

def write_to_file(data: str, file_name: str, file_path: str = "my_profile_pics"):
 try:
  with open(path.join(file_path, file_name+".png"), "wb") as file:
   file.write(data)

 except Exception as e:
  print(f"\n{e}\n")
```

接下来将是实际的 HTTP 请求，我们现在将使用`requests` 库。

```
# ./sync_api.py
# --------------------------------
import requests
import utils  

def create_new_avatar(avatar_style:str, base_url:str):
 seed = utils.generate_random_seed()
 request_url = utils.generate_api_url(seed=seed, avatar_style=avatar_style, base_url=base_url)
 resp = requests.get(request_url)
 data = resp.content
 utils.write_to_file(data=data, file_name=seed, file_path=r"C:/Users/user/desktop/pp")
```

只是标准的 HTTP 请求。我们使用我们的效用函数为 DiceBear 生成一个随机种子，并发出一个 GET 请求。然后，我们从结果响应对象中提取图像数据。
将数据写入实际文件由另一个实用程序处理。如果你想改变文件的存储位置，你可以把`C:/Users/user/desktop/pp`切换到你喜欢的位置。

看起来已经很好了，我们差不多完成了。

现在为这个程序创建一个入口点，`main.py`。

```
# ./main.py
# ---------------------------
import sync_api
from timeit import default_timer as timer

BASE_URL = "https://avatars.dicebear.com/api/{avatar_style}/{seed}"

ALL_STYLES = [

 "adventurer",

 "adventurer-neutral",

 "avataaars",

 "big-ears",

 "big-ears-neutral",

 "big-smile",

 "botts",

 "croodles",

 "micah"

 ]

def generate_profile_pics(n):
 for _ in range(n):
  sync_api.create_new_avatar(avatar_style="adventurer", base_url=BASE_URL)

start = timer()
generate_profile_pics(20)
print(f"Time Elapsed: {timer()-start} seconds")
```

就是这样！我们已经设置为创建 20 张个人资料图片。

> 注意:DiceBearAvatar API 是一个很好的免费资源，不应该被滥用。**负责任地使用！**

您可以继续运行:

```
python main.py
```

您应该在指定的目录中获得 20 个 PNG 图像。

好了，现在说点好的。我们有没有办法加快那段代码的速度？我们是否可以同时运行该任务的多个实例？进入异步库`asyncio` 和`aiohttp`，这是我们用 Python 进行异步 web 请求的工具集。

`aiohttp`最适合用客户端会话来处理多个请求，所以我们将使用它(`requests`也支持客户端会话，但它不是一个流行的范例)。

```
# ./async_api.py
# ----------------------------
import aiohttp
import asyncio
import utils

async  def aio_create_new_avatar(client_session:aiohttp.ClientSession, avatar_style:str, base_url:str):
 seed = utils.generate_random_seed()
 request_url = utils.generate_api_url(seed=seed, avatar_style=avatar_style, base_url=base_url)

 async with client_session.get(request_url) as resp:
  data = await resp.read()
 utils.write_to_file(data=data, file_name=seed, file_path=r"C:/Users/user/desktop/pp")
```

好的。所以和`sync_api.py`没太大区别。我们还有几个导入，然后我们在客户端会话中使用一个上下文管理器来发出 web 请求。如果`async/await`语法对你来说是新的，你可以看看[这篇文章](https://medium.com/@katchyemma/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1)，它介绍了 Python 中异步的整个概念。

接下来我们将修改`main.py`来使用我们的新代码。我喜欢精彩的比赛，所以我们将跟踪异步和同步代码的执行时间。
下面是更新后的`main.py`:

```
# ./main.py
# -----------------------------------
# new imports
import aiohttp
import asyncio
import async_api

import sync_api

from timeit import default_timer as timer

BASE_URL = "https://avatars.dicebear.com/api/{avatar_style}/{seed}"

ALL_STYLES = [

 "adventurer",

 "adventurer-neutral",

 "avataaars",

 "big-ears",

 "big-ears-neutral",

 "big-smile",

 "botts",

 "croodles",

 "micah"

 ]

def generate_profile_pics(n):

 for _ in range(n):
  sync_api.create_new_avatar(avatar_style="adventurer", base_url=BASE_URL)

async def async_generate_profile_pics(n):

 Client = aiohttp.ClientSession()
 Tasks = []

 for _ in range(n):

  # Create a new profile pic
  Tasks.append(
    async_api.aio_create_new_avatar(
       client_session=Client, 
       avatar_style="big-smile",
       base_url=BASE_URL
    )
  )

  try:
   await asyncio.gather(*Tasks)  

  except:
   pass

  finally:
   await Client.close()

start = timer()
generate_profile_pics(20)
print(f"Time Elapsed: {timer()-start} seconds")

# For Windows Users 
asyncio.set_event_loop_policy(asyncio.WindowsSelectorEventLoopPolicy())

start = timer()
asyncio.run(async_generate_profile_pics(20))
print(f"Time Elapsed: {timer()-start} seconds")
```

搞定了。如果我们运行它，我们应该在我们指定的目录中得到 40 多张个人资料图片。平均来说，同步代码在我的电脑上执行需要 16 秒，而异步代码只需要 1 秒！如果你问我，我会说是时间上的重大改进。所以在这种情况下，代码复杂性和代码优化之间的权衡得到了回报。

这个项目对我特别有用。我用它来生成个人使用的显示照片，以及我使用的 web 应用程序的占位符显示照片。如果你想在商业上使用这些图片，请检查每种风格的[许可](https://avatars.dicebear.com/licenses)。

这个项目的完整代码可以在这里找到。

[](https://github.com/TobeTek/personal_pros/tree/master/start_async_py) [## master tobe tek/personal _ pros/start _ async _ py 上的 personal _ pros/personal _ pros

### 个人项目。在 GitHub 上创建一个帐户，为 TobeTek/personal_pros 开发做贡献。

github.com](https://github.com/TobeTek/personal_pros/tree/master/start_async_py)  [## 概述| DiceBear 头像

### 你想创造男性、女性还是抽象的化身？你有几个可爱的设计头像之间的选择

avatars.dicebear.com](https://avatars.dicebear.com/styles) [](https://medium.com/@katchyemma/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1) [## Asyncio 101 |编写异步 Python 程序

### 异步代码是目前的一个大话题。这是一个相当宽泛的术语，有时可能意味着并发性…

medium.com](https://medium.com/@katchyemma/asyncio-101-writing-asynchronous-python-programs-61b732ca77b1)  [## aiohttp

### 渴望开始吗？这个页面很好地介绍了如何开始使用 aiohttp 客户端 API。首先，制作…

docs.aiohttp.org](https://docs.aiohttp.org/en/stable/client_quickstart.html) 

感谢阅读！离别迷因🙃

![](img/9ff621140eaa050a7d0e0350591e40b1.png)