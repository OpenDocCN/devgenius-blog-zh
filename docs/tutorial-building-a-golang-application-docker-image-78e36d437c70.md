# 教程:构建 Golang 应用程序 Docker 映像

> 原文：<https://blog.devgenius.io/tutorial-building-a-golang-application-docker-image-78e36d437c70?source=collection_archive---------1----------------------->

Golang 应用程序的 Docker 图像

![](img/c94baec860bf73658836c065c9dde629.png)

杰斯·贝利在 [Unsplash](https://unsplash.com/s/photos/desk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在我们经过几个月的努力写出了应用之后，如何部署呢？我们用一个简单的`Hello World`的例子来学习一下。

项目结构如下:

```
.├── go.mod└── hello.go
```

`hello.go`的代码内容如下:

```
package mainfunc main() {
    println("hello world!")
}
```

为了跟上潮流，我们这里选择使用 Docker 部署。

**初次尝试。**

为了方便起见，我们准备将所有内容放入 Docker 进行编译，经过一番研究，我们得到如下`Dockerfile`文件:

```
FROM golang:alpineWORKDIR /buildCOPY hello.go .RUN go build -o hello hello.goCMD ["./hello"]
```

下一步开始建设。

```
$ docker build -t hello:v1 .$ docker run -it --rm hello:v1 ls -l /buildtotal 1260-rwxr-xr-x    1 root     root       1281547 Mar  6 15:54 hello-rw-r--r--    1 root     root            55 Mar  6 14:59 hello.go *#* try to run it$ docker run -it --rm hello:v1hello world!
```

它运行成功，然后我们看看图像的大小。

```
$ docker images | grep hellohello         v1    2783ee221014   44 minutes ago   314MB
```

震惊了我，整个图像居然有 **314MB** ，只是`docker build`，怎么回事？

虽然可以运行，但是这个图像的大小太吓人了，我们只是简单的打印了一行`hello world`，图像大小超过 **300 MB** ，太不合理，需要优化。

**第二次尝试。**

找资料后发现我们用的基图太大了。

```
$ docker images | grep golanggolang    alpine     d026981a7165   2 days ago          313MB
```

有朋友告诉我可以先把代码编译好，然后再复制进去，这样就不需要那个巨大的基础镜像了，不过说起来容易，我还是花了一些时间学习，最后的`Dockerfile`是这样的:

```
FROM alpineWORKDIR /buildCOPY hello .CMD ["./hello"]
```

让我们重建图像:

```
$ docker build -t hello:v2 ....=> ERROR [3/3] COPY hello .                         0.0s------> [3/3] COPY hello .:------failed to compute cache key: "/hello" not found: not found
```

哦呼，错误的报告。找不到提示`hello`，忘记先编译`hello.go`再执行。

```
$ go build -o hello hello.go$ docker run -it --rm hello:v2standard_init_linux.go:228: exec user process caused: exec format error
```

呜呜，又失败了。

嗯，格式不对，原来我们的开发机不是`linux`，不要放弃，再来一次。

```
$ GOOS=linux go build -o hello hello.go$ docker build -t hello:v2 .*# ...*Successfully
```

最后，构建成功，让我们尝试一下。

```
$ docker run -it --rm hello:v2hello world!
```

没问题，我们来看看内容和大小。

```
$ docker run -it --rm hello:v2 ls -l /buildtotal 1252-rwxr-xr-x    1 root     root       1281587 Mar  6 16:18 hello $ docker images | grep hellohello    v2   0dd53f016c93   53 seconds ago      6.61MBhello    v1   ac0e37173b85   25 minutes ago      314MB
```

哇，这次才 **6.61MB** ，还行！

**第三次尝试。**

虽然上面的图像可以成功构建，但还是有一些不足。它不是一个多阶段的构建。

我们需要能够从`Go`代码构建一个`docker`图像，这分为三个步骤:

*   原生编译`Go`代码，如果涉及到`cgo`跨平台编译会比较麻烦。
*   用编译后的可执行文件构建一个`docker`映像。
*   编写一个`shell`脚本或`makefile`，在一个命令中完成这些步骤。

多阶段构建就是将所有内容都放在一个`Dockerfile`中，没有源代码泄漏，没有用于跨平台编译的脚本，以及最小的映像。

热爱学习，追求完美，最后写了下面的`Dockerfile`。

```
FROM golang:alpine AS builderWORKDIR /buildADD go.mod .COPY . .RUN go build -o hello hello.go FROM alpineWORKDIR /buildCOPY --from=builder /build/hello /build/helloCMD ["./hello"]
```

第一个`FROM`从构建一个`builder`映像开始，可执行文件`hello`在这个映像中被编译。

从第二个`FROM`开始的部分是从第一个图像开始`copy`可执行文件`hello`，并使用尽可能小的基础图像`alpine`以确保最终图像尽可能小。

至于为什么不用小一点的`scratch`，那是因为`scratch`里真的什么都没有，有问题也没机会看一眼，而且`alpine`也才 **5MB** ，对我们服务好不会有太大影响。

让我们先运行它来验证:

```
$ docker run -it --rm hello:v3hello world!
```

没问题，意料之中！看看大小是什么样的:

```
$ docker images | grep hellohello    v3     f51e1116be11   8 hours ago    6.61MBhello    v2     0dd53f016c93   8 hours ago    6.61MBhello    v1     ac0e37173b85   8 hours ago    314MB
```

第二种方法构建的图像大小完全相同。看看镜子里的内容:

```
$ docker run -it --rm hello:v3 ls -l /buildtotal 1252-rwxr-xr-x    1 root     root       1281547 Mar  6 16:32 hello
```

此外，只有一个可执行文件`hello`可以完美构建！

感谢您阅读这篇文章。如果你在这篇文章中发现任何错误，请告诉我。