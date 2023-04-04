# 如何在 Kubernetes 集群上使用 MySQL 部署 Rest API 应用程序

> 原文：<https://blog.devgenius.io/how-to-deploy-rest-api-application-using-mysql-on-the-kubernetes-cluster-4c806de1a48?source=collection_archive---------1----------------------->

![](img/661e2ab2fa01c6b670ef48b007453910.png)

安特·汉默斯米特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

让我们将 Kubernetes twist 添加到一个经典的软件任务中——使用 MySQL 的 API

软件开发中的经典任务——用 MySQL 数据库编写一个 API 应用程序用于数据存储。可以找到大量关于它的出版物，我们不打算专注于这个特定的任务。

加上一个小小的转折——它变成了一个具有挑战性和教育性的任务。这个小变化就是 Kubernetes 平台上的虚拟化。
让我们回顾一下如何在 Kubernetes 集群上使用 MySQL 部署 Rest API 应用程序的分步教程。重点将放在部署方面，应用程序代码仅用于提供正确工作概念的证据。

此部署中有 3 个组件:

*   库伯内特星团
*   运行在 Kubernetes 集群上的 MySQL
*   运行在 Kubernetes 集群上的简单 Rest API 应用程序，能够使用运行在同一集群上的 MySQL

> **点击关注，不再错过我的文章。**

## **先做第一件事——Kubernetes 集群**

K3s 是一款轻量级的 Kubernetes。

