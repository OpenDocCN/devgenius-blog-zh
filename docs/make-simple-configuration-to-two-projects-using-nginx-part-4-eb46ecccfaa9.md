# 使用 Nginx 对两个项目进行简单配置(第 4 部分)

> 原文：<https://blog.devgenius.io/make-simple-configuration-to-two-projects-using-nginx-part-4-eb46ecccfaa9?source=collection_archive---------4----------------------->

![](img/f2a8b7908b7fedec6d07c2a65a6c64f4.png)

# 简介:

在以前关于 Nginx 的文章中，我们谈到了 Nginx 的强大，我们如何在我们的机器上安装 Nginx，我们谈到了 Nginx 文件系统并解释了重要的文件和文件夹，但直到现在才使用 Nginx 配置。

在本文中，我们将制作一个简单的项目并测试它，我们将更好地理解 Nginx 文件，我们将使用一些 Nginx 命令。

我认为这篇文章可以让你很好地理解 Nginx 的一般知识，以及 Nginx 的配置和文件。

好吧，让我们继续这篇文章。

**Nginx 文章的所有部分:**

*   [Nginx Web 服务器简介(第一部分)](https://medium.com/javarevisited/intro-to-nginx-web-server-part-1-bb590fad7035)
*   [安装 Nginx 并通过命令管理(第二部分)](https://medium.com/javarevisited/installing-nginx-and-managed-by-command-part-2-b6b32b90b62d)
*   [Nginx 文件配置和日志(第三部分)](https://medium.com/p/62e71ad1c3a0)
*   你在 Nginx 系列的第四部分

# nginx 配置组件:

*   **HTTP 块:**

http 块包含用于处理 web 流量的指令，这些指令通常被称为通用指令，因为它们被传递给 NGINX 服务的所有网站配置。使用 include 指令传递用于处理不同类型 web 流量请求的配置文件，后跟配置文件路径。

*   **服务器块:**

服务器块包含服务器配置指令，其中包含服务器名称和 TCP 端口等信息。listen 指令告诉 NGINX 主机名/IP 和应该监听 HTTP 连接的 TCP 端口。当请求到达服务器时，server_name 指令指示 NGINX 选择服务器(从多个服务器块中)。

*   **位置块:**

***位置*** 模块使您能够在一个服务器模块内处理几种类型的 URIs/路线。通常，您会利用一个或多个正则表达式来定义和处理一类路由。该查找然后进行到比较和匹配位置块，并且服务于最接近请求 URI 命中的一个。

# 两个应用程序配置

例如，我们需要在我们的服务器中处理两个不同的应用程序，默认情况下 Nginx 包含一个服务器块，但在我们的情况下，我们需要创建另一个服务器块来处理任何应用程序的请求。

但是本文的第一步我们需要为我们的应用程序创建一个简单的 HTML 页面。

**让我们**去创建一个简单的 Html 页面

第一步，我们需要通过以下命令转到 **/var/www** 路径:

```
cd /var/www/
```

并在 **var/www** 路径下创建两个文件

```
mkdir test.com/html example.com/html
```

在我们的 HTML 页面中，我们需要创建 file.html，Nginx 将在发送请求时向用户提供这个页面。

*   现在我们需要创建一个简单的名为`index.html`的 **HTML** 文件

```
nano /var/www/test.com/html/index.html
```

*   并在其中编写这段代码

```
<html><head><title>Welcome to test.com!</title></head><body><h1>Success!  The test.com server block is working!</h1></body></html>
```

用同样的方法为 example.com 创建另一个 html 页面

> 你可以用这种方式将 html 文件复制到其他路径中

```
cp /var/www/test.com/html /var/www/example.com/html/index.html
```

> 不要忘记修改它，使其引用我们的第二个域

现在我们有一个 HTML 页面，但 Nginx 不知道我们的 HTML 页面在哪里，我们没有任何配置来处理请求，在这一部分我们需要创建 server_block 来进行正确的配置，并处理所有请求的正确路径。

让我们在配置中创建 ***server_block*** 。

默认情况下，Nginx 包含一个名为 default 的服务器块，我们可以使用它作为自己配置的模板。我们将从设计第一个域的服务器块开始，然后将它复制到第二个域中，并进行必要的修改。

目前，如果我们去/etc/nginx/available-sites 路径，我们可以找到一个名为 default 的文件，我们将把这个文件中的所有内容复制到其他文件 example.com 和 test.com。

我们可以通过以下命令复制**默认**文件的内容:

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example.com
```

> 不要忘记对**test.com**文件重复上述命令

并通过以下命令打开该文件**example.com**文件:

```
nano /etc/nginx/sites-available/example.com
```

当执行上述命令时，您可以看到下面的结果我们需要用正确的配置将这个结果更新到***example.com***并且我们需要用这个文件链接***example.com/html/index.html***。

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

让我们进行更新，为 example.com 进行正确的配置。

> **default_server:** 该语法意味着任何没有 ***server_name*** 的请求都将重定向到该服务器。
> 
> **我们可以在 Nginx** 中定义该命令一次

现在我们需要将 HTML 文件改为 exmaple.com HTML 文件:

当在 example.com 文件中应用以下语法时，我们可以进行这种修改。

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/example.com/html;

}
```

现在我们需要将服务器名修改为[**【www.example.com】**](http://www.example.com)我们在对 ***服务器名*** 属性进行以下修改时使**修改**:

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/example.com/html; server_name example.com www.example.com;
}
```

> 现在你可以保存并退出 example.com 文件
> 
> 并且我们需要**对**test.com**文件中的**做同样的修改，以同样的方式链接**域名**和 **HTML 文件**。
> 
> **但是别忘了改变:**
> 
> **服务器名**test.com[www.test.com](http://www.test.com)
> 
> **并更改 HTML 目录**

之后，我们需要启用服务器块。我们可以执行以下命令来启用它:

在上一节中，我解释了启用站点的文件，在这一点上我们需要将**/etc/nginx/sites-available**与/**etc/nginx/sites-enabled**链接起来，下面的命令可以将**可用文件**与**启用文件**链接起来:

```
$ sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/$ sudo ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/
```

> 这是 **/sites-enabled** 的任务

好了，现在我们有三个 server_block 由 Nginx 启用:

*   `example.com`:将响应`example.com`和`[www.example.com](http://www.example.com)`的请求
*   `test.com`:将响应`test.com`和`[www.test.com](http://www.test.com)`的请求
*   `default`:将响应端口 80 上与其他两个块不匹配的任何请求。

> 但是 nginx.config 文件在哪里呢它的作用是什么。

好的，这篇文章的下一部分将会是 **nginx.config** 和**修改它以查看**新服务器**。**

我们需要打开 nginx.config。我们可以使用以下命令打开文件。

```
nano /etc/nginx/nginx.config
```

执行上述命令后，您可以看到以下文件:

```
user www-data;worker_processes auto;pid /run/nginx.pid;include /etc/nginx/modules-enabled/*.conf;events {worker_connections 768;# multi_accept on;}http {### Basic Settings##sendfile on;tcp_nopush on;tcp_nodelay on;keepalive_timeout 65;types_hash_max_size 2048;# server_tokens off;client_max_body_size 1024M;server_names_hash_bucket_size 64;# server_name_in_redirect off;include /etc/nginx/mime.types;default_type application/octet-stream;### SSL Settings##ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLEssl_prefer_server_ciphers on;### Logging Settings##access_log /var/log/nginx/access.log;error_log /var/log/nginx/error.log;### Gzip Settings##gzip on;....}
```

> **注意:**以上文件不是 nginx 的默认文件，但我更新了它结论:

现在我们需要链接 **/sites-enable** 文件和 **/nginx.config**

我们必须将上述文件中的以下属性添加到与 **nginx.config** 链接的 **/sites-enabled** 文件中。

```
include /etc/nginx/sites-enabled/test.com;
include /etc/nginx/sites-enabled/example.com;
```

我们需要在下面一行从 nginx.config 中删除`#`

```
http {
    . . .

    server_names_hash_bucket_size 64;

    . . .
}
```

**测试重启所有服务的 nginx 配置**

我们需要测试所有的配置，以确保我们编写的配置文件没有任何错误。

我们可以编写以下命令来测试 Nginx 中的所有配置文件。

```
sudo nginx -t
```

如果没有任何问题，您可以通过以下命令重新启动 Nginx 服务。

```
sudo systemctl restart nginx
```

如果您在本地机器上使用 Nginx，您必须修改主机的文件以在浏览器中测试结果。

在第一步中，我们需要转到 **/etc/hosts** 路径，您可以执行下面的命令来转到这个路径。

```
nano /etc/hosts
```

执行上述命令后，您可以看到以下结果。

```
127.0.0.1   localhost
. . . 
```

将本地主机与 test.com 或示例链接。我们可以在 **/etc/hosts** 中编写下面的命令

```
your-ip-address example.com www.example.com
your-ip-address test.com www.test.com
```

`your-ip-address eg:128.25.255.45`

## 我们浏览器中的测试结果

现在我们需要打开**浏览器**进行测试，看看我们的配置的最终结果是什么。

在**浏览器中，**我们可以访问`[www.test.com](http://www.test.com)` **或** `www.example.com`

第一块给我提供了以下内容

![](img/1d75ef5689ee885611b79cf2ce5b949b.png)

第二块给我提供了以下内容

![](img/c3217faa6af9092c09b8b429d1eb69f6.png)

根据上面的结果，所有的请求都是正确的，没有任何问题。

# **结论:**

在本文中，我们看到了如何进行配置？用提供的 HTML 页面创建一个简单的应用程序，在带有**主机名**的浏览器中查看，我们可以看到 Nginx 的配置是多么简单。

我们看到了如何在同一个 Nginx 中用不同的主机名映射两个应用程序。

你可以打开 [**原 Nginx 网站**](https://www.nginx.com/) 或 [**数字海洋网站**](https://www.digitalocean.com/community) 通过原 Nginx 文档深入了解 Nginx。

**Nginx 文章的所有部分:**

*   [Nginx Web 服务器简介(第一部分)](https://medium.com/javarevisited/intro-to-nginx-web-server-part-1-bb590fad7035)
*   [安装 Nginx 并通过命令进行管理(第二部分)](https://medium.com/javarevisited/installing-nginx-and-managed-by-command-part-2-b6b32b90b62d)
*   [Nginx 文件配置和日志(第 3 部分)](https://medium.com/p/62e71ad1c3a0)
*   你在 Nginx 系列的第四部分

> 不要忘记在文章上拍手，每篇文章可以拍手 50 次。

# 参考资料:

*   [https://kinsta.com/knowledgebase/what-is-nginx/](https://kinsta.com/knowledgebase/what-is-nginx/)
*   [https://medium . com/devopscorry/what-is nginx-understanding-the-basics-of-nginx-in-2021-f8e E0 f 3d 3d 54](https://medium.com/devopscurry/what-is-nginx-understanding-the-basics-of-nginx-in-2021-f8ee0f3d3d54)
*   [https://www . digital ocean . com/community/tutorials/how-to-install-nginx-on-Ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
*   [https://www . digital ocean . com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-Ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04)