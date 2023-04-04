# 如何在 Python Flask 中创建 Todo 应用程序

> 原文：<https://blog.devgenius.io/how-to-create-a-todo-application-in-python-flask-2f1d8f24c352?source=collection_archive---------5----------------------->

在本文中，您将学习如何使用 Flask 框架用 Python 开发一个简单的 Todo 应用程序。Flask 是一个轻量级的 Python web 框架，提供了用 Python 构建在线应用程序的便捷工具和功能。因为您可以仅使用一个 Python 文件轻松设计 web 应用程序，所以它为开发人员提供了灵活性，并且对于初学者来说是一个易于使用的框架。Flask 也是可扩展的，因为它不需要复杂的样板代码或特定的目录结构来开始。

通过学习 Flask，你可以很容易地用 Python 构造 web 应用。Python 库可用于为您的 web 应用程序添加额外的功能，例如在数据库中存储数据和验证在线表单。

在本教程中:

*   您将创建一个简单的 web 应用程序，它使用浏览器来呈现 HTML 文本。
*   您将学习如何安装 Flask，创建和运行 Flask 应用程序，并在开发模式下测试它。
*   路由将用于在您的 web 应用程序中显示满足不同目标的大量网页。
*   查看功能也将用于允许用户通过动态路线与应用程序进行交互。
*   最后，调试器将用于解决问题。

# 第一步

## 使用 Python 3 中的虚拟环境创建一个项目。

首先，打开命令提示符并打开您的虚拟环境(如果有)。您还需要创建一个名为“todo-flask”的新文件夹，并使用“cd”命令导航到该文件夹。

```
$ mkdir todo-flask
$ cd todo-flask
$ python3 -m venv venv
```

有必要打开虚拟环境。

```
$ . venv/bin/activate
```

它在 Windows 机器上的激活方式不同

```
$ venv\Scripts\activate
```

# 第二步

## 安装烧瓶

假设您已经安装了 Python、pip 和虚拟环境。我们现在可以继续安装 Flask 本身了。

```
$ pip install Flask
```

## 安装 Flask_SQLAlchemy

SQLAlchemy 是一个流行的数据库抽象层和对象关系映射器，它需要一些设置，但是有一个 Flask 插件可以为您完成这项工作。我们将需要它，因为我们将在数据库中存储信息。只需运行下面的命令来安装 Flask SQLAlchemy。

```
$ pip install Flask-SQLAlchemy
```

# 第三步

## 创建 Hello World 应用程序

创建 app.py 文件，在其中写出您的代码

```
from flask import 
Flask app = Flask(__name__)  
@app.route('/') 
def hello_world():     
    return 'Hello, World!'      
    if __name__ == "__main__":
    app.run(debug=True)
```

