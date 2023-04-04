# 在荚果形成之后和荚果死亡之前的内务处理任务

> 原文：<https://blog.devgenius.io/housekeeping-task-post-pod-formation-and-before-a-pod-dies-ce8ec2b6423f?source=collection_archive---------3----------------------->

![](img/a24d502a530c64660a531f1570aff185.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果你想在谷歌云平台(GCP)上，在 POD 创建后或 POD 死亡前做一些整理/清理工作，那么这篇文章就是为你准备的。

你会发现很多关于什么是豆荚、容器以及如何用库伯奈特人建造一个帝国的理论，我不会深入讨论这些细节，这里的[](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)**是对库伯奈特人的快速参考。我会尽量把重点放在如何调用一个程序上，一旦荚果形成，荚果即将死亡，……简单:)。**

**我们将尝试使用 postStart 和 preStop 容器处理程序。我们将首先看到 **postStart** ，因为我们将能够查看容器并看到输出。然后，我们将尝试使用 **preStop** 事件删除 Google Cloud 发布/订阅。此处 参照 [**了解容器生命周期事件。**](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/)**

**所以让我们开始旅程吧…**

```
Step 1- Have your environment readyStep 2- Using "postStart" eventStep 3- Using "preStop" event
```

****第一步** - **准备好你的环境**。尝试获得或构建一个 kubernetes 环境(集群),该环境由一个包含 NodeJS 的容器组成。在这个容器中，我希望“/usr”文件夹下有一个 demo.js 文件。创建这样的容器有不同的方法。我自己创造了一个新的码头工人形象。接下来的步骤将创建您自己的图像，该图像将包含“/usr”下的 demo.js 文件**

```
1\. docker pull node2\. docker run -it — name=”hellonode_test” node:latest /bin/bash3\. Goto "/usr" folder. Create a file demo.js and put following javascript code.***function testimony() {console.log(‘This is function only to prove my point.’); } testimony()***Easy way to do this is following

echo "***function testimony() {console.log(‘This is function only to prove my point.’); } testimony()***" > demo.js4\. docker commit -m=”This a test hellonode image” hellonode_test hellonodejs5\. docker tag hellonodejs gcr.io/<your project id>/hellonodejs6\. docker push gcr.io/<your project id>/hellonodejs
```

****步骤 2** - **使用“postStart”事件**-【Kubernetes 在容器创建后立即发送 postStart 事件】**。我将在这里尝试演示 postStart 事件的用法。****

**在第一步中，我们已经准备好部署映像。让我们继续，并使用下面的最小配置来部署它。yaml "配置如下所示。**

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
 name: my-pod
spec:
 replicas: 1
 selector:
 matchLabels:
 app: testhellonode
 template:
 metadata:
 labels:
 app: testhellonode
 spec:
 containers:
 — name: my-container
 image: gcr.io/<your project id>/hellonodejs
 imagePullPolicy: Always
 command: [“/bin/sh”, “-c”, “while :; do sleep 1; done”]
 lifecycle:
 postStart:
 exec:
 command: [“/bin/sh”, “-c”, “node ./usr/demo.js > /usr/share/message”]
```

> **命令推上面的配置- ***kubectl apply -f " <路径到 yaml > "*****

**一旦部署成功，您可以使用类似于 **kubectl get deployments** 和/或 **kubectl get pods 这样的命令来验证。**尝试获取 POD 的名称(POD_Name)。现在尝试运行以下命令**

```
**kubectl exec -it <POD_Name> — /bin/bash**cat /usr/share/message
```

**您应该会看到一条消息“这只是用来证明我的观点的函数。”已显示。**

**此消息是作为 postStart 处理程序的结果生成的，在该处理程序中，我们尝试运行 demo.js 并将其传输到“/usr/share/message”。这证明了我们应该能够调用任何编程逻辑来执行某些任务。在我的例子中，我成功地调用了 javascript。**

****第三步** - **使用“preStop”事件**。要查看“preStop”事件的证据，我们将无法使用日志，因为 POD 将被销毁，我们无法登录 POD 来查看日志。因此，要看到这个事件正在工作，我们必须从外部改变一些状态，我们可以去那里验证。我们可以使用很多方法，比如将一个条目写入持久存储(数据库)等等。我将尝试删除现有的云发布/订阅。**

**先决条件-尝试在主题“hellotopic”上手动创建一个名为“hellosub”的云发布/订阅。参考 [**此处**](https://cloud.google.com/pubsub/docs/quickstart-console) 关于如何创建订阅。**

**当“preStop”事件发生时，我们将使用下面的 javascript 代码删除订阅。我将把这段代码放在一个名为“delsub.js”的文件中。**

```
async function deleteSubscription() {const subscriptionName = ‘hellosub’;const { PubSub } = require(‘@google-cloud/pubsub’);const pubsub = new PubSub();await pubsub.subscription(subscriptionName).delete();}deleteSubscription()
```

**同样，为了让上面的代码工作，我们必须有一个 kubernetes 环境(集群)，由一个带有单个容器的 POD 组成，具有 NodeJS 和所需的节点模块(“@google-cloud/pubsub”)。我们可以创建一个包含所有上述所需组件的新映像，并尝试将文件 delsub.js 放在“/usr”文件夹下。参考[此处](https://cloud.google.com/pubsub/docs/reference/libraries#client-libraries-install-nodejs)的云发布/订阅客户端。**

**我将尝试在前面的 yaml 文件中的 PostStart 旁边添加以下预停止条目**

```
 **preStop:
            exec:
              command: ["/bin/sh", "-c", "node ./usr/delsub.js"]**
```

**因此，yaml 文件现在将如下所示**

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
 name: my-pod
spec:
 replicas: 1
 selector:
 matchLabels:
 app: testhellonode
 template:
 metadata:
 labels:
 app: testhellonode
 spec:
 containers:
 — name: my-container
 image: gcr.io/<your project id>/hellonodejs
 imagePullPolicy: Always
 command: [“/bin/sh”, “-c”, “while :; do sleep 1; done”]
 lifecycle:
 postStart:
 exec:
 command: [“/bin/sh”, “-c”, “node ./usr/demo.js > /usr/share/message”]
 **preStop:
 exec:
 command: ["/bin/sh", "-c", "node ./usr/delsub.js"]**
```

**使用上述 yaml 文件进行重新部署。**

**preStop 事件仅在 POD 死亡时触发，因此要重现/触发 preStop 事件，我们必须杀死 POD，有不同的方法可以做到这一点，但缩小 POD 大小是最简单的方法。因此，我将把 POD 大小从 1 缩小到 0(零)。一旦我们将大小缩小到 0，就会触发 preStop 事件，这将删除“hellosub”订阅。我们可以通过查看订阅列表来验证这一点。**

****总结**:在这篇文章中，我们看到了如何从容器生命周期事件“postStart”和“preStop”中调用 javascript 代码。这比调用简单的脚本前进了一小步。类似地，我们可以定义一个处理程序来调用不同编程语言的代码或脚本，我将把它留给您做进一步的探索。总之，这些事件可用于在荚果形成后或荚果死亡前完成一些内务/重要任务。**