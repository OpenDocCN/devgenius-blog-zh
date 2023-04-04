# 运行 Golang 可执行文件时如何处理“无法执行二进制文件:Exec 格式错误”

> 原文：<https://blog.devgenius.io/how-to-handle-cannot-execute-binary-file-exec-format-error-when-running-golang-executable-b5b77110b820?source=collection_archive---------0----------------------->

![](img/2408608cdac30ab5a057bbdb456a6af4.png)

[Elisa Ventur](https://unsplash.com/@elisa_ventur?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/error?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的原始照片

如果您通过运行`go build`编译了 Golang 二进制文件，然后试图在另一个架构上运行该可执行文件，那么您可能会遇到错误消息`cannot execute binary file: Exec format error`。

例如，如果您在 Mac 电脑上运行`go build main.go`，然后试图在 Linux 虚拟机上运行生成的可执行文件，那么您可能会看到这个错误。

事实证明，如果您可以轻松地重新创建二进制文件，这个修复并不太糟糕。

本质上，您必须编译二进制文件来考虑交叉编译。为此，您可以运行以下命令，例如:

```
env GOOS=linux GOARCH=amd64 go build main.go
```

基本上，您将不得不考虑正确的操作系统和架构，您将最终运行可执行文件。

您通常可能不会指定这些 Golang 环境变量，因为您正在创建可执行文件的同一个地方运行该文件。

如果您想查看这两个字段的所有可能选项，那么您可以运行以下命令来获取列表:

```
$ go tool dist list
```

输出示例如下:

```
go tool dist list
aix/ppc64
android/386
android/amd64
android/arm
android/arm64
darwin/amd64
darwin/arm64
dragonfly/amd64
freebsd/386
freebsd/amd64
freebsd/arm
freebsd/arm64
illumos/amd64
ios/amd64
ios/arm64
js/wasm
linux/386
linux/amd64
linux/arm
linux/arm64
linux/mips
linux/mips64
linux/mips64le
linux/mipsle
linux/ppc64
linux/ppc64le
linux/riscv64
linux/s390x
netbsd/386
netbsd/amd64
netbsd/arm
netbsd/arm64
openbsd/386
openbsd/amd64
openbsd/arm
openbsd/arm64
openbsd/mips64
plan9/386
plan9/amd64
plan9/arm
solaris/amd64
windows/386
windows/amd64
windows/arm
```

选择一个适合您将要运行可执行文件的架构，设置好这些环境变量，您就可以成功了！

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)