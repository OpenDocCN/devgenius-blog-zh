# GoReleaser 构建多拱门码头图像

> 原文：<https://blog.devgenius.io/goreleaser-build-multi-arch-docker-images-8dd9a7903675?source=collection_archive---------6----------------------->

![](img/b3489c0b824001c5f428d2688a67aca2.png)

照片由[威廉·威廉](https://unsplash.com/@william07?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/container-terminal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

> *GoReleaser 是一款面向 Go 项目的发布自动化工具。目标是简化构建、发布和发布步骤，同时为所有步骤提供变体定制选项。GoReleaser 是基于 CI 工具构建的，您只需下载并在构建脚本中执行它。当然，如果你愿意，你仍然可以在本地安装它。
> 您的发布流程可以通过. goreleaser.yaml 文件进行定制。
> 一旦你设置好了，每次你想创建一个新的版本，你需要做的就是标记并运行 go release release*

我最近了解了 goreleaser，并立即喜欢上了这个工具。它易于使用，极大地简化了部署过程。

**前置要求**

1.  [GoReleaser](https://goreleaser.com/install/) (在 1.5.0 版本中测试过)
2.  [Docker Buildx](https://docs.docker.com/buildx/working-with-buildx/) 已安装

在一些操作系统中，不包括 Docker 的 Buildx，我们必须随后安装它，你可以用命令`docker buildx`测试它。

**创建一个 Git 存储库**

首先，我们将创建一个 Git 存储库。这样我们就可以使用 [Git 标签](https://git-scm.com/book/en/v2/Git-Basics-Tagging)，goreleaser 使用这些标签来创建容器图像标签。

```
mkdir goreleaser-test
cd goreleaser-test
git init
```

**创建一个 Dockerfile**

现在我们有了一个工作 git 环境，我们将在我们的工作环境中为我们的构建创建一个最小的 Dockerfile。我们(goreleaser)将把构建二进制文件复制到容器中，当容器启动时,`ENTRYPOINT` 将运行我们的二进制文件。

```
FROM alpine:3.15.0# copy over the binary from the first stage
COPY helloworld /helloworld/helloworldWORKDIR “/helloworld”
ENTRYPOINT [ “/helloworld/helloworld” ]
```

**创造一些围棋**

我们将创建一个最小的 Go 二进制文件，输出 Hello World、操作系统和处理器架构。所以在`./helloworld/helloworld.go`创建文件

```
package mainimport (
        "fmt"
        "runtime"
)func main() {
        fmt.Println("Hello World")
        fmt.Println(runtime.GOOS, runtime.GOARCH)
}
```

**创建. goreleaser.yaml**

下一步是准备我们的`.goreleaser.yaml`发布。我们把它放在主路。

```
# [https://goreleaser.com](https://goreleaser.com)
project_name: helloworld
before:
  # [https://goreleaser.com/customization/hooks/](https://goreleaser.com/customization/hooks/)
  hooks:
    # tidy up
    - go mod tidy
builds:
  # [https://goreleaser.com/customization/build/](https://goreleaser.com/customization/build/)
  - main: ./helloworld
    binary: helloworld
    env:
      - CGO_ENABLED=0
    goos:
      - linux
    goarch:
      - amd64
      - arm
      - arm64
    goarm:
      - 6
      - 7
    mod_timestamp: "{{ .CommitTimestamp }}"
dockers:
  # [https://goreleaser.com/customization/docker/](https://goreleaser.com/customization/docker/)
  - use: buildx
    goos: linux
    goarch: amd64
    image_templates:
      - "0hlov3s/{{ .ProjectName }}:{{ .Version }}-amd64"
      - "0hlov3s/{{ .ProjectName }}:latest-amd64"
    build_flag_templates:
      - "--platform=linux/amd64"
      - "--label=org.opencontainers.image.created={{.Date}}"
      - "--label=org.opencontainers.image.title={{.ProjectName}}"
      - "--label=org.opencontainers.image.revision={{.FullCommit}}"
      - "--label=org.opencontainers.image.version={{.Version}}"
  - use: buildx
    goos: linux
    goarch: arm64
    image_templates:
      - "0hlov3s/{{ .ProjectName }}:{{ .Version }}-arm64v8"
      - "0hlov3s/{{ .ProjectName }}:latest-arm64v8"
    build_flag_templates:
      - "--platform=linux/arm64/v8"
      - "--label=org.opencontainers.image.created={{.Date}}"
      - "--label=org.opencontainers.image.title={{.ProjectName}}"
      - "--label=org.opencontainers.image.revision={{.FullCommit}}"
      - "--label=org.opencontainers.image.version={{.Version}}"
  - use: buildx
    goos: linux
    goarch: arm
    goarm: 6
    image_templates:
      - "0hlov3s/{{ .ProjectName }}:{{ .Version }}-armv6"
      - "0hlov3s/{{ .ProjectName }}:latest-armv6"
    build_flag_templates:
      - "--platform=linux/arm/v6"
      - "--label=org.opencontainers.image.created={{.Date}}"
      - "--label=org.opencontainers.image.title={{.ProjectName}}"
      - "--label=org.opencontainers.image.revision={{.FullCommit}}"
      - "--label=org.opencontainers.image.version={{.Version}}"
  - use: buildx
    goos: linux
    goarch: arm
    goarm: 7
    image_templates:
      - "0hlov3s/{{ .ProjectName }}:{{ .Version }}-armv7"
      - "0hlov3s/{{ .ProjectName }}:latest-armv7"
    build_flag_templates:
      - "--platform=linux/arm/v7"
      - "--label=org.opencontainers.image.created={{.Date}}"
      - "--label=org.opencontainers.image.title={{.ProjectName}}"
      - "--label=org.opencontainers.image.revision={{.FullCommit}}"
      - "--label=org.opencontainers.image.version={{.Version}}"
docker_manifests:
  # [https://goreleaser.com/customization/docker_manifest/](https://goreleaser.com/customization/docker_manifest/)
  - name_template: 0hlov3s/{{ .ProjectName }}:{{ .Version }}
    image_templates:
      - 0hlov3s/{{ .ProjectName }}:{{ .Version }}-amd64
      - 0hlov3s/{{ .ProjectName }}:{{ .Version }}-arm64v8
      - 0hlov3s/{{ .ProjectName }}:{{ .Version }}-armv6
      - 0hlov3s/{{ .ProjectName }}:{{ .Version }}-armv7
  - name_template: 0hlov3s/{{ .ProjectName }}:latest
    image_templates:
      - 0hlov3s/{{ .ProjectName }}:latest-amd64
      - 0hlov3s/{{ .ProjectName }}:latest-arm64v8
      - 0hlov3s/{{ .ProjectName }}:latest-armv6
      - 0hlov3s/{{ .ProjectName }}:latest-armv7
checksum:
  # [https://goreleaser.com/customization/checksum/](https://goreleaser.com/customization/checksum/)
  name_template: 'checksums.txt'
snapshot:
  # [https://goreleaser.com/customization/snapshots/](https://goreleaser.com/customization/snapshots/)
  name_template: "{{ incpatch .Version }}-SNAPSHOT"
source:
  # [https://goreleaser.com/customization/source/](https://goreleaser.com/customization/source/)
  enabled: true
```

## git 提交和标记

```
git add .
git commit -m ‘Initial Commit’
git tag v0.0.1
```

**运行测试版本**

既然一切都准备好了，我们就可以进行测试发布了。因此，当我们使用`--snapshot`运行 goreleaser 时，我们将在本地构建容器图像，而 goreleaser 不会将它们推送到注册表。

```
goreleaser release — rm-dist — snapshot
```

**查看您的集装箱图片**

不，我们成功地创建了容器图像，我们可以在我们当地的 Maschine 上测试它们。所以我们要检查哪些图像是构建的。

```
❯ docker images
REPOSITORY                        TAG                      IMAGE ID       CREATED        SIZE
0hlov3s/helloworld                0.0.2-SNAPSHOT-armv6     a613a22f467a   35 hours ago   6.04MB
0hlov3s/helloworld                latest-armv6             a613a22f467a   35 hours ago   6.04MB
0hlov3s/helloworld                0.0.2-SNAPSHOT-amd64     7058d357c075   35 hours ago   6.79MB
0hlov3s/helloworld                latest-amd64             7058d357c075   35 hours ago   6.79MB
0hlov3s/helloworld                0.0.2-SNAPSHOT-armv7     d8e9a147bdd4   35 hours ago   5.06MB
0hlov3s/helloworld                latest-armv7             d8e9a147bdd4   35 hours ago   5.06MB
0hlov3s/helloworld                0.0.2-SNAPSHOT-arm64v8   1bf6fe199d86   35 hours ago   6.64MB
0hlov3s/helloworld                latest-arm64v8           1bf6fe199d86   35 hours ago   6.64MB
```

既然我们知道了，我们有什么图像，我们就可以用`docker run`命令运行它们。

```
❯ docker run 0hlov3s/helloworld:0.0.2-SNAPSHOT-amd64
Hello World
```

**有用链接**

*   [git hub Repo 示例](https://github.com/0hlov3/goreleaser-multi-arch-docker)
*   [码头工人图像-gore leaser](https://goreleaser.com/customization/docker/)

**不要相信我**

作者对信息中包含的错误或因使用信息而产生的损害不承担责任。

不要犹豫，向我提问或向我发送改进建议。

#go #go/goreleaser