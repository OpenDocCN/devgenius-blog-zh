# 微服务样板——带 Lumen + Laravel + Nginx + Mysql →零件(1/3)

> 原文：<https://blog.devgenius.io/micro-services-with-lumen-laravel-nginx-mysql-70b3554e8068?source=collection_archive---------0----------------------->

![](img/52c27fb2c699b0fe7363e44b962dac77.png)

## 在这个系列中

*   [微服务样板文件:设置(第 1/3 部分)](https://medium.com/@shadysmaoui/micro-services-with-lumen-laravel-nginx-mysql-70b3554e8068)
*   [微服务样板:设计(第 2/3 部分)](https://medium.com/@shadysmaoui/microservices-boilerplate-with-lumen-laravel-nginx-mysql-part-2-3-a3c0cc241cf4)

## 介绍

在这篇文章中，我将一步一步地演示用`Lumen`和`Laravel`以及作为 web 服务器的`nginx`和`mysql`来对接您的微服务项目的过程。这是三篇文章系列的第一部分。这个初始部分将允许我们建立我们项目的基础，对其主要组件进行分类，并建立基础，以便在所有组件之间进行流畅的交流。

下面是与这个项目相关的 Github 资源库:

【https://github.com/shsma/microservices-workspace】

## 先决条件:

*   `Docker`
*   `Docker-compose`

## 技术

*   `Lumen -v 6.x`:它是构建基于`Laravel`的微服务和超快的 API 的完美解决方案。这是目前最快的微框架之一。
    [https://lumen.laravel.com/](https://lumen.laravel.com/)
*   `Laravel -v 6.x` : Laravel 是一个 web 应用框架，具有丰富、优雅的语法。
    [https://laravel.com/](https://laravel.com/)
*   `Nginx:`它是一种网络服务器，可为互联网上最繁忙的网站加速内容和应用交付、提高安全性、促进可用性和可扩展性。
    [https://www.nginx.com/](https://www.nginx.com/)
*   我们将使用`php-7.4`和`fpm`进程管理器。
*   我们将使用`mysql 8.0`并利用它的超快的速度。

## 设置

创建`microservices-workspace`文件夹:

`->$ mkdir microservices-workspace && cd microservices-workspace`

`->$ mkdir apps`

现在，在我们的工作区中，我们创建了`docker`文件夹，以及在`apps`文件夹中的两个组件`curator`和`dipenser`文件夹。

*   `Curator`是 Laravel 应用程序，它将包含视图和核心逻辑，以及迁移和模型。
*   `Dispenser`是 Lumen 应用程序，它将充当微服务，分发/应答来自`curator`的呼叫。

`->$ composer create-project --prefer-dist laravel/laravel apps/curator`

`->$ omposer create-project --prefer-dist laravel/lumen apps/dispenser`

我们将创建一个 docker 文件夹，稍后再处理:

`->$ mkdir docker`

最后，我们需要在项目的根目录下创建一个`docker-compose`文件:

`->$ touch docker-compose.yml`

## **文件夹结构**

以下是我们的目标文件夹结构:

![](img/1779af85983cf3dfc2ef1ba23602fb44.png)

文件夹结构

## **设置 Docker + Docker-compose**

## Docker 文件

由于我们不会为两个`php` 应用程序使用来自`docker-compose`文件的预编译图像，我们需要为每个组件提供一个特定的`Dockerfile`:

→ `.apps/curator/Dockerfile`

→ `.apps/dispenser/Dockerfile`

## docker-撰写

→首先，我们需要在我们的`docker-compose.yml`中设置`Nginx`网络服务器组件。

![](img/bad8cdf75e64319051e335c3372fee58.png)

Nginx 组件

→既然 web 服务器已经在我们的 docker-compose 文件中设置好了，是时候建立两个主要组件了，`curator`和`dispenser`:

![](img/77962bdc3fd44e5f0a2c8722caaaf17d.png)

策展人组件

![](img/5869ae245ce93d0b8195d9dcbce884ba.png)

分配器组件

在这一阶段，我们的 web 服务器和两个主要组件已经安装完毕。剩下要做的就是设置`db`组件。我们必须记住网络设置，以便 web 服务器按照`bridge`设计模式的规定正常运行，最后是`volumes`确保我们的`db`组件的某种持久性。

![](img/b94e503e37a5c69a555fb59b0d4e2195.png)

Db 组件

最终结果是，`docker-compose.yml`文件应该如下所示:

## Docker 文件夹

![](img/0937f1301d840e2cde28e632deccc126.png)

Docker 文件夹结构

目前，我们将关注`mysql`和`php`子文件夹，下一步我们将关注包含更复杂结构的`nginx`子文件夹。

为了确保我们的整个设置功能正常，我们必须提供一个有效的`php`设置，以便`docker-compose`文件可以将它链接到应用程序容器中正确的`local.ini` `php`。

`->$ cd docker && mkdir php`

`->$ touch php/local.ini`

将以下设置复制到`local.ini`文件中。

```
upload_max_filesize=40M
post_max_size=40M
```

同样，我们必须设置`mysql`配置，以确保`db`和`mysql`容器正常工作:

`->$ mkdir mysql && touch mysql/my.cnf`

将以下内容复制粘贴到与 db 容器内的`my.cnf`链接的`my.cnf`文件中:

```
[mysqld]
general_log = 1
general_log_file = /var/lib/mysql/general.log
secure-file-priv=NULL
```

## Nginx 文件夹

首先，我们必须设置`ssl`证书，我们将使用这些证书通过`https`协议为我们的应用程序提供服务。最初，我们将在`docker`文件夹中创建一个`ssl`文件夹，并且我们将创建私有和公共密钥，如下所示。

为了您的方便和保持简单，您将在下面找到私钥和公钥，您可以简单地复制并粘贴到它们的适当位置

→ `./docker/nginx/ssl/ssl_cert.key`

```
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDkqvlH/9a/JiSZ
XR6/Fvb8Q8uaWR+sF/wzD3/d/mrDBMZYyVUVEaTgYiwLe73IFZnvPbijENFKlo/j
o61XtmcPdl690dL/aekFKy7rQoU3nbaxr1MimNY3MZ2kXLL8fgWqs28/Se0EjqUM
O80imUBp3RwvxUa0nY9ooJ09dxPP7O7hjfiNpXYQgZ/LqFOjEeVtjsrI5vT6cKAU
dwEiML12BmXSuJCGsjChmHf6j1J1UkT2jZe9wRyp8HBuP3K6ZN9b/+UV6tmGobzX
jQDM9c7sI0Xgi/Ck3jlP+I8CGokIfESASwkCa0P7xWuOdSMa+DI201GtJiqMi57H
9bINrKoxAgMBAAECggEAGO/YTZTF64SSWzM/hiFQAAS4sHVlsdPPyhW3oRL8u5XK
t6YFJE4PR/UMzpfJA4fviJqXhxjvag3vVSjlX3ph1NrYzCSUuQvowIqMDFXtxzno
0YnILC60tcIwads+HbD0l0AIMsOyGFwh/aqL3WkPEDxvtXnzEwFXwVY6r5hLzdHZ
CstyfkfyBqRfOvBltyt0YraMvr3Icubh/DnK48HzUx3LRQHhPmJSPVVUTgCNtvk/
NtNJofs9bOUc8YZbJPb3WyDJg/DR5g2qEgYASTZNCiyOs89+H/cHBknfGymWdKnp
Tx9BaZGGcrkizlYMqa62LBK8nw3ne88CXcQOAkV0AQKBgQD9oWy/TgmTJjG7heJl
1MIAVPdAdHM+/0wGXHSnoR1hjWE/XO6suQlP0WQa8/YDkVEfcOiHeiH//oorWuTD
hMLKiJXifDSJAnH8ionUEDCh5JJMuhd7ILPrJWB44OeYVWzNzEJqleP4zLp7gg8l
6HBbGhoTW1OIPnBImHiiKpJqQQKBgQDmzdlS+fXsx8ShkOZAD/ZvNYAZIXZay+vd
6in/U9d3YTVYOFpyf2kGppkZmiSw34dIaEp11nwjSo+TPwHUKdg6gRitbpwQlWU5
j2IDUp3otI1AodhcdlVlcAP1tkohArWO86DeAmX7MMbl5QnKV/vvg0b9tY99UGrZ
XRJB19/j8QKBgQDPG949aXAQJQQ/aNiy5sV0l+0/SNhC/R4Lo7vulnXzjwt2PmD3
0jF0aDwidkzSJPvlfMGkv2q8LBKPzK05q1OVJ2udgmcz6jdCTa626HGOAaQl22AJ
P28r+V2J173GextaAg3KE5QROryc1lugTOpSukHIxK61dqch4L/BTJo7AQKBgGj4
CfTw9eYAXO9WJUHRgLTHUkEbtOZKhDVjQEJ9cSs8LLS4vHoCdFOwBUVdk35VywMl
W2gIamDqki3XpA9IEK1a9Uw7WAM6ztTE30C/nE2vf+Z7NRmoP+JodYSUqVYmE0i2
iN0vng4F+w+ngTJjnZMq02ij4GSP5GiDKcCEW7EhAoGBAKisBPhj2b2+spGGHdru
9IsPA6aFiOjrz2RgXangEpulliNfjQK1px9yzo0INdwxB5kXZOEKSNGuBVgeu1zm
FOaiomVfpTfQbTc7GunGeg+tU7fxpwyJ2F4yf93KibZZBQajv1+kYVjvEID+5oDs
PsN8Wxl8hzv36p4674bkzWxj
-----END PRIVATE KEY-----
```

→ `./docker/nginx/ssl/ssl_cert.pem`

```
-----BEGIN CERTIFICATE-----
MIIDLjCCAhYCCQDL5Hz3lJ4c4TANBgkqhkiG9w0BAQsFADBZMQswCQYDVQQGEwJD
QTEPMA0GA1UECAwGUXVlYmVjMREwDwYDVQQHDAhNb250cmVhbDERMA8GA1UECgwI
TFhSYW5kQ28xEzARBgNVBAMMCmx4cmNvLnRlc3QwHhcNMTgwOTI1MjAxMjAzWhcN
MTkwOTI1MjAxMjAzWjBZMQswCQYDVQQGEwJDQTEPMA0GA1UECAwGUXVlYmVjMREw
DwYDVQQHDAhNb250cmVhbDERMA8GA1UECgwITFhSYW5kQ28xEzARBgNVBAMMCmx4
cmNvLnRlc3QwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDkqvlH/9a/
JiSZXR6/Fvb8Q8uaWR+sF/wzD3/d/mrDBMZYyVUVEaTgYiwLe73IFZnvPbijENFK
lo/jo61XtmcPdl690dL/aekFKy7rQoU3nbaxr1MimNY3MZ2kXLL8fgWqs28/Se0E
jqUMO80imUBp3RwvxUa0nY9ooJ09dxPP7O7hjfiNpXYQgZ/LqFOjEeVtjsrI5vT6
cKAUdwEiML12BmXSuJCGsjChmHf6j1J1UkT2jZe9wRyp8HBuP3K6ZN9b/+UV6tmG
obzXjQDM9c7sI0Xgi/Ck3jlP+I8CGokIfESASwkCa0P7xWuOdSMa+DI201GtJiqM
i57H9bINrKoxAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAHL+nIFZO4wtEWfewdI9
9paXoHpqHvY/twLU5Jekx7REv5+deYH4/aaq+gcDu3zgSCJz9mu/v8e+EDRVAmJf
yX01JMGuRC/G10YRiUYsaHXlYcn7dZsxPnZ1zi5vAiNymaxk14vIkpnkAjRe56B6
6iUuyGesu3xSRLCZkAi+YMQYq6qIqIn4LW87oQ6CrM0tTpb8u1QPQlz8FJ5MHLkf
ldXfQmbF3yGndl+cEsDKsExYV72vbJ441cpVGJmTupv5q4QOqsHgv1cnwXjvqio/
b0fQBBOE//o1aTgA1rZVWBONlTC4QBY7TzJe7+B2DEDQlXfWVOnB8dkkqNsRKtWo
ywA=
-----END CERTIFICATE-----
```

此时，`ssl`证书应该是好的，并且已经设置好了，这允许我们继续构建我们的`Nginx`服务器。

在`Ngnix`文件夹的根目录下有我们的主文件夹`nginx.conf`，它包含 web 服务器的核心配置，我们稍后将把特定于每个组件的配置转置到其中。

→ `./docker/nginx/nginx.conf`

在`/nginx`文件夹中，我们将创建一个`/sites`文件夹，其中包含特定于每个组件的配置:

→ `./docker/nginx/sites/curator.conf`

→ `./docker/nginx/sites/dispenser.conf`

在流程的这个阶段，最后一个任务是将域名添加到我们的主机中，放在`/etc/hosts`下，这样我们就可以在本地主机上提供服务。

→在`/etc/hosts:`的底部添加以下一行

`127.0.0.1 dispenser.shadysmaoui.test
127.0.0.1 curator.shadysmaoui.test`

我们现在准备尝试构建和运行我们的组件，但是首先我们应该在`.env`文件中为每个应用程序生成应用程序密钥。因此，导航到应用程序文件夹并执行这两个命令..

`cp .env.example .env`

`php artisan key:gen`

从项目的根目录中运行“生成并运行”命令:

`docker-comopse build && docker-compose up -d`

构建完成了，我们的容器也准备好了，我们需要从每个容器中运行
`composer install`。

现在访问下面的域

`[https://dispenser.shadysmaoui.test](https://dispenser.shadysmaoui.test) && https://[curator.shadysmaoui.test](https://curator.shadysmaoui.test)\`

![](img/1607421950d1bfd8ac5af0ed8955bc06.png)

管理者

![](img/53888367ce672b8ea000385e60480ab3.png)

药剂师

开始了。为了在这个系列中促进开发工作流，我们已经正确地完成了我们的设置。

如果你喜欢这篇文章，请**鼓掌并订阅**！干杯！！✌🏽💕💕👌🏽

别忘了明星[https://github.com/shsma/microservices-workspace](https://github.com/shsma/microservices-workspace)；)

**在这里继续阅读第二部分:**

[](https://medium.com/@shadysmaoui/microservices-boilerplate-with-lumen-laravel-nginx-mysql-part-2-3-a3c0cc241cf4) [## 微服务样板文件—带有 Lumen + Laravel + Nginx + Mysql →部分(2/3)

### 现在，我们的设置已经完成，可扩展架构的基础也准备好了，我们可以向前看并思考…

medium.com](https://medium.com/@shadysmaoui/microservices-boilerplate-with-lumen-laravel-nginx-mysql-part-2-3-a3c0cc241cf4)