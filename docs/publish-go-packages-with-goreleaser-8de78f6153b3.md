# 用 Goreleaser 发布 Go 包

> 原文：<https://blog.devgenius.io/publish-go-packages-with-goreleaser-8de78f6153b3?source=collection_archive---------6----------------------->

![](img/2d99c51fc55f5c0b03ee07f923589aec.png)

照片由 [Chinmay Bhattar](https://unsplash.com/@geekgunda?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

[Goreleaser](https://goreleaser.com/) 牛逼。这是一个简单的工具，允许你发布你的 *go* 包。最近，我和我的团队将它与我们构建的公司范围的 CLI 工具一起使用。

在本教程中，我们将使用 *goreleaser* 来自动发布一个简单的`go`包。

# 装置

在 macOS 上，要安装 *goreleaser* ，我们可以使用:

```
go install github.com/goreleaser/goreleaser@latest
```

或者我们使用流行的 [*家酿*](https://brew.sh/) 包管理器为 macOS 和 Linux 提供:

```
brew install goreleaser/tap/goreleaser
```

在 Ubuntu Linux 上，我们可以使用`apt`:

```
echo 'deb [trusted=yes] https://repo.goreleaser.com/apt/ /' | sudo tee /etc/apt/sources.list.d/goreleaser.list
sudo apt update
sudo apt install goreleaser
```

更多选项可以在这里找到[。](https://goreleaser.com/install/)

# 创建包

创建一个新文件夹来存放我们的项目。然后用初始化模块

```
go mod init main
```

接下来，我们创建一个名为`main.go`的文件。在`main.go`中，复制并粘贴以下代码:

```
package mainfunc main() {
  println("This is a tutorial about Goreleaser!")
}
```

# 初始化 Goreleaser

下一步是设置*或释放器*。为此，我们运行:

```
goreleaser init
```

这个命令在我们的目录下创建`.goreleaser.yaml`文件。我们将仔细看看这个文件。

# 更新`.goreleaser.yaml`

我已经在生成的`.goreleaser.yaml`文件中添加了一些字段。我们将讨论重要的部分。

```
release:
  github:
    owner: justdamilare
    name: mytoolbefore:
  hooks:
    - go mod tidy
    - go generate ./...builds:
  - env:
      - GO_VERSION=1.19
    goos:
      - linux
      - windows
      - darwin# replaces architecture naming in the archive name
archives:
  - replacements:
      darwin: Darwin
      linux: Linux
      windows: Windows
      386: i386
      amd64: x86_64 format_overrides:
      - goos: windows
        format: zipchecksum:
  name_template: 'checksums.txt'snapshot:
  name_template: "{{ incpatch .Version }}-next"changelog:
  sort: asc
  filters:
    exclude:
      - '^docs:'
      - '^test:'# upload binaries to gcloud bucket called `mytool`
blobs:
  - provider: gs
    bucket: mytool
    folder: "{{ .Version }}"# generate homebrew-tap  
brews:
  - name: justdamilare
    tap:
      owner: mytool
      name: homebrew-tap
    folder: Formula
    homepage: https://github.com/justdamilare/mytool
    description: A simple go project
    # use custom download strategy in case the Github repository is private
    download_strategy: GitHubPrivateRepositoryReleaseDownloadStrategy
    custom_require: "../custom_download_strategy"
    test: |
      system "#{bin}/mytool"
    install: |
      bin.install "mytool"
```

需要注意的是，如果不需要`homebrew-tap`和`blobs`，可以移除这些部分。如果需要`homebrew-tap`，还应该创建一个名为`homebrew-tap`的 Githib 回购。

# 发布包

最后，我们可以发布我们的包了。为此，我们需要在 Git 上为我们的发布创建一个标签。例如，要为版本`0.1.0`创建一个标签，我们可以运行:

```
git tag v0.1.0
```

和

```
git push origin v0.1.0
```

然后在带有`main.go`的目录中，运行:

```
goreleaser release
```

Goreleaser 将构建所有的二进制文件。这些二进制文件将使用本地 GitHub 凭证自动上传到 Github。构建也将位于主目录的`/dist`文件夹中。如果包含了`brew`发布方法，生成的`*.rb`文件也会在`/dist`文件夹中。如果 Goreleaser 没有自动将生成的`Formula`复制到`homebrew-tap` repo 中，可以手动复制。你可以查看如何将构建发布到私有自制软件——点击[这里](/create-homebrew-taps-for-private-github-repos-44daf2f4cff8)

# 摘要

*   安装 goreleaser
*   创建 *go* 包
*   用`goreleaser init`初始化 Goreleaser
*   更新`.goreleaser.yaml`
*   通过创建一个带有`git tag vX.X.X`的标签来发布构建，然后运行`goreleaser release`