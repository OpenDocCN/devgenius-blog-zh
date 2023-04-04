# 使用 WTForms 和 Oracle REST 数据库服务(ORDS)API

> 原文：<https://blog.devgenius.io/working-with-wtforms-and-oracle-rest-database-services-ords-apis-7540aa57bd79?source=collection_archive---------16----------------------->

![](img/5503fcf85911c46ab81e855cbfdf7102.png)

当前的 Flask 应用程序，通过使用 HTML 和 CSS 的 Bootstrap 变得可展示

# 欢迎回来

标题本身就很说明问题，如果您一直在关注，您会知道我正在开发一个演示应用程序，它使用 [Flask](https://flask.palletsprojects.com/en/2.1.x/quickstart/) (一个 web 应用程序微框架)通过 Oracle REST 数据服务(ORDS)API 连接到我的 Oracle 自治数据库(ADB)。

Flask 的优势之一是它允许您扩展功能，并允许很好的定制。*但是……*

> *默认情况下，Flask 不包括数据库抽象层、表单验证或其他任何已经存在的不同库可以处理的东西。相反，Flask 支持扩展，将这种功能添加到您的应用程序中，就好像它是在 Flask 本身中实现的一样。*
> 
> [*烧瓶文件*](https://flask.palletsprojects.com/en/2.1.x/foreword/#what-does-micro-mean)

# 为什么选择亚行和 ORDS？

出于*明显的原因*，我主要使用这种*特定的*配置(与 ORDS 和亚行)。但是其次，我还没有遇到任何引用 Oracle ADB 或 ORDS 的东西(当然是在 Flask、WTForms 和 python 的上下文中)。

我看过的大部分在线教程和读过的书都是参考数据库的 SQLlite 或者(有一种情况)MongoDB。SQLite 看起来确实是一个很好的切入点，尤其是当您在收集概念证明时。但是随着规模的扩大，我怀疑你会用 SQLite 达到递减的性能回报。事实上，Flask 文档中提到了这一点:

> *Python 在 sqlite3 模块中内置了对 SQLite 的支持。SQLite 很方便，因为它不需要设置单独的数据库服务器，并且内置于 Python 中。* **然而，如果并发请求试图同时写入数据库，它们会因为每次写入按顺序发生而变慢。小应用不会注意到这一点。一旦你变大了，你可能想换一个不同的数据库。**
> 
> [*烧瓶文件*](https://flask.palletsprojects.com/en/2.1.x/tutorial/database/)

最后一句话意义重大。我知道 ORDS 在建立时会考虑到高可用性，那么为什么不从一开始就利用这一点呢？此外，使用我的 ADB，创建数据库模式和表的工作非常简单(甚至很有趣)。

关于，MongoDB，很明显，我有偏见但是我也没有条理。我仍然是一个“技术婴儿”因此，下载、安装和配置 MongoDB Community Edition*plus*MongoDB Compass GUI 对我来说是一个不必要的步骤(我实际上是为了之前的教程才这么做的)。

不管怎样，我已经有了我的 OCI 租赁和我的自治数据库，我继续对我们的生态系统越来越适应。

![](img/f6220fff704c57720e9a225f75ab1f21.png)

另外:你应该[注册](https://www.oracle.com/cloud/free/)一个*永远免费*的 OCI 账户。

继续…我还想使用数据库动作，以及 ORDS 提供的 REST API 特性。更不用说，从我在 Flask 教程中看到的一切来看，在做任何真正有意义的事情之前，你必须首先创建数据库模型。我不完全确定该如何着手(请在 2023 年回来查看)。

ORDS 很简单——只需给我 API 端点的 URI，我就可以从那里开始了。如果我需要一些额外的东西，我们可以相应地修改 API。

# API 获取请求响应(通过 ORDS)

说到这里，下面是我的 ORDS API GET 请求中 JSON 的样子:

![](img/14869f57e16d23210cbf251eca223727.png)

我不会花太多时间来解释这个图像(你可以详细阅读，这里[这里](https://followthecoffee.com/get-requests-with-python-ords-and-the-oracle-autonomous-database/))，但我最感兴趣的是这个城市:(在 items 下找到)。有 18 个城市，我需要全部的*来装瓶子。更具体地说，我需要它们用于 WTForms。*

# 关于 WTForms

我不会给出关于 WTForms 的速成课程，但底线是这是扩展 Flask 功能的另一种方式。

> *WTForms 是一个用于 Python web 开发的灵活的表单验证和呈现库。它可以与您选择的任何 web 框架和模板引擎一起工作。它支持数据验证、CSRF 保护、国际化(I18N)等等。*
> 
> [*WTForms 文档*](https://wtforms.readthedocs.io/en/3.0.x/#)

在我的场景中，WTForms 允许我在 Flask 应用程序中轻松地创建和处理(如路由)表单。更具体地说，有一个我需要使用的`SelectFields()`表单(这只是我最初设计这个应用程序的方式，你可以在这里[查看](https://followthecoffee.com/project-overview-python-flask-ords-autonomous-database/))。

# 选择

这些表格有*字段*。以下是我最感兴趣的表格细节:

![](img/6ad97c557fdf71340d4dd6e0494167f7.png)

您会注意到我可以包含各种参数，但我只关心`choices[]`。您可以在选择列表中硬(静态)编码，*或*(正如我本周刚刚成功完成的那样)，将来自 API GET 请求的列表传递给`choices[]`参数。

如果您看过文档，那么您会看到`choices[]`可以通过三种方式中的一种来“通过”:

1.  一个`(value,label)`对列表(也可以是一个只有 ***个值*** 的列表，在这种情况下，该值用作标签)
2.  定义选项分组的`{label: list}`对字典，或
3.  一个不带参数的函数，返回一个列表或者一个字典

我的方法使用了第一个选项。实际上，在我的例子中，*仅值*方法。

# 崩溃

首先，我将显示下拉列表；在我的应用程序中会是这样的(仅供参考，这个*实际上*有效):

我知道视频很快，但是你应该能够看到下拉列表(大约在:15-:17 标记处)以及它是如何渲染的。

这个下拉列表是通过对我的启用了 ORDS 的“城市”表的初始 GET 请求实现的。完整的代码(包括 Flask 位)如下所示:

![](img/50268fcfe345d1a9d230a8b2080d51bf.png)

**注意:** *我假设您可能对 flask 和 WTForms 比较熟悉。另外，您的重点是这个问题的 REST API-to-choices 部分。*

# 系统地

好的，一个*快速*概述:

*   第 27–29 行:这对于 Flask 来说很标准，不需要解释
*   第 31–33 行:是我构建表单的地方。您将看到“城市”是字段(可供选择)。下拉列表将有一个标题，上面写着，“让我们从选择一个城市开始”(你可以在视频中看到)。而选择将是一个`list[]`。第 33 行的 Submit 就是创建 submit 按钮的方式
*   第 37–39 行:对于任何使用 python 中的[请求库](https://docs.python-requests.org/en/latest/)的 GET 请求来说也是非常标准的

**注意:** *但是在这里，您会看到我的 URL 实际上是由 ORDS 提供的 URI(记住，我启用了 REST 的我的城市表——这是我们看到的“端点”)*

*   第 40 行:我列清单(这一步是空的，好难过)
*   第 41–43 行:这是*抵抗之战。*在这里，我们通过 JSON 响应中的项目执行一个循环:因为这是一个嵌套数组，所以您必须指定您想要的数组以及该数组中您想要取出的确切的*(在本例中是“城市”)。第 43 行是 jeffsmith 的建议，按照字母顺序显示列表(我必须承认，这样做更好)。*
*   *第 45–48 行:这些也是 Flask 的标准代码。您需要将 city(第 32 行)设置为 city_list，这样 WTForms 就知道`SelectField(choices=[])`来自哪里*

# *代码*

*简单的解释结束后，下面是代码。请自便:*

```
*#For the GET request portion, you'll at a minimum need requests and json. If you are working in Flask already, then you know the rest. import requests
import json 
from wtforms import SelectField, SubmitField
from flask import Flask, json, jsonify, render_template, request
from flask_wtf import FlaskForm#Creating the Flask instance.app = Flask(__name__)
app.config['SECRET_KEY'] = 'password1234'#Creating the Form.class CitiesForm(FlaskForm):
  city = SelectField("Let's begin by choosing a city", choices=[])
  submit = SubmitField('Submit')#Routes, used for displaying the html on your web app.@app.route('/', methods = ['GET', 'POST'])
def index():#This is the section where you perform the GET request to retrieve the JSON from your ORDS endpoint. URL = '[your ORDS endpoint]'
    response = requests.get(URL)
    r = json.loads(response.text)#Creating the list of cities; which will be populated shortly.city_list = []#Looping through the nested array (from the inital JSON / API response).     for nestedArr in r['items']:
      city_list.append(nestedArr['city'])
      city_list.sort()#Finishing touches so WTForms, and Flask know where to look. And also setting up how/where the html on your web application renders. city=city_list
    form = CitiesForm()
    form.city.choices = city_list 
    return render_template('index.html', form=form)*
```

*显然，你会想改变一些名字。但是 ORDS 的作品应该还是很相似的。*

# *接下来*

*我的下一步是为应用程序创建第二个*和第三个*依赖下拉菜单。我的意思是，基于用户的城市选择，该城市将被用作后续 GET 请求(对我的 ADB)的查询参数。ORDS 将用一个包含目标城市餐馆信息的列表来响应(其他信息都被排除)。*

*如果你已经做到这一步，并且有任何问题，请给我发一个[便条](http://mailto:chris.hoina@oracle.com)。*

*如果你认为这是值得分享的，并且它可能会帮助其他人，请把它传下去！*

# *在外面给我兑现*

*一如既往，您可以在以下网址找到我:*

*   *[推特](https://twitter.com/chrishoina%29)*
*   *[YouTube](https://www.youtube.com/user/chrishoina/)*
*   *[GitHub](https://github.com/chrishoina)*
*   *[不和](https://discord.gg/seYeAPgq9E)*
*   *[领英](https://www.linkedin.com/in/chrishoina/)*
*   *[堆栈溢出](https://stackoverflow.com/users/9552864/chris-hoina)*
*   *[中等](https://chrishoina.medium.com/)*

*# Oracle # RESTAPIs # OCI #自主数据库# Python # Flask # wt forms # ORDS # Oracle database tools # always free*