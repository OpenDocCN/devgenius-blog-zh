# 针织烧瓶和 Nginx 反向代理

> 原文：<https://blog.devgenius.io/knit-flask-nginx-reverse-proxy-5a7d65ccf912?source=collection_archive---------8----------------------->

1.  首先，确保您已经在服务器上安装了 Flask 和 nginx。
2.  接下来，创建一个 Flask 应用程序，它有一个返回简单消息的基本路由。
3.  创建一个名为“flask_app.py”的文件，并将以下代码放入其中:

```
from flask import Flask app = Flask(name)
@app.route('/') def hello(): return 'Hello from Flask!'
if name == 'main': app.run()
```

4.在同一个目录中，创建一个名为“app.conf”的文件，并将以下代码放入其中:

```
 server { listen 80; server_name yourdomain.com;

  location / {
      proxy_pass http://127.0.0.1:5000;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
  }
}
```

5.在“app.conf”文件中用您的实际域名替换“yourdomain.com”。

6.打开您的 nginx 配置文件，通常位于/etc/nginx/nginx.conf，并在末尾添加以下行:

```
include /path/to/app.conf;
```

7.使用“服务 nginx 重启”命令重启 nginx。

8.使用命令“python flask_app.py”启动 Flask 应用程序。

9.最后，在 web 浏览器中导航到您的域，您应该会看到消息“您好，来自 Flask！”

10.就是这样！您的 Flask 应用程序现在使用 nginx 作为反向代理运行。