# 开始使用 Django

> 原文：<https://blog.devgenius.io/get-started-with-django-2c9c51031cc0?source=collection_archive---------25----------------------->

对于初学者来说，设置 Django 服务器可能是一项艰巨的任务，但是只要有正确的步骤和一定的耐心，就可以轻而易举地完成。在本文中，我将指导您完成设置 Django 服务器的过程，并向您展示一些代码示例。

![](img/8dc1e1b50ecd9b6a0765ba1891aeefdb.png)

首先，确保您的计算机上安装了 Python。如果没有，可以从 Python 官方网站下载。接下来，使用以下命令安装 Django 框架:

```
pip install django
```

一旦安装了 Django，使用下面的命令创建一个新项目:

django-admin start project my _ project

这将创建一个名为“my_project”的新目录，其中包含 Django 项目所需的文件和文件夹。打开目录并运行以下命令启动服务器:

```
python manage.py runserver
```

您的服务器现在运行在 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 上。通过在地址栏中键入该地址，您可以在 web 浏览器中访问它。

接下来，我们将在项目中创建一个新的应用程序。这个应用程序将包含我们网站的代码和功能。在终端中，导航到“my_project”目录并运行以下命令:

```
python manage.py startapp my_app
```

这将创建一个名为“my_app”的新目录，其中包含应用程序所需的文件和文件夹。在应用程序中，创建一个名为“views.py”的新文件。这是我们为网站功能编写代码的地方。

创造一个简单的“你好，世界！”页面上，将以下代码添加到“views.py”文件中:

```
from django.http import HttpResponse

def index(request):
  return HttpResponse("Hello, World!")
```

这创建了一个名为“index”的函数，它返回一个简单的“Hello，World！”访问时的消息。接下来，我们需要将这个函数映射到一个 URL，这样就可以在我们的网站上访问它。在“my_app”目录中，打开“urls.py”文件并添加以下代码:

```
from django.urls import path
from . import views

urlpatterns = [
path('', views.index, name='index'),
]
```

这将“索引”功能映射到我们网站的根 URL。现在，当我们访问 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) “你好，世界！”将显示消息。

现在我们已经设置了一个基本的应用程序，让我们添加一些模板和静态文件。在“my_app”目录中，创建一个名为“templates”的新目录，并在其中创建一个名为“index.html”的新文件。这是我们将为我们的网站布局编写 HTML 代码的地方。

将以下代码添加到“index.html”文件中:

```
<!DOCTYPE html>
<html>
<head>
    <title>My Django App</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

这创建了一个简单的 HTML 页面，带有“Hello，World！”航向。要在我们的应用程序中使用此模板，请打开“views.py”文件并更新“index”函数，如下所示:

```
from django.shortcuts import render

def index(request):
return render(request, 'index.html')
```

当访问“index”函数时，它使用“render”函数来呈现“index.html”模板。

最后，我们需要添加一些静态文件供我们的应用程序使用。在“my_app”目录中，创建一个名为“static”的新目录，并在其中创建另一个名为“my_app”的目录。这是我们可以添加 CSS 和 JavaScript 文件的地方。

通过这些步骤，您已经成功地设置了一个 Django 服务器，并使用模板和静态文件创建了一个基本的应用程序。现在，您可以继续开发您的应用程序，并根据需要添加更多功能。

为了进一步开发您的应用程序，您可以添加更多视图和 URL 模式来处理不同的页面和功能。例如，要创建显示项目列表的页面，您可以添加一个新的视图和 URL 模式，如下所示:

在“views.py”文件中，添加以下代码:

```
from django.shortcuts import render
from .models import Item

def item_list(request):
items = Item.objects.all()
return render(request, 'item_list.html', {'items': items}) 
```

这将创建一个名为“item_list”的新视图，该视图从“item”模型中获取一个项目列表，并将其传递给“item_list.html”模板进行呈现。

在“urls.py”文件中，添加以下代码:

```
from django.urls import path
from . import views

urlpatterns = [
path('', views.index, name='index'),
path('items/', views.item_list, name='item_list'),
]
```

这将“item_list”视图映射到 URL“[http://127 . 0 . 0 . 1:8000/items/](http://127.0.0.1:8000/items/)”。

要显示模板中的项目列表，请在“templates”目录中创建一个名为“item_list.html”的新文件，并添加以下代码:

```
<!DOCTYPE html>
<html>
<head>
    <title>My Django App</title>
</head>
<body>
    <h1>Item List</h1>
    <ul>
        {% for item in items %}
        <li>{{ item.name }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

它使用 for 循环遍历项目列表，并在列表中显示每个项目的名称。

通过这些步骤，您已经添加了一个用于显示项目列表的新视图和 URL 模式。您可以继续开发您的应用程序，并根据需要添加更多功能。