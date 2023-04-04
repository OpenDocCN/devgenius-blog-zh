# 如何使用公私密钥对在 docker 容器中安装无密码 OpenSSH 服务器

> 原文：<https://blog.devgenius.io/how-to-install-an-openssh-server-within-a-docker-container-with-a-passwordless-public-private-key-f4f4be9ef9f9?source=collection_archive---------9----------------------->

# 为什么要在 docker 容器中安装 OpenSSH 服务器？

当您编写安装远程服务器或云实例的方法时，您希望在生产环境中运行之前确保它能够正常工作。

在远程实例中测试这种脚本可能需要将其重置为初始状态，这是一项耗时的任务。

通过用本地 docker 容器替换远程服务器，您无需花费任何成本，您可以加速开发过程，并且每次您想要再次运行脚本时，您只需重新启动容器，它就会立即恢复到初始状态。

## 生成公钥-私钥对

使用“ssh-keygen”命令生成公钥和私钥对:

```
ssh-keygen -t rsa -b 4096 -C "[ubuntu@example.com](mailto:ubuntu@example.com)" -f id_rsa -P ""
```

应该生成两个文件 id_rsa 和 id_rsa.pub，它们将在稍后用于 SSH 连接。

## 文档文件

创建一个名为 Dockerfile 的文件，复制/粘贴下面的代码:

```
FROM ubuntu:21.10RUN apt update && apt install openssh-server sudo -yRUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 ubuntuRUN  echo 'ubuntu:ubuntu' | chpasswdRUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> \ /etc/sudoersRUN mkdir /home/ubuntu/.sshCOPY ./id_rsa.pub /home/ubuntu/.ssh/id_rsa.pubRUN cat /home/ubuntu/.ssh/id_rsa.pub >> /home/ubuntu/.ssh/authorized_keysRUN chmod -R go= /home/ubuntu/.sshRUN chown -R ubuntu /home/ubuntu/.sshRUN service ssh startEXPOSE 22CMD ["/usr/sbin/sshd","-D"]
```

这个 docker 文件扩展了官方的 ubuntu 映像，安装了“openssh-server”和“sudo ”,然后创建了一个名为“Ubuntu”的用户，该用户拥有 root 权限，没有密码。

公钥被复制并添加到 authorized_keys 文件中，以便使用私钥通过 SSH 进行连接。

## docker-compose.yml

创建一个名为 docker-compose.yml 的文件，复制/粘贴下面的代码:

```
version: '3'services: ubuntu_01:
        container_name: ubuntu_01
        build:
            context: .
            dockerfile: Dockerfile
        networks:
            - network ubuntu_02:
        container_name: ubuntu_02
        build:
            context: .
            dockerfile: Dockerfile
        networks:
            - network ubuntu_03:
        container_name: ubuntu_03
        build:
            context: .
            dockerfile: Dockerfile
        networks:
            - networknetworks:
    network:
        driver: bridge
```

这个 docker 组合使用相同的先前创建的映像定义 3 个容器，并将它们附加到网络。

## 打开集装箱

让我们运行它们:

```
docker-compose up -d
```

验证一切正常运行:

```
docker ps
```

您应该会看到这样的内容:

```
CONTAINER ID   IMAGE                  COMMAND               CREATED             STATUS             PORTS     NAMES
4d1b8091014e   ubuntu-ssh_ubuntu_03   "/usr/sbin/sshd -D"   About an hour ago   Up About an hour   22/tcp    ubuntu_03
506d1af29f25   ubuntu-ssh_ubuntu_02   "/usr/sbin/sshd -D"   About an hour ago   Up About an hour   22/tcp    ubuntu_02
76c85e4fe2ec   ubuntu-ssh_ubuntu_01   "/usr/sbin/sshd -D"   About an hour ago   Up About an hour   22/tcp    ubuntu_01
```

## 获取“ubuntu_01”容器 IP

执行下面的命令，根据名称获取容器 IP:

```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubuntu_01
```

输出应该类似于:

```
172.25.0.4
```

## 通过 SSH 连接到“ubuntu_01”容器

一切都已启动并运行，所以让我们使用“ubuntu”用户和私钥连接到容器“ubuntu_01 ”:

```
ssh ubuntu@172.25.0.4 -i id_rsa
```

对于第一次连接，它会要求您确认主机，只需键入“是”并按“回车”即可。

通过添加选项"-o StrictHostKeyChecking=no "来禁用主机检查，并使用" docker inspect …"来获取 IP 作为变量，这将使您可以使用一条线路进行连接，而无需密码或主机检查。

```
ssh -o StrictHostKeyChecking=no ubuntu@$(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubuntu_01) -i id_rsa
```

我希望这将是有用的，我愿意接受任何反馈来改进这篇文章。

感谢您的阅读:)

欢迎**关注**了解更多内容🔔，**拍拍**👏🏻**与你喜欢的任何人分享文章**。

一如既往，我感谢你的支持。