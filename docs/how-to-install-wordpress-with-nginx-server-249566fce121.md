# 如何用 Nginx 服务器安装 Wordpress

> 原文：<https://blog.devgenius.io/how-to-install-wordpress-with-nginx-server-249566fce121?source=collection_archive---------1----------------------->

![](img/10f6aaa20da9eef53f82b3f41f76744b.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 下载 WordPress

```
mkdir workspace && cd workspace
curl -LO [https://wordpress.org/latest.tar.gz](https://wordpress.org/latest.tar.gz)
tar xzvf latest.tar.gz
mkdir example
cp -rf workspace/wordpress/* example
cp workspace/example/wp-config-sample.php workspace/example/wp-config.php
```

找到你的 WordPress 目录，我的是/home/ajay/workspace/example。通过从工作区目录执行以下命令来更改该目录的所有权和权限。

```
sudo chown -R www-data:www-data example/*
sudo chmod 777 example/*
```

# 安装 MySQL 并创建数据库和数据库用户

你需要确保你的系统中安装了 MySQL。如果没有，那么通过下面的命令安装它

```
sudo apt-get update
sudo apt-get install mysql-server
```

现在，通过以下命令创建一个数据库及其用户

```
sudo mysql
CREATE DATABASE wordpress;
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'password'; 
FLUSH PRIVILEGES;
EXIT;
```

# 设置 WordPress 配置文件

现在，打开 WordPress 配置文件:

```
sudo nano workspace/example/wp-config.php
```

该文件将如下所示，

```
define('DB_NAME', 'wordpress'); # replace it with your database name

/** MySQL database username */
define('DB_USER', 'wordpressuser'); # replace it with your db user name

/** MySQL database password */
define('DB_PASSWORD', 'password'); # # replace it with your db password
```

保存文件，然后按 ctrl+X 和 y 退出。

# 配置 Nginx

要安装 Nginx，你首先要在你的系统上安装 Nginx。所以你可以在 ubuntu 上安装它

```
sudo apt-get update
sudo apt-get install nginx
```

现在转到/etc/nginx/sites-available 并创建您的网站 conf 文件，例如，我正在创建 example.conf

```
server {
    listen 80;
    listen [::]:80;
    root   /home/ajay/workspace/example; # this is the root folder of wordpress
    index  index.php index.html index.htm;
    server_name  example.com [www.example.c](http://www.gsefdmit.test)om; # replace it with your website namelocation / {
        #try_files $uri $uri/ =404;
 try_files $uri $uri/ /index.php$is_args$args;
    }location ~ [^/]\.php(/|$) {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock; # using php7.2
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }}
```

现在通过以下命令检查 Nginx 配置

```
sudo nginx -t
```

如果一切正常，它将向您显示以下输出

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

现在创建一个软链接，使您的网站将被启用。为此，只需键入

```
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/
```

现在，通过以下命令重启 Nginx 服务器

```
sudo systemctl restart nginx
```

注意:在/etc/hosts 中添加域名。

# 安装附加的 PHP 扩展

需要注意的一点是，我们使用的是 php7.2-fpm。所以你需要确保你已经在你的系统中安装了它。如果您没有它，那么通过下面的命令安装它

```
sudo apt install php7.2-cli php7.2-fpm php7.2-curl php7.2-gd php7.2-mysql php7.2-mbstring zip unzip
```

如果安装成功，重启 php7.2-fpm

```
sudo systemctl restart php7.2-fpm.service
```

为了检查一切是否正常，我们检查了 Nginx 和 php7.2-fpm 的状态

```
sudo systemctl status php7.2-fpm.service
sudo systemctl status nginx
```

# 通过 Web 界面完成安装

在 web 浏览器中，导航到服务器的域名或公共 IP 地址(www.example.com)。接下来的步骤不言自明。

你喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCvEB7wXUEXGFE9lCx0USR3Q) **！**