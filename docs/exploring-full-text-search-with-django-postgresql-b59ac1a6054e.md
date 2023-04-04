# 使用 Django/PostgreSQL 探索全文搜索

> 原文：<https://blog.devgenius.io/exploring-full-text-search-with-django-postgresql-b59ac1a6054e?source=collection_archive---------3----------------------->

![](img/f09db7cb8cc2ca51d46ec30c6cd5db57.png)

在这个循序渐进的教程中，您将学习如何使用 PostgreSQL 数据库在 Django 应用程序中使用全文搜索。在本文结束时，您将对全文搜索在您的应用程序中的使用有一个透彻的理解。

本文的要求是要很好的了解 Django 的 ORM 和 Queryset 函数。

# 什么是全文搜索？

在我回答之前，让我问你一件事。文字处理器(MS Word，Notepad)中的查找和替换功能和 Google 中的搜索有什么区别？两者都在寻找，不同的是怎么做。谷歌把你的查询分解成不同的词，爬上每一个网站获取相关的结果，另一方面，查找替换把你的查询作为一个整体，在文本中搜索。前者是全文搜索。它分解查询并删除不必要的部分，如 in、or，然后返回最相关的结果。

# 为什么要进行全文搜索？

让我们用一个真实的例子来理解全文搜索的必要性。假设我们有两个给定标题的帖子，用户在 python 中寻找 async。

# 我们应用程序中的帖子标题示例:

`- Python also supports async function.
- Async in python is amazing`

用户在普通正则表达式搜索时只会得到第二篇文章，但是我们可以看到第一篇文章也与用户的查询相关。这就是我们在应用程序中需要全文搜索的原因。

# 全文搜索的功能

*   它要快得多。
*   结果是相关的。
*   更好的结果排名。

# 在 Python/Django 中搜索

在深入研究全文搜索之前，我想快速回顾一下 SQLite 数据库搜索的正常工作方式。

下面您将找到获取样板文件的存储库。我和 django 一起创建了一个名为 core 的新项目，在那里我创建了一个名为 blog 的新应用。运行`python manage.py migrate`之后，将这段代码添加到您的项目中。

blog/ *models.py* 存储博客文章内容和标题的新模型(表)。

```
from django.db import models# Create your models here.
class Post(models.Model):
 title = models.CharField(max_length=100)
 content = models.TextField()def __str__(self):
 return f”{self.title}”
```

blog/admin.py 在管理站点中添加帖子模型以使用创建/读取/更新/删除操作。

```
from django.contrib import admin
from .models import Post# Register your models here.admin.site.register(Post)
```

blog.views.py 添加主页视图以查看所有帖子

```
from django.shortcuts import render
from .models import Postdef home(request):
    qs = Post.objects.all()
    if request.GET.get('query'):
        qs = qs.filter(title__icontains=query)
    return render(request, "index.html", context={"queryset": qs})
```

core/urls.py 为主视图添加新的 URL。

```
urlpatterns = [
    path("admin/", admin.site.urls),
    path("", views.home),
]
```

# 要搜索的模板

blog/templates/index.html 用于搜索和查看 queryset 中所有帖子的模板

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Full Text Search</title>
  </head>
  <body>
      <form>
           {% csrf_token %}
          <input type="text" name="query">
          <input type="submit" value="Search">
    <form/>
    {% for object in queryset %}
    <h2>{{object.title}}</h2>
    <p>{{object.content | truncatechars:200}}</p>
    {% empty %}
    <h2>No Results</h2>
    {% endfor %}
  </body>
</html>
```

将这些更改添加到项目中后，在提示符/终端中启动该命令。

```
python manage.py runserver
```

转到[127 . 0 . 0 . 1:8000/admin/blog/post](http://127.0.0.1:8000/admin/blog/post/)并添加类似任何主题的任意五个帖子。

![](img/c81ee71a170478ca90b769bc8b2b2f87.png)

管理站点中的所有帖子

# ' title__icontains '如何工作。

我搜索机器人，查询集返回

![](img/55466755a169691110363a0c8f4c04f0.png)

没有全文的搜索结果

但是如果我搜索`python bots`

![](img/9beb721463554d6879fae438f01f7c72.png)

title__icontains 可以返回相关结果

没有结果，因为我在帖子的标题中没有具体的术语“python 机器人”。

让我们用全文搜索来纠正这个错误。

# Django 中如何使用 PostgreSQL？

全文只有在 PostgreSQL 这样的高级生产就绪数据库中才有可能。我们可以在 Django 应用程序中使用 PostgreSQL 数据库，方法是在本地下载数据库或使用在线数据库作为服务。在本教程中，我们将使用一个名为[的服务，一个 PostgreSQL 服务。](https://www.elephantsql.com/)

我们必须遵循的步骤:

*   创建远程 PostgreSQL 数据库
*   将此数据库连接到 Django 项目。
*   更改设置以包括 postgresql 数据库后端
*   安装 psycopg2，使用 Python 与 PostgreSQL 对话

在这个网站上创建一个新帐户，然后选择小海龟计划(免费/慢速)。创建新实例后，您将看到此页面。

![](img/7afe74c0be22a43fb65d359d6602f3c8.png)

ElephantSQL 中的实例详细信息屏幕

复制 URL，因为这是将这个数据库连接到您的本地 Django 应用程序所需要的唯一东西。

在设置文件 core/settings.py 中进行以下更改

```
.....INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "django.contrib.postgres",                  #THIS APP
    "blog",
]
.....
DATABASES = {
    "default": {
        # My URL
        """postgres://tdjcpial:[7UglyA2MM16ksuez2ICeDxfxZjlQnj9X@salt.db.elephantsql.com](mailto:7UglyA2MM16ksuez2ICeDxfxZjlQnj9X@salt.db.elephantsql.com)/tdjcpial"""
        # Change the below settings according to your Copied URL.
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "tdjcpial",
        "USER": "tdjcpial",
        "PASSWORD": "7UglyA2MM16ksuez2ICeDxfxZjlQnj9X",
        "HOST": "salt.db.elephantsql.com",
    }
}
.....
```

在这个改变之后，我们需要使用`pip install psycopg2`安装 db 适配器 psycopg2

现在我们的数据库已经完全连接了，要从管理站点使用这个数据库，运行`python manage.py migrate`，然后运行`python manage.py createsuperuser`

您现在可以在这个数据库中创建新的文章来测试全文功能。

# 要实现全文搜索，我们需要执行以下步骤:

*   解析查询
*   解析内容/数据

打开 blog/views.py 并添加以下代码，然后我们将逐行浏览。

```
from django.shortcuts import render
from .models import Post
from django.contrib.postgres.search import SearchVector, SearchQuery# Create your views here.def home(request):
    qs = Post.objects.all()
    query = request.GET.get("query")
    if query:
        # qs = Post.objects.filter(title__icontains=query)
        qs = Post.objects.annotate(search=SearchVector("title", "content")).filter(search=SearchQuery(query))
    return render(request, "index.html", context={"queryset": qs}) 
