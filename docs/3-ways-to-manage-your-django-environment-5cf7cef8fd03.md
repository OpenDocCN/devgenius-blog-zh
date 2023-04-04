# 管理 Django 环境的 3 种方法

> 原文：<https://blog.devgenius.io/3-ways-to-manage-your-django-environment-5cf7cef8fd03?source=collection_archive---------12----------------------->

![](img/9985e8132fb802c8dd7867929c97f7d1.png)

由[在](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我开发 Django web 应用已经有一段时间了。对于不同版本的 python 及其库，我们需要不同的环境来管理它们。然而，设置环境可能是一件令人头疼的事情。

今天我将介绍 3 种方法来解释我如何管理我的 Django 环境。

# 虚拟

python 项目可以有几种环境。每个都有自己的配置(env 变量)和包。我们可以把每个环境看作一个与外界(其他环境)隔绝的气泡世界。

在 Python3 中，它有自己的虚拟环境，名为 virtualenv。基本上，我们可以通过键入以下命令来调用环境:

```
python3 -m venv env
source env/bin/activate
```

在这个特定的环境中，您可以安装/更新软件包，添加/删除环境变量等。您必须始终确保项目运行在相同的环境中，以访问包和变量。

不使用 python 环境也没问题。然后，当您启动 python 项目时，它将自动使用系统的环境`/usr/local/bin`。显然这不是一个好的选择，因为你的其他应用程序很容易影响系统环境，你可能会出现一些奇怪的错误。

# Pipenv

经过一些研究，我发现了 Django 开发人员常用的工具:

[](https://github.com/pypa/pipenv) [## pypa/pipenv

### ~ pyup . io 的依赖性扫描~ ] Pipenv 是一个工具，旨在带来所有包装世界的最佳(bundler…

github.com](https://github.com/pypa/pipenv) 

Pipenv 自动为您的项目创建和管理一个 virtualenv，并在您安装/卸载软件包时从您的`Pipfile`中添加/删除软件包。

使用 Pipenv，您不必担心 pip 不与 python 环境绑定，因为该工具会自动创建和绑定环境，而且

因为我总是使用下面的命令将包信息写到 requirement.txt 中，以便持续集成。有时它只是写入一些不必要的系统包。这有时会有问题。

```
pip freeze > requirements.txt
```

Pipenv 使用`Pipfile`和`Pipfile.lock`将抽象依赖声明从最后测试的组合中分离出来。您可以通过将 Pipfile 上传到 CI 系统来关注它，或者如果您坚持使用 requirements.txt，则运行下面的命令。

```
pipenv run pip freeze > requirements.txt
```

# 码头工人

Docker 是管理 Django 环境的一个很好的选择，因为 Docker 本身是一个完全不同的环境和文件系统，包含了所有的库。

随着开发变得越来越复杂，我们总是有几个不同环境的团队，如 QA、生产、测试和开发等。每个环境都有自己的依赖项和配置。我们传过去的开发环境，不代表我们可以传过去的生产环境。

Docker 作为一个容器是一致的。它允许您以相同的方式打包，而不管您的操作系统是什么。它允许您分发软件，而不管设置如何。它允许您在运行软件的任何地方以同样的方式测试软件。

在这里，我附上了我的 Django 项目中的 docker 文件和 docker-compose 文件配置:

```
// DockerfileFROM python:3.7-alpine
MAINTAINER QI LIENV *PYTHONUNBUFFERED* 1COPY ./requirements.txt /requirements.txt
RUN apk add --update --no-cache postgresql-client
RUN apk add --update --no-cache --virtual .tmp-build-deps \
      gcc libc-dev linux-headers postgresql-dev
RUN pip install -r /requirements.txt
RUN apk del .tmp-build-depsRUN mkdir /app
WORKDIR /app
COPY ./app /appRUN adduser -D user
USER user// docker-compose.ymlversion: "3"services:
  app:
    build:
      context: .
    ports:
       - "8000:8000"
    volumes:
       - ./app:/app
    command: >
       sh -c "python manage.py wait_for_db &&
              python manage.py migrate &&
              python manage.py runserver 0.0.0.0:8000"
    environment:
      - DB_Host=db
      - DB_NAME=app
      - DB_USER=postgres
      - DB_PASS=supersecretpassword
    depends_on:
      - db
  db:
    image: postgres:10-alpine
    environment:
      - POSTGRES_DB=app
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=supersecretpassword
```

Dockerfile 通过一个或多个配置映像的构建命令来定义应用程序的映像内容。`docker-compose.yml`文件描述了制作应用程序的服务。这些服务是网络服务器和数据库。

在上面的例子中，我们添加了 python:3.7-alpine image。通过添加新的`app`目录来修改父映像。通过安装在`requirements.txt`文件中定义的 Python 需求，父映像被进一步修改。

有关更多解释，请参考下面的文章:

[](https://docs.docker.com/compose/django/) [## 快速入门:作曲和 Django

### 预计阅读时间:7 分钟本快速入门指南演示了如何使用 Docker Compose 来设置和运行一个

docs.docker.com](https://docs.docker.com/compose/django/) 

# 结论:

当我开发 Django 应用程序的时候。我实际上是一个接一个地逐步更新这三种方式。现在我和崔维斯. CI 的 Docker 在一起。总的来说，这都是关于我们如何有效地管理 python 环境。

感谢阅读。如果你有兴趣阅读我的其他文章，欢迎查看我的个人资料。你也可以通过媒体或邮件联系我。