# 使用 PyScript 让 Python 在您的浏览器中运行

> 原文：<https://blog.devgenius.io/getting-python-to-run-in-your-browser-using-pyscript-1477b90d345d?source=collection_archive---------4----------------------->

让我们建立一个倒计时网页。

![](img/b0571d4bfa54bbdca79d154bda6e28fe.png)

# 介绍

又有新浪潮了！作为数据科学、机器学习和后端语言，Python 一直是第二大和最受欢迎的语言，JavaScript 作为前端和后端语言都占据了第一位。现在 python 想靠近前端，或者更好地将后端远离服务器。所以它不会是服务器代码，它只是脚本，它将在浏览器内部运行。

本质上，在浏览器中使用 python，您的后端代码只需下载一次，就可以像 JavaScript 一样继续在浏览器中运行。所以服务器只提供它一次，就像其他文件、css、javascript 图像一样。

这是由一种叫做网络组装的新技术实现的。

网络上的 WhatsApp 和我们手机上的很不一样。PyScript 可以解决这个问题。网飞可以利用压缩技术来确保数据成本不会阻止潜在客户，甚至 YouTube 和其他流媒体服务也可以利用这一点。那些允许下载的网站也可以提供不同的文件类型和大小供用户选择。我真的认为 PyScript 在多媒体和 web 方面有一席之地。

# 装置

对于安装，PyScript 并不真的需要安装，你只需要把它链接到你的网页上。要将所有的 python 代码放在 html 中，您不需要服务器，但是要在单独的 python 文件中运行一些代码，您需要一个服务器。所以我们将安装一个服务器，一个轻量级的服务器。在我们可以在服务器上运行 python 之前，我们需要一个 ASGI web 框架。我们不会真的在服务器上运行 python，但是服务器将为浏览器提供文件，就像它为浏览器提供图像、css、javascript 和 html 一样。python 将在浏览器中运行。所以我们将使用这些库或技术。

*   FastAPI——Python 中发展最快的框架之一
*   uvicon——FastAPI 推荐的服务器。
*   w3 . css——互联网上最小的 CSS 框架。最适合我们的使用案例

## FastAPI

要安装 FastAPI，请打开命令提示符或终端并执行

```
$ pip install fastapi
```

## 紫玉米

要安装 Uvicorn，打开命令提示符或终端并执行

```
$ pip install "uvicorn[standard]"
```

## W3.css