```

在上面的代码中，我们注释掉了 i_contains，并添加了一个由 Django PostgreSQL 后端提供的全文搜索类。

让我们了解一下每个班级都在做什么:

*   SearchVector(*fields):这个类处理查询需要搜索的数据，这个类从 PostgreSQL 中运行 ts_vector 函数，将单词分解成标记，并为每个标记分配一个类型。
*   SearchQuery(query):该类处理用户提供的查询，通过将查询分解为标记并删除查询中不必要的部分(如标点符号(，！？)和类似这样的词(and，or，要么)。

annotate 函数用值 SearchVector(title，content)在一个表中创建了一个新的临时列，这意味着我们希望为查询搜索这两个字段。然后，筛选器会筛选出那些在其搜索列中包含查询的行。

让我们测试一下将这一更改添加到视图后会得到什么。

![](img/2d964f6e466e0ee72865d82a0e6e22bd.png)

全文搜索中的相关结果

瞧啊。记得之前我们在`python bots`上没有得到任何结果，但现在我们的应用程序显示了一些与我们的查询相关的帖子。

# 还有更多:

# SearchHeadline():

如果您的查询与内容匹配，此类用于突出显示结果，第一个参数确定我们需要突出显示的位置在这种情况下，内容，第二个参数采用 SearchQuery(query ),我们已经讨论过了，第三个和第四个参数显示当找到匹配的标记时，我们应该如何将它们括起来，在这种情况下，我们使用粗体、斜体和下划线标签。将以下代码添加到您的 blog/views.py 中

```
from django.shortcuts import render
from .models import Post
from django.contrib.postgres.search import SearchVector, SearchQuery, SearchHeadline# Create your views here.def home(request):
    qs = Post.objects.all()
    query = request.GET.get("query")
    if query:
        # qs = Post.objects.filter(title__icontains=query)
        # qs = Post.objects.annotate(search=SearchVector("title", "content")).filter(
        #     search=SearchQuery(query)
        # )
        qs = Post.objects.annotate(
            headline=SearchHeadline(
                "content",
                SearchQuery(query),
                start_sel="<b><u><i>",
                stop_sel="</i></u></b>",
            )
        )
    return render(request, "index.html", context={"queryset": qs})
```

要做到这一点，我们需要稍微修改一下模板文件。

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Full Text Search</title>
  </head>
  <body>
      <form>
           {% csrf_token %}
          <input type="text" name="query">
          <input type="submit" value="Search">
    <form/>
    {% for object in queryset %}
    <h2>{{object.title}}</h2>
    <p>{{object.headline | safe}}</p>
    {% empty %}
    <h2>No Results</h2>
    {% endfor %}
  </body>
</html>
```

让我们来看看:

![](img/caa83b143cf9f02c8be577c0b510316e.png)

搜索词在结果中突出显示

我们将`{{object.content}}`改为`{{object.headline}}`，并添加了安全的模板标签过滤器，这样我们的 HTML `<b><u><i>query</i></u></b>`就可以执行了。

看看我们的查询是如何变得大胆而符合逻辑的。

# 结论

如需更多信息和文档:

*   [姜戈全文](https://docs.djangoproject.com/en/4.0/ref/contrib/postgres/search/)
*   [PostgreSQL 文档](https://www.postgresql.org/docs/current/textsearch-controls.html)

[GitHub 代码库](https://github.com/vivekthedev/full-text-search-blog-example)，我不会改变我的数据库的设置，以防你想篡改数据。

如果你喜欢这篇文章，也可以考虑看看我在 Twitter 上的表现，我在那里每天发布的文章不超过 280 个字符。