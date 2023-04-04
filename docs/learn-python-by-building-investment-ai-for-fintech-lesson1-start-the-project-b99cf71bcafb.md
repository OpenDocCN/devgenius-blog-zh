# 通过为金融科技构建投资人工智能来学习 Python 第 1 课:启动项目

> 原文：<https://blog.devgenius.io/learn-python-by-building-investment-ai-for-fintech-lesson1-start-the-project-b99cf71bcafb?source=collection_archive---------10----------------------->

![](img/7d7ffb98f485f120f76b41f2b6c59b5c.png)

[Python 课程](https://www.blog.duomly.com/python-course-with-building-a-fintech-investment-ai-lesson-1-start-the-project)

今天我们将开始 Python 课程，开始构建 AI。

在最后几集里，我们开始了为我们的金融科技应用构建前端和后端的第一步。

下面是链接:

**Golang 课程:**

[Golang 课程用构建一个 fintech 银行应用程序—第一课:启动项目](https://www.blog.duomly.com/golang-course-with-building-a-fintech-banking-app-lesson-1-start-the-project/)

[Golang 课程用构建一个 fintech 银行应用程序—第二课:登录和 REST API](https://www.blog.duomly.com/golang-course-with-building-a-fintech-banking-app-lesson-2-login-and-rest-api/)

**Angular 课程:**
[Angular 课程用 Tailwind CSS 构建一个银行应用程序—第一课:启动 一个使用 Tailwind CSS 的银行应用程序——第二课:登录表单](https://www.blog.duomly.com/angular-course-building-a-banking-application-with-tailwind-css-lesson-1-start-the-project/)

现在是做一些特别的事情的时候了！ 我们开始构建一个人工智能，它将预测股票价格，并告诉我们现在投资以获得最大利润的最佳选择是什么。我迫不及待地想让你看看这一切。

在今天的这一集里，我将向大家展示“Python 与 AI”课程的第一部分，我们将设置项目的第一部分。

第一部分是项目安装、配置和数据库迁移。

我们开始吧！

如果你喜欢视频，这里是 youtube 版本:

Python 课程

# 安装 flask 和依赖项

您需要做的第一步是安装依赖项。

我们会需要几个，但对我们来说最重要的是一个烧瓶。
打开项目目录中的终端并键入:

```
pip install flask
```

接下来，安装 SQLAlchemy:

```
pip install flask_sqlalchemy
```

您需要安装的第三个插件是 Migrate:

```
pip install flask_migrate
```

最后一个是 PostgreSQL 驱动程序:

```
pip install psycopg2
```

# 初始化 git repo 并创建。gitignore

好了，当我们最重要的依赖项准备就绪时，我们应该初始化 git 存储库并指定 git 应该忽略的文件。

打开终端，键入:

```
git init
```

接下来，创建。gitignore 文件中的项目的 dir，并添加几个路径，我们应该忽略:

```
**/__pycache__/*
migrations/*
.vscode
```

# 创建 app.py 并导入 dep

这是我们真正开始用 Python 编码的第一步。

我们需要创建一个名为“app.py”的文件，并导入我们已经安装的依赖项。

```
import psycopg2
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
```

# 导出烧瓶变量

当我们创建完 app.py 文件后，我们应该将路径导出到 flask 的应用程序。

之后，flask 就会知道什么文件应该作为应用程序运行。

打开终端，键入:

```
export FLASK_APP=app.py
```

# 定义应用程序并配置数据库连接

下一步，我们应该定义 flask 应用程序并创建 app 变量。第二，我们应该做的是创建代码来配置与数据库的连接。

在名为“app.py”的文件中，我们应该在 imports 下面添加我们的 app 声明和连接字符串。

```
app = Flask(__name__)
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SQLALCHEMY_DATABASE_URI'] = "postgresql://user:password@localhost:5432/dbname"
```

# 添加应用程序运行

在这一步中，我们将重点关注 app.run 命令。
这将让我们从命令行启动应用程序。

第一节课我们不会用到，但以后的课要加进去。

在 app.py 文件的底部，如果 **name** 等于‖**main**，我们应该添加 app.run(debug=True)。

```
if __name__ == '__main__':
    app.run(debug=True)
```

# 创建模型目录

太好了！现在，我们可以继续前进，专注于数据库的模型。

在第一课中，我们将创建名为“prices”的唯一模型，这样我们就可以使用同名的 SQL 表。

首先，我们需要创建一个名为“models”的目录，将其添加到您的项目中。

# 在模型目录中创建 prices.py 并从应用程序导入数据库

现在，当我们创建了一个必要的目录，我们可以进入文件。

首先，创建名为 prices.py 的文件(将其添加到名为“models”的目录中)。接下来，你应该进入 prices.py 文件，从应用程序中导入名为“db”的变量。

```
from app import db
```

# 在 prices.py 中创建模型

这一步是我们今天编码的最大部分，也是最重要的。我们需要创建用于迁移的整个模型。

我们应该先创建一个名为“PriceMode”的类，并传递“db。模型”，作为参数。

下一步是声明一个名为“ **tablename** ”的变量，其值为“价格”。在那一行下面，我们需要创建将在我们的表中使用的列。

之后，我们应该创建一个名为“ **init** 的函数，并将我们的列分配给 self。

让我们看看下面的例子中我是如何做到的。

```
class PriceModel(db.Model):
    __tablename__ = 'prices' id = db.Column(db.Integer, primary_key=True)
    company = db.Column(db.String())
    date = db.Column(db.String())
    openPrice = db.Column(db.Integer())
    highPrice = db.Column(db.Integer())
    lowPrice = db.Column(db.Integer())
    closePrice = db.Column(db.Integer())
    volume = db.Column(db.Integer()) def __init__(self, company, date, openPrice, highPrice, lowPrice, closePrice, volume):
        self.company = company
        self.date = date
        self.openPrice = openPrice
        self.highPrice = highPrice
        self.lowPrice = lowPrice
        self.closePrice = closePrice
        self.volume = volume def __repr__(self):
        return f"<Price for {self.company} in day {self.date} is {self.closePrice}>"
```

# 在 app.py 中导入模型并添加迁移

现在是编码的最后一步，我们运行代码！

我们应该回到名为“app.py”的文件，导入我们的价格模型。

导入模型后，我们应该声明一个名为“migrate”的变量，并调用我们之前介绍的 Migrate 函数。

对于名为“Migrate”的函数，我们需要将 app 和 db 变量作为 params 传递。

这段代码应该在名为“db”的变量之后，最后一个 if 语句之前。

我们来看看下面的例子。

```
from models.prices import PriceModel
migrate = Migrate(app, db)
```

# 初始化数据库

我们完成了代码！

现在我们应该运行它，检查是否一切正常。

首先你需要做的是在终端中使用这个方法初始化 DB:

```
flask DB init
```

# 迁移和升级

如果一切顺利，您应该有一些 **pycache** 目录和名为“migrations”的新目录。

如果是，您可以进入数据库迁移。

首先你需要做的就是迁移，打开终端，并键入:

```
flask db migrate
```

迁移后，您需要使用结果升级，打开终端并键入:

```
flask db upgrade
```

# 结论

恭喜你！

你进军建筑投资 AI 的第一步已经完成。现在，您的项目已经为下一个功能做好了准备，我们将在接下来的课程中重点介绍这些功能。

python 课程的代码库在这里:

[https://github . com/Duomly/Python-ai-investment-fin tech/tree/Python-AI-course-Lesson-1](https://github.com/Duomly/python-ai-investment-fintech/tree/Python-AI-course-Lesson-1)

在下一课中，我们将重点为我们的 AI 构建 API 和数据。保持更新，我迫不及待地想向你展示我们一起构建的强大工具的下一步！

![](img/da0dbcf44b16aceb7cca413a6d9a5ef7.png)

[编程课程在线](https://www.duomly.com)

感谢您的阅读，
来自 Duomly 的 Radek