对于每一个我们想要的 URL/route，我们都要创建一个函数，用 [@app](http://twitter.com/app) 来修饰。路由('路径/到/您的/url ')。因为这将是我们的第一页，我们将只使用正斜杠/。我们设置 debug=True 以避免每次修改代码时都要重新加载服务器。

## 执行应用程序

```
python3 app.py
```

我们现在可以在 [http://127.0.0.1:5000/](http://127.0.0.1:5000/) 检查我们的第一个运行程序。

# 第四步

## 将数据库添加到应用程序

我们必须首先导入 SQLAlchemy，然后配置一些数据库设置值，构造数据库，最后为我们的 todo 项开发一个模型类。我们的模型需要三个条目:一个 id、一个标题和一个指示任务是否已经完成的标志。在我们开始程序之前，我们还执行了`db.create_all().`

```
from flask import Flask 
from flask_sqlalchemy 
import SQLAlchemy  
app = Flask(__name__)  #
 /// = relative path, //// = absolute path app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.sqlite' app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False 
db = SQLAlchemy(app)

class Todo(db.Model):     
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100))  
    complete = db.Column(db.Boolean)@app.route('/') 
def hello_world():     
    return 'Hello, World!' if __name__ == "__main__":     
    db.create_all() 
    app.run(debug=True)
```

## 更改主页路线以显示所有待办事项

我们导入一个渲染模板并渲染一个完整的 html，而不仅仅是一个字符串。在下一阶段，我们将制作模板文件。用以下内容替换`hello_world()`:

```
from flask import Flask, render_template ... @app.route("/") 
def home(): 
    todo_list = Todo.query.all() 
    return render_template("base.html", todo_list=todo_list)
```

# 第五步

## 在 HTML 中创建样板用户界面

在项目的根文件夹中，创建一个名为`templates`的文件夹。Flask 正在寻找这个目录，因此它必须是这个名字！在这个模板文件夹中创建一个`base.html`文件，并将以下内容添加到其中:

```
<!DOCTYPE html>
<html lang="en">  
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
   <title>Todo App</title> 
</head>  
<body> 
     <h1>To Do App</h1>      {% for todo in todo_list %}      <p>{{todo.id }} | {{ todo.title }}</p>      {% if todo.complete == False %} 
     <span>Not Complete</span> 
     {% else %} 
     <span>Completed</span> 
     {% endif %} {% endfor %} </body> </html>
```

在这个语法中使用了 Jinja 模板引擎，允许我们访问我们的 Python list todo 列表，我们将它作为参数提供给模板。像 for 循环和 if 语句这样的控制流也是可能的。虽然语法类似于 Python，但并不相同。如果你想了解更多这方面的内容，请看[文档](https://jinja.palletsprojects.com/)。

# 第六步

## 要添加、删除和编辑新的待办事项，请添加路线。

对于我们的每一个操作，我们添加一个新的路由，它对我们的待办事项执行所需的操作，更新数据库，然后将我们重定向到主页。

```
from flask import Flask, render_template, request, redirect, url_for @app.route("/add", methods=["POST"]) 
def add(): 
    title = request.form.get("title") 
    new_todo = Todo(title=title, complete=False)
    db.session.add(new_todo) 
    db.session.commit()
    return redirect(url_for("home")) @app.route("/update/<int:todo_id>") 
def update(todo_id): 
    todo = Todo.query.filter_by(id=todo_id).first() 
    todo.complete = not todo.complete 
    db.session.commit() 
    return redirect(url_for("home")) @app.route("/delete/<int:todo_id>") 
def delete(todo_id): 
    todo = Todo.query.filter_by(id=todo_id).first()
    db.session.delete(todo) 
    db.session.commit() 
    return redirect(url_for("home"))
```

这也必须添加到我们的 base.html:

```
!DOCTYPE html> <html lang="en">
  <head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>Todo App</title> 
  </head>  
<body> 
     <h1>To Do App</h1> 
     <form action="/add" method="post"> 
        <div>  
          <label>Todo Title</label>   
          <input type="text" name="title" placeholder="Enter Todo...">  
           <br> 
           <button type="submit">Add</button> 
        </div> 
    </form>      <hr>      {% for todo in todo_list %}      <p>{{todo.id }} | {{ todo.title }}</p>      {% if todo.complete == False %} 
     <span>Not Complete</span> 
     {% else %} 
     <span>Completed</span> 
     {% endif %}      <a href="/update/{{ todo.id }}">Update</a> 
     <a href="/delete/{{ todo.id }}">Delete</a>      {% endfor %} 
</body>  
</html>
```

## 使用语义用户界面添加样式

语义 UI 用于样式。这是一个开发框架，有助于使用易于阅读的 HTML 创建令人惊叹的响应性布局。我们只需要在 HTML 中添加类名；我们不需要为 CSS 费心。例如，`<a class=”ui blue button” href=”…”>Update</a>`将把我们的普通标签转换成一个可爱的蓝色按钮。我们使用 CDN 将语义 UI 集成到我们的 HTML 中。欲了解更多信息，请查阅官方[文档](https://semantic-ui.com/introduction/getting-started.html)。

在标题部分包括以下内容:

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css"> 
<script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script>
```

要添加样式，我们只需要添加一些类名。这是最终 HTML 的样子:

```
<!DOCTYPE html> 
<html lang="en"> 

<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>Todo App</title> 
     <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.css"> 
    <script src="https://cdn.jsdelivr.net/npm/semantic-ui@2.4.2/dist/semantic.min.js"></script> 
</head> <body> 
    <div style="margin-top: 50px;" class="ui container"> 
        <h1 class="ui center aligned header">To Do App</h1>  
        <form class="ui form" action="/add" method="post"> 
            <div class="field"> 
                <label>Todo Title</label> 
                <input type="text" name="title" placeholder="Enter Todo..."><br> 
            </div> 
            <button class="ui blue button" type="submit">Add</button>         </form> 
         <hr> 
         {% for todo in todo_list %} 
        <div class="ui segment"> 
            <p class="ui big header">{{todo.id }} | {{ todo.title }}</p> 
             {% if todo.complete == False %} 
            <span class="ui gray label">Not Complete</span>
            {% else %} 
            <span class="ui green label">Completed</span>
            {% endif %} 

            <a class="ui blue button" href="/update/{{ todo.id }}">Update</a> 
            <a class="ui red button" href="/delete/{{ todo.id }}">Delete</a> </div> 
        {% endfor %}
     </div> 
</body> </html>
```

我希望您从本教程中学会了如何使用 Flask 创建一个小型的基于 REST 的 web 应用程序。如果您使用过 Django 等其他 Python 框架，您可能会注意到 Flask 更容易使用。

虽然您可以使用 Flask 来显示 HTML 页面和模板，但本课主要集中在程序的后端部分，没有任何 GUI。

虽然使用 Flask 来处理 HTML 模板是完全可以的，但大多数用户使用 Flask 来创建后端服务，然后使用任何流行的 JavaScript 库来创建应用程序的前端。你可以尝试看看什么最适合你。祝你的烧瓶冒险之旅成功！