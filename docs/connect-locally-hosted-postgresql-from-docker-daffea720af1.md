# 从 Docker 连接本地托管的 PostgreSQL

> 原文：<https://blog.devgenius.io/connect-locally-hosted-postgresql-from-docker-daffea720af1?source=collection_archive---------2----------------------->

![](img/040acae1708f53132b065af07adc20a2.png)

照片由[沙哈达特·拉赫曼](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> **先决条件**

*   在系统中本地安装 PostgreSQL。
*   让 Docker 容器启动并运行。

> **从 docker 容器连接 PostgreSQL 的步骤**

*   获取本地 IP 地址，类似于`192.168.1.111`(在基于 linux 的系统中使用 ipconfig)
*   使用`docker exec -it <containerid> <entrypoint>`进入 docker
*   尝试 Ping 命令`Ping 192.168.1.111`，它将验证 docker 容器可以到达本地机器。
*   尝试 Telnet 命令`telnet 192.168.1.111 5432`，验证 docker 可以连接本地系统上的 db 实例。如果失败，请遵循以下步骤。

> **修复:万一连接失败**

*   找到**PostgreSQL . conf**
    `$ find / -name "postgresql.conf"
    /var/lib/pgsql/9.4/data/postgresql.conf` 用`listen_addresses = '*'`
    更新文件重启 PostgreSQL `sudo systemctl restart postgresql`
    使用`netstat -tnlp | grep 5432`命令验证 Postgres 正在监听`0.0.0.0:5432`。
*   用值`host all all 0.0.0.0/0 md5
    host all all ::/0 md5`
    配置 **pg_hba.conf，**重启 PostgreSQL `sudo systemctl restart postgresql`

现在，我们将能够连接到 PostgreSQL 服务器。

> **如果我的本地 IP 地址改变了怎么办？**

使用 docker bridge 命令并使用`192.168.0.1`作为系统的静态本地 IP。

```
docker network create **-d** bridge **--subnet** 192.168.0.0/24 **--gateway** 192.168.0.1 mynet
```

最后，我们准备好了设置:)