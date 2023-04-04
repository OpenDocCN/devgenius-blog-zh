# Django 的多个数据库

> 原文：<https://blog.devgenius.io/multiple-databases-in-django-925ca821bdd0?source=collection_archive---------0----------------------->

![](img/492012e01a9034a152befb8916070740.png)

费萨尔·米在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

大家好，希望你们都好。

在本教程中，我们将看到如何在 django 中使用多个数据库。在本教程中，我们使用了 **django 3.0** ，但它将适用于 django 3.0 以上的所有版本。

让我们首先为这个项目创建一个目录并设置虚拟环境。我在这个项目中使用 python 3.6 和 Django 3.0。

转到您想要创建项目的文件夹，安装 django 和其他所需的东西。(如果您在虚拟环境中创建项目会更好，它会将该项目与您的其他项目分开)。

创建名为 **checkingdb** 的项目

***django-admin 开始项目检查 db***

cd 检查 b

***python manage . py startapp contenter，python manage . py startapp customer，python manage . py startapp user checking***

现在在 settings.py 文件中配置数据库，但是如果使用 postgres，首先启动 pgAdmin 并创建新的数据库。在这里，我创建了名为 **contentdb、usersdb、customerdb** 的 3 个 db，其中 contentdb 是默认的，另外两个是额外的 db，您可以将它们用于其他目的。

首先在 INSTALLED_APPS 中添加所有应用程序，现在让我们配置数据库 _ 路由器。

```
DATABASE_ROUTERS = ['contenter.router.CheckerRouter']  # it consists the path where your router.py file reside. in my case it is in contenter app.
```

现在让我们在设置中配置数据库

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'contentdb',
        'USER': 'postgres',
        'PASSWORD': 'admin1234',
        'HOST': 'localhost',
        'PORT': '5432'
    },
    'usersdb': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'usersdb',
        'USER': 'postgres',
        'PASSWORD': 'admin1234',
        'HOST': 'localhost',
        'PORT': '5432'
    },
    'customerdb': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'customerdb',
        'USER': 'postgres',
        'PASSWORD': 'admin1234',
        'HOST': 'localhost',
        'PORT': '5432'
    }

}
```

我已经在 contenter app 中创建了 **router.py** 文件。下面是 **router.py** 文件的代码。

```
class CheckerRouter:

    def db_for_read(self, model, **hints):
        if model._meta.app_label == 'customers':
            return 'customerdb'
        elif model._meta.app_label == 'userchecking':
            return 'usersdb'
        return 'default'

    def db_for_write(self, model, **hints):
        if model._meta.app_label == 'customers':
            return 'customerdb'
        elif model._meta.app_label == 'userchecking':
            return 'usersdb'
        return 'default'

    def allow_relation(self, obj1, obj2, **hints):
        if obj1._meta.app_label == 'customers' or obj2._meta.app_label == 'customers':
            return True
        elif 'customers' not in [obj1._meta.app_label, obj2._meta.app_label]:
            return True
        elif obj1._meta.app_label == 'userchecking' or obj2._meta.app_label == 'userchecking':
            return True
        elif 'userchecking' not in [obj1._meta.app_label, obj2._meta.app_label]:
            return True
        return False

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        if app_label == 'customers':
            return db == 'customerdb'
        elif app_label == 'userchecking':
            return db == 'usersdb'
        return None
```

现在需要在所有应用程序的 models.py 中做一些更改，我们可以开始了。

contenter\models.py

```
from django.db import models

class Content(models.Model):
    app_name = models.CharField(max_length=100)
    language = models.CharField(max_length=100)

    def __str__(self):
        return self.app_name
```

客户\模型. py

```
from django.db import models

class CustomerChecker(models.Model):
    customer_name = models.CharField(max_length=100)

    class Meta:
        # app_label helps django to recognize your db
        app_label = 'customers'

    def __str__(self):
        return self.customer_name
```

userchecking \ models.py

```
from django.db import models

class UserChecker(models.Model):
    user_name = models.CharField(max_length=100)
    age = models.IntegerField()

    class Meta:
        app_label = 'userchecking'  # name of app

    def __str__(self):
        return self.user_name
```

不要忘记在 admin.py 文件中包含您的模型。

毕竟这些需要迁移所有的应用程序

***python manage . py migrate—database = customer db***

***python manage . py migrate—database = users db***

***python manage.py 迁移***

然后使用***python manage . py runserver***运行服务器，并转到浏览器

键入 **localhost:8000/admin** ，登录并在您的模型中添加数据，然后签入您的 db。

注意:-当您更改模型中的任何内容时，不要忘记使用***Python manage . py make migrations。***

以下是该项目的 github 链接

[](https://github.com/dj221ai/multiple-databases-in-django.git) [## DJ 221 ai/django 多数据库

### 这是一个简短的项目，展示了我们如何在 django3.0 中使用多个数据库

github.com](https://github.com/dj221ai/multiple-databases-in-django.git) 

如果你有什么建议，请告诉我。

谢谢你的时间。