我们将从这个[链接](https://www.w3schools.com/w3css/4/w3.css)将 w3.css 链接到 webapp，您也可以下载到您的本地计算机上。

## PyScript

PyScript 提供了两个文件供您链接。一个是 css 文件，当 PyScript 准备好它的组件时，它在页面上绘制一个加载器。另一个是作为 Javascript 文件的入口点。

PyScript 自己的网站 PyScript.net 上提供了获取这些文件的链接。从那里，您可以选择安装选项并获得这两个文件。

CSS:<link rel="”stylesheet”" href="”https://pyscript.net/alpha/pyscript.css&quot;">js:<script defer src = " https://py script . net/alpha/py script . js "></script>

## 背景图像

使用的背景图片是 Hasan Albari 的照片:[https://www . pexels . com/Photo/silver-and-black-laptop-computer-1229861](https://www.pexels.com/photo/silver-and-black-laptop-computer-1229861)。

下载它或者你可以在你的电脑上使用任何合适的图片。把它复制到文件夹 ***图片*** 或者你可以直接使用链接。

**所有安装和设置都已完成**。

## 设置目录结构

为 web 应用程序创建一个文件夹，命名为 say ***launchpage。*** 在里面再创建一个文件夹，命名为 ***static*** ，然后创建另外 3 个文件夹，命名为 ***css*** 、 ***images*** 和 ***python*** ，直接放在 ***static*** 文件夹里面。 ***python*** 文件夹会包含你自己的 python 代码。因此，您的文件夹结构应该如下所示:

```
launchpage
  /- static
    -- css
    -- images
    -- python
```

# 前端

现在，在我们编写 python 代码之前，我们应该先用 HTML 和 CSS 编写一个前端。

在项目目录下创建一个文件，命名为 index.html。

```
launchpage
  -  index.html
  /- static
    -- css
    -- images
    -- python
```

打开***index.html***并用此代码填充

> index.html

```
<!DOCTYPE html>
<html lang="en"><head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Lauchpage</title>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css" /> <style>
        html, body {
            height: 100%;
            width: 100%;
        } body {
            background-image: url('./static/images/pexels.jpg');
            background-size: cover;
            color: white;
        } main {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        } .time-cont {
            display: flex;
            flex-wrap: nowrap;
            align-items: baseline;
            justify-content: center;
        } </style>
</head><body><main class="w3-display-container">
    <div class="w3-display-middle">
        <h1 class="w3-jumbo">Coming Soon!</h1>
        <div class="time-cont">
            <p id="time_p" class="w3-large w3-center">10Days 12Hrs 15mns 45s</p>
            <span>&nbsp;more</span>
        </div>
    </div>
</main></body>
</html>
```

*上面的代码是一个普通的 html 文档，有几行 css 在* ***样式*** *标签中声明。尸体的背景是哈桑·阿尔巴里的照片。图像的 url 是一个相对路径，现在我们在前面使用一个点。这是在我们使用服务器提供服务之前。除了图像之外，我们只有两个文本，即将到来的和我们的模拟时间字符串。注意，声明我们的时间字符串的****<p>****标签有一个 id，就是****time _ p****。ID 是脚本语言通常与 HTML 元素通信的方式。*

右键单击该文件，选择打开方式，然后选择您最喜欢的浏览器，您应该会看到类似这样的内容，呈现在您的浏览器中。

![](img/b3fb0a5095d89260028cadb522a885ed.png)

现在我们的前端做好了。很短，是吧？

# 后端

对于 PyScript，后端在短时间内是后端，因为它实际上将在浏览器内部运行。我们不能说的是，‘服务器端代码’，后端是允许的。所以让我们继续我们的后端。

在 launchpage 文件夹中创建一个文件，并将其命名为 main.py，因此您的目录结构应该如下所示

```
launchpage
  -  index.html
  -  main.py
  /- static
    -- css
    -- images
    -- python
```

打开 main.py 并用以下代码填充它

> main.py

```
from fastapi import FastAPI
from fastapi.responses import HTMLResponse
from fastapi.staticfiles import StaticFiles app = FastAPI()app.mount('/static', StaticFiles(directory='static'), name='static') @app.get("/", response_class=HTMLResponse)
def read_root():
    with open('index.html', 'r') as ind:
        data = ind.read()
    return HTMLResponse(data)
```

在上面的代码中，我们有 python 导入语句。 ***FastAPI*** *为主接口，****html response****为针对 JSON 的响应或其他形式的响应，这意味着响应不会被格式化为默认的 JSON 格式。****app . mount****代码确保当 url 以'/static '(' https://domain . com/Static ')开头时，它应该作为一个静态文件目录来处理，其中目录是' Static '，这意味着静态文件夹与我们的 main.py 文件在同一个文件夹中。最复杂的代码，也许是****@ app . get****decorator，这意味着当用户用 get 请求查询服务器的'/' ('https://domain.com/')路径时，它应该运行****read _ root****函数。在****read _ root****函数中，我们读取 index.html 并将内容以 HTML* 的形式返回给用户

然后在 index.html 中使用。/static 并将其更改为/static。

```
...
    <title>Lauchpage</title>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css" /> <style>
        html, body {
            height: 100%;
            width: 100%;
        } body {
            background-image: url('/static/images/pexels.jpg');
            background-size: cover;
            color: white;
        }
...
```

要运行这个，正如我们已经达到的程度，您需要启动服务器。打开命令提示符或终端，并执行以下操作:

```
$ uvicorn main:app --reload
```

****main:app****正在调用 main.py 文件并运行 main . py****中我们声明为****app = FastAPI()****的****app 变量**

*运行这个，你应该看到和以前一样的东西。唯一的变化是，现在我们不使用点相对路径，以服务于我们的形象。接下来，我们将开始编写 python 代码，这些代码将来自服务器。*

## *Hello 浏览器*

*让我们编写第一个在浏览器中运行的 python 代码。*

```
*<!DOCTYPE html>
<html lang="en"><head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Lauchpage</title>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css" />
    <link rel="stylesheet" href="https://pyscript.net/alpha/pyscript.css" /> <style> ... </style>
    <script defer src="https://pyscript.net/alpha/pyscript.js"></script>
</head><body><main class="w3-display-container">
    <div class="w3-display-middle">
        <h1 class="w3-jumbo">Coming Soon!</h1>
        <div class="time-cont">
            <p id="time_p" class="w3-large w3-center">10Days 12Hrs 15mns 45s</p>
            <span>&nbsp;more</span>
        </div>
        <py-script>
            print('Hello Browser')
        </py-script>
    </div>
</main></body>
</html>*
```

**上面的代码链接了 PyScript 的 css 和 JavaScript。然后我们有了一个****<py-script>****标签，从里面我们可以运行 python 代码，目前我们只打印'****Hello Browser****'**

*因为 Uvicorn 是用 reload 参数调用的，所以您应该能够刷新页面并看到如下内容。如果你已经关闭了它，在终端中再次运行 Uvicorn 代码，然后刷新你的浏览器*

*![](img/1f8cacaae8c18783532f42b6c7eba6a8.png)*

*对于现在的 **Hello Browser** ，打印在我们定义 **< py-script >** 标签的地方，在 **span** 的父 **div** 之后。但是将来我们会对现有的 html 元素进行修改。*

*创建一个新文件 countdown.py，这样你的目录结构应该是这样的:*

```
*launchpage
  -  index.html
  -  main.py
  /- static
    -- css
    -- images
    /- python
      -  countdown.py*
```

*打开 countdown.py 文件，并用以下代码填充它。*

> *倒计时. py*

```
*from datetime import datetime DATE_MAP = ('s', 'mns', 'Hrs', 'Days', 'Months', 'Years') def get_time() -> str:
    dt = datetime(2022, 10, 1, 0, 0, 0)
    delta = dt - datetime.now()
    cleaned_delta = str(delta).replace(' days, ', ':')
    deltas = cleaned_delta.split('.')[0].split(':')
    x = len(deltas)
    stmts = [] for i in range(x):
        ind = x - 1 - i
        stmt = deltas[i] + DATE_MAP[ind]
        stmts.append(stmt)return ' '.join(stmts)*
```

*在上面的代码中，我们有一个元组，它以简短的形式存储秒、分和小时，以完整的形式存储日期的名称，没有任何偏见，只是常规。然后有一个函数从 2022 年 10 月 1 日减去我们当前的日期，看看我们还剩多少天，多少小时等等。它将结果作为字符串返回*

*现在将 **countdown.py** 文件导入到 index.html 中*

> *index.html*

```
 *... </style>
    <script defer src="https://pyscript.net/alpha/pyscript.js"></script>
    <py-env>
        - paths:
            - /static/python/countdown.py
    </py-env></head><body>...*
```

**在上面的代码中，我们添加了一个****<py-env>****标签在****<head>****里面，然后导入我们的****count down . py****，考虑到我们为 FastAPI 配置的静态文件夹设置，放在 main.py 文件中。**

*现在删除 hello browser 代码，并像在 python 中一样导入这个文件*

> *main.py*

```
 *... <span>&nbsp;more</span>
        </div>
        <py-script>
            from countdown import get_time
        </py-script>
    </div>
</main>...*
```

*现在使用 ***asyncio*** ，让我们调用在 ***countdown.py*** 中声明的 ***get_time*** 函数来给我们每秒剩余的时间字符串。*

> *main.py*

```
*...<py-script>
    from countdown import get_time
    import asyncio async def tell_time():
        while True:
            time_p = Element('time_p').write(get_time())
            await asyncio.sleep(1) pyscript.run_until_complete(tell_time())</py-script>...*
```

**在上面的代码中，我们使用****async****关键字定义了一个函数。这个函数调用我们的****get _ time****函数，并把它的返回字符串写成****time _ p****的内部文本，也就是那个带有“****10 days 12 hrs 15 MnS 45s**标签的******<p>****它在一个* ***而*** *循环中这样做，永远运行。它只休眠一秒钟，所以这段代码运行时，时间字符串每秒都会改变。然后我们调用****pyscript . run _ until _ complete****来运行****async****函数****tell _ time****。在这种情况下，它将永远不会完成。这证明 python 确实在浏览器中运行，因为你可以在页面加载完成后关闭服务器，你仍然可以永远得到时间更新。****

*现在完整的 index.html 文件应该看起来像这样*

> *index.html*

```
*<!DOCTYPE html>
<html lang="en"><head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Launchpage</title>
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css" />
<link rel="stylesheet" href="https://pyscript.net/alpha/pyscript.css" /><style>
html, body {
height: 100%;
width: 100%;
}body {
background-image: url('/static/images/pexels.jpg');
background-size: cover;
color: white;
}main {
width: 100%;
height: 100%;
display: flex;
align-items: center;
justify-content: center;
}.time-cont {
display: flex;
flex-wrap: nowrap;
align-items: baseline;
justify-content: center;
}</style><script defer src="https://pyscript.net/alpha/pyscript.js"></script><py-env>
- paths:
- /static/python/countdown.py
</py-env></head>
<body><main class="w3-display-container">
<div class="w3-display-middle">
<h1 class="w3-jumbo">Coming Soon!</h1>
<div class="time-cont">
<p id="time_p" class="w3-large w3-center">10Days 12Hrs 15mns 45s</p><span>&nbsp;more</span>
</div><py-script>
from countdown import get_time
import asyncio async def tell_time():
while True:
time_p = Element('time_p').write(get_time())
await asyncio.sleep(1) pyscript.run_until_complete(tell_time())</py-script>
</div></main></body>
</html>*
```

*现在，当您刷新页面或重新运行 uvicorn 时，您应该会看到类似这样的内容:*

*![](img/b0571d4bfa54bbdca79d154bda6e28fe.png)*

*全部完成！*

# *最后的想法*

*目前 PyScript 正在大量开发中，它也有点不稳定。将它安装到您的服务器上以增加加载时间，还没有完全完成。但是这将很快完成，加载时间会更快。文档也很害怕，但是 python 社区已经准备好了。*

*当 PyScript 处于实验模式时，它的潜在改编也应该处于实验模式，在你的家里和办公室，作为爱好项目和资金充足的研究。一起前途一片光明。*

*再见。*