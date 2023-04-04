# Django 登录/注册系统

> 原文：<https://blog.devgenius.io/django-login-registration-system-602e9b0cc842?source=collection_archive---------7----------------------->

#在开始之前导入必要的模块

> 从焦点导入*

![](img/e52987d9fa2263a047679bbd805c0013.png)

我想写一个简单的 Django 应用程序，展示用 Django 执行复杂的任务是多么简单。

希望它能帮助您理解 Django 强大的内置特性，因为当您第一次开始时，很难理解它。

## 先决条件

*   基本的 HTML 和 Python。
*   熟悉 Django 的基本工作流程。

## 目标

使用 Django 创建一个登录/注册系统，熟悉 Django 的工作流程。

## 特征

使用 Django 登录/注册系统，您可以利用以下功能:

*   注册
*   注销
*   签约雇用
*   密码重置

# 初始设置

我假设你已经安装了 Django。您可以通过在 shell 提示符下运行以下命令(由$前缀表示)来判断 Django 是否已安装以及是哪个版本:

```
$ python -m django --version
```

创建项目

```
$ django-admin startproject MySystem
```

创建应用程序

```
$ django-admin startapp sample
```

安装库:
这里我们将使用[django-crisp-forms](https://pypi.org/project/django-crispy-forms/)和[django-registration-redux](https://pypi.org/project/django-registration-redux/)

```
$ pip install django-crispy-forms$ pip install django-registration-redux
```

# 入门指南

管理设置(编辑已安装的应用程序)

```
MySystem/setting.py INSTALLED_APPS = [# Default apps'django.contrib.admin','django.contrib.auth','django.contrib.contenttypes','django.contrib.sessions','django.contrib.messages','django.contrib.staticfiles',# My apps'sample','registration',# Third Party Libraries'crispy_forms',]CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

添加根目录(在设置文件的末尾添加)

```
MySystem/setting.py LOGIN_REDIRECT_URL = "/home"PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))MEDIA_ROOT = PROJECT_ROOT + '/static/'MEDIA_URL = '/media/'PROJECT_DIR = os.path.dirname(os.path.abspath(__file__))STATIC_ROOT = os.path.join(PROJECT_DIR, 'static')
```

配置 URL

```
MySystem/urls.py from django.contrib import adminfrom django.urls import pathfrom django.urls.conf import includefrom django.views.generic.base import RedirectViewfrom django.conf.urls.static import staticfrom MySystem import settingsfrom sample import viewsurlpatterns = [path('admin/', admin.site.urls),path('accounts/', include('registration.backends.default.urls')),path('', views.home.as_view()),] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

为主页创建视图

```
sample/views.py from django.views.generic.base import TemplateViewclass home(TemplateView):template_name = "sample/home.html"
```

在 sample/templates 文件夹中创建用于登录、注册和主页的 html 页面。我们的文件结构如下所示:

```
sample/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
    templates/
        registration/
             login.html
             registration_form.html
        sample/
             home.html
        base.html
```

## 通过电子邮件验证用户

在设置文件的末尾添加以下代码，配置您的 smtp 电子邮件地址，以便每当新用户注册时发送验证码。

```
ACCOUNT_ACTIVATION_DAYS=3EMAIL_HOST= 'smtp.gmail.com'EMAIL_HOST_USER= 'yourEmailAddress'EMAIL_HOST_PASSWORD= 'yourPassword'EMAIL_PORT= 587
EMAIL_USE_TLS= True
```

## 现在让我们设计我们的模板:-

*base.html*

```
<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>Login/Registration</title></head><body>Welcome All!{% block content %}{% endblock %}{% if user.is_authenticated %}
<a href="/accounts/logout">Logout</a>{% else %}

<a href="/accounts/login">Logout</a>{% endif %}</body></html>
```

*registration_form.html*

```
{% extends 'base.html' %}  
{% block content %}   
<h2>Register</h2><form method="post">

{% csrf_token %} 
{{form | crispy}}<button type="submit">Register</button>   
</form><a href="/accounts/login">Already have an account</a><a href="/accounts/password/reset/">Forgot passwork?</a>{% endblock %}
```

*login.html*

```
{% extends 'base.html' %}  
{% block content %}   
<h2>Login</h2> <form method="post">

{% csrf_token %} 
{{form | crispy}}<button type="submit">Login</button>   
</form><a href="/accounts/register">Don't have an account</a><a href="/accounts/password/reset/">Forgot passwork?</a>{% endblock %}
```

*home.html*

```
{% extends 'base.html' %}{% block content %}<h2>Home Page</h2><h4>Welcome {{user.username}} </h4>
{% endblock %}
```

# 结论

这就是了。python 的 Django 框架下的一个完整的登录注册系统。

我希望这篇教程能帮助你理解 Django 的工作流程。使用这种松散耦合的模式可以为应用程序添加许多样板文件和抽象，但它也是一种可预测的、熟悉的模式，通常在许多框架中使用，并且是开发人员需要了解的一个重要概念。

如果你想讨论什么，请随时联系我。

如果您能发送您的反馈和建议，我将非常高兴。此外，我喜欢交新朋友，我们可以成为朋友，只要给我发邮件。

> *Web:*[https://portfolio . abhisheksrivastava . me](https://portfolio.abhisheksrivastava.me/) *insta gram:*[*https://www.instagram.com/theprogrammedenthusiast/*](https://www.instagram.com/theprogrammedenthusiast/) *LinkedIn:*[*https://www.linkedin.com/in/abhishek-srivastava-49482a190/*](https://www.linkedin.com/in/abhishek-srivastava-49482a190/) *Github:*[https://github.com/abhishek2x](https://github.com/abhishek2x) *邮箱:abhisheksrivastavabbn@gmail.com*

[](https://github.com/abhishek2x) [## abhishek2x -概述

### 在 GitHub 上注册你自己的个人资料，这是托管代码、管理项目和构建软件的最佳地方…

github.com](https://github.com/abhishek2x)  [## Abhishek Srivastava - Vellore 理工学院-印度北方邦勒克瑙| LinkedIn

### 查看 Abhishek Srivastava 在世界上最大的职业社区 LinkedIn 上的个人资料。阿布舍克的教育是…

www.linkedin.com](https://www.linkedin.com/in/abhishek-srivastava-49482a190/) [](https://abhishek2x.github.io/tech/main.html) [## Abhishek Srivastava 投资组合

### 天生的企业家和激情的开发者，技术爱好者，开源贡献者…

abhishek2x.github.io](https://abhishek2x.github.io/tech/main.html)