k3d 是一个轻量级的包装器，可以在 docker 中运行 [k3s](https://github.com/rancher/k3s) (Rancher Lab 的最小 Kubernetes 发行版)。

还有什么比这更能提供最好的开发体验呢？这意味着能够在您的笔记本电脑或虚拟机上本地运行所有组件，以避免在运行应用程序时出现任何依赖性。

安装 Kubernetes 集群的步骤

```
$ k3d cluster create --api-port 6550 -p "8081:80@loadbalancer"$ export KUBECONFIG="$(k3d kubeconfig write k3s-default)"$ k3d cluster list
```

简单优雅—只需一步，我们就可以运行 Kubernetes 集群。

如果你从未使用过 k3d，你至少应该尝试用它来探索一个用最少的资源在本地运行集群的新世界。

如果您的 KUBECONFIG 的定义不是持久的，并且您在终端重新启动后丢失了它，而您的 K3d 集群仍在运行，请使用以下命令指向正确的集群。

```
$ kubectl config get-contexts$ kubectl config use-context <your cluster name>
```

> **一定要点击关注，千万不要错过另一篇关于技巧和提示、生活经验等的文章！**

从这个 GitHub 库中克隆本教程的代码

[](https://github.com/LearnTechnoBios/rest-api-mysql-k3d) [## GitHub-LearnTechnoBios/rest-API-MySQL-k3d

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/LearnTechnoBios/rest-api-mysql-k3d) 

## **运行在 Kubernetes 集群上的 MySQL】**

我们将使用 Kubernetes secret 来存储 MySQL 数据库密码
以获得`base64`编码的密码运行

```
$ echo -n 'admin' | base64
```

使用 mysql-secret.yaml 中的输出

```
apiVersion: v1kind: Secretmetadata:name: mysql-passtype: Opaquedata:password: YWRtaW4=
```

创建一个秘密运行

```
kubectl create -f mysql-secret.yaml
```

验证创建

```
kubectl get secrets
```

使用 mysql-pod.yaml 在集群上部署 mysql pod

```
apiVersion: v1kind: Podmetadata:name: k8s-mysqllabels:name: lbl-k8s-mysqlspec:containers:- name: mysqlimage: mysql:latestenv:- name: MYSQL_ROOT_PASSWORDvalueFrom:secretKeyRef:name: mysql-passkey: passwordports:- name: mysqlcontainerPort: 3306protocol: TCPvolumeMounts:- name: k8s-mysql-storagemountPath: /var/lib/mysqlvolumes:- name: k8s-mysql-storageemptyDir: {}
```

创建运行

```
kubectl create -f mysql-pod.yaml
```

验证创建

```
kubectl get pod
```

等待 pod 进入运行状态，这可能需要几分钟时间。

现在我们有一个在集群
上运行 MySQL 的 pod，让我们连接到 MySQL pod 并尝试使用它

```
$ kubectl exec k8s-mysql -it -- bash
echo $MYSQL_ROOT_PASSWORD
mysql --user=root --password=$MYSQL_ROOT_PASSWORD
show databases;
```

从 Pod 上使用时，它工作正常。到目前为止，无法从另一个 pod 访问 MySQL pod。

为了允许来自其他 pod 和外部系统的访问，我们应该暴露一个端口。我们将为它使用一个服务。

使用 mysql-service.yaml

```
apiVersion: v1kind: Servicemetadata:name: mysql-servicelabels:name: lbl-k8s-mysqlspec:ports:- port: 3306selector:name: lbl-k8s-mysqltype: ClusterIP
```

创建管路

```
kubectl create -f mysql-service.yaml
```

为了验证创作

```
kubectl get svc
```

## **部署 API 应用并使用 MySQL**

对于非常简单的应用程序，请使用 app.py

```
#!/usr/bin/python
 import os
 from flask import Flask
 from peewee import MySQLDatabase, IntegerField

 MYSQL_ROOT_USER = os.getenv('MYSQL_ROOT_USER', 'root')
 MYSQL_ROOT_PASSWORD = os.getenv('MYSQL_ROOT_PASSWORD', 'admin')
 MYSQL_ROOT_HOST = os.getenv('MYSQL_ROOT_HOST', 'localhost')
 MYSQL_ROOT_PORT = os.getenv('MYSQL_ROOT_PORT', '3306')
 MYSQL_ROOT_DB = os.getenv('MYSQL_ROOT_DB', 'mydb')
 FLASK_APP_PORT = os.getenv('FLASK_APP_PORT', '8282')

 db = MySQLDatabase(database=MYSQL_ROOT_DB, user=MYSQL_ROOT_USER, password=MYSQL_ROOT_PASSWORD,
                    host=MYSQL_ROOT_HOST, port=int(MYSQL_ROOT_PORT))

 app = Flask(__name__)
 app.run(host='0.0.0.0', port=int(FLASK_APP_PORT))

 @app.route('/')
 def get_db_version():
     cursor = db.cursor()
     cursor.execute("SELECT VERSION()")
     data = cursor.fetchone()
     db.close()
     return "Database version : %s " % data
```

添加 requirements.txt 文件

```
Flask

pymysql

peewee

cryptography
```

添加 Dockerfile

```
FROM python:3

WORKDIR /project

COPY . .

RUN pip install -r requirements.txt

CMD ["flask", "run", "--host=0.0.0.0", "--port=8282" ]
```

构建图像

```
docker build -t <image-name>:<image-tag> .
```

推送 docker 映像(在运行推送命令之前，不要忘记登录 docker hub)

```
docker push   <image-name>:<image-tag>
```

> **点击关注，不再错过我的文章。**

使用 config-map.yaml 定义将在 API 应用程序中使用的环境变量的值

您应该使用以下命令的输出为 MYSQL_ROOT_HOST
添加正确的值

```
kubectl get pod k8s-mysql -o template --template={{.status.podIP}}
```

配置图. yaml

```
apiVersion: v1

kind: ConfigMap

metadata:

  name: app-config

data:

  FLASK_APP: app.py

  MYSQL_ROOT_USER: root

  MYSQL_ROOT_PASSWORD: admin

  MYSQL_ROOT_HOST: "<plase holder>"

  MYSQL_ROOT_PORT: "3306"

  MYSQL_ROOT_DB: mydb

  FLASK_APP_PORT: "8282"
```

kubectl apply -f config-map.yaml

要部署应用程序，请使用 we b-application-deployment . YAML 添加您的 image-name:image-tag——docker build 命令中使用的值

```
---

apiVersion: apps/v1

kind: Deployment

metadata:

  labels:

    app: web-application 

  name: web-application 

spec:

  replicas: 1

  strategy:

    type: Recreate

  selector:

    matchLabels:

      app: web-application

  template:

    metadata:

      labels:

        app: web-application

    spec:

      containers:

      - image: <image-name:image-tag > 

        name: web-application

        imagePullPolicy: Always        

        resources:

          limits:

            cpu: "0.3"

            memory: "512Mi"

          requests:

            cpu: "0.3"

            memory: "256Mi"

        env:

          - name: FLASK_APP

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: FLASK_APP

          - name: MYSQL_ROOT_USER

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: MYSQL_ROOT_USER

          - name: MYSQL_ROOT_PASSWORD

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: MYSQL_ROOT_PASSWORD

          - name: MYSQL_ROOT_HOST

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: MYSQL_ROOT_HOST

          - name: MYSQL_ROOT_PORT

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: MYSQL_ROOT_PORT

          - name: MYSQL_ROOT_DB

            valueFrom:

              configMapKeyRef:

                name: app-config

                key: MYSQL_ROOT_DB
```

> 点击关注，永远不要错过我的另一篇文章。

创建部署

```
kubectl apply -f web-application-deployment.yaml
```

验证创建

```
kubectl get deployment
```

要为此部署运行创建服务

```
kubectl create service clusterip web-application — tcp=80:8282
```

使用 web-application-ingress.yaml 为此应用程序创建入口规则

```
apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:

  name: web-application

  annotations:

    ingress.kubernetes.io/ssl-redirect: "false"

spec:

  rules:

  - http:

      paths:

      - path: /

        pathType: Prefix

        backend:

          service:

            name: web-application

            port:

              number: 80
```

创造

```
kubectl apply -f web-application-ingress.yaml
```

验证所有安装程序是否运行

```
curl localhost:8081/
```

您应该会收到以下输出(版本可能不同)

```
Database version: 8.0.28 
```

我希望这篇循序渐进的工作教程能够帮助您在 Kubernetes 上进行部署。

> **一定要点击关注，千万不要错过另一篇关于技巧和提示、生活经验等的文章！**

## 摘要

我们学习如何使用 MySQL
将简单的 API 应用程序部署到 Kubernetes 集群。我们使用一个 [k3d](https://k3d.io/) 包装器在 docker 中运行 [k3s](https://github.com/rancher/k3s) (Rancher Lab 的最小 Kubernetes 发行版)。
我们审查了 Kubernetes 部署配置，以允许对 API 应用程序的外部请求

编码快乐！！！