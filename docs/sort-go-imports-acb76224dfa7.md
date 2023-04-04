# 分类导入

> 原文：<https://blog.devgenius.io/sort-go-imports-acb76224dfa7?source=collection_archive---------9----------------------->

Go imports 通过强制执行至少两组 stdlib 和其他所有东西来实现这一点。然而，我的团队有一个将导入分成 4 组的策略。

```
import (
    stdlib
    github or other packages url
    all others directory
    current directory
)
```

这也是解决 go 导入的方法

1.  如果您使用的是 goland，您可以进入首选项->转到->导入，然后在那里添加您的配置。有很多功能，比如

在一次申报中转移所有进口

分组标准库导入

在单个组中移动所有 stdlib 导入等等

2.在 go 的主要版本发布后，他们增加了: [goimports](https://pkg.go.dev/golang.org/x/tools/cmd/goimports) 包

要安装:

```
go install golang.org/x/tools/cmd/goimports@latest
```

例如，运行:

早前:

```
import (
    "database/sql"
    "io"
    "strconv"
    "golang.org/x/net/context"
    "yourDirectory/foo/bar"
    "yourDirectory/foo/baz"
)
```

新:

```
goimports -local directoryName/import (
    "database/sql"
    "io"
    "strconv"

    "golang.org/x/net/context"

    "yourDirectory/foo/bar"
    "yourDirectory/foo/baz"
)
```

更多:

```
goimports -w -local "workingDirectory/" . : w updates source file and . refers to current directory path
```

3.正在开发的最后一个使用 [goimports-reviser](https://github.com/incu6us/goimports-reviser)

Golang 的工具，用于按 3–4 个组(带有自己的 [linter](https://github.com/incu6us/goimports-reviser/blob/master/linter/README.md) )对 goimports 进行排序:std、general、local(这是可选的)和项目依赖项。此外，将为您的代码准备格式(因此，您不需要单独使用`gofmt`或`goimports`)。使用附加选项`-rm-unused`删除未使用的导入，使用`-set-alias`为版本化的包或带有附加前缀/后缀的包重写导入别名(例如:`opentracing "github.com/opentracing/opentracing-go"`)。`-local` -将为本地导入创建组。值应该用逗号分隔。

# 选项:

```
Usage of goimports-reviser:
  -file-path string
        File path to fix imports(ex.: ./reviser/reviser.go). Required parameter.
  -format
        Option will perform additional formatting. Optional parameter.
  -list-diff
    	Option will list-diff files whose formatting differs from goimports-reviser. Optional parameter.
  -local string
        Local package prefixes which will be placed after 3rd-party group(if defined). Values should be comma-separated. Optional parameters.
  -output string
        Can be "file", "write" or "stdout". Whether to write the formatted content back to the file or to stdout. When "write" together with "-list-diff" will list the file name and write back to the file. Optional parameter. (default "file")
  -project-name string
        Your project name(ex.: github.com/incu6us/goimports-reviser). Optional parameter.
  -rm-unused
        Remove unused imports. Optional parameter.
  -set-alias
        Set alias for versioned package names, like 'github.com/go-pg/pg/v9'. In this case import will be set as 'pg "github.com/go-pg/pg/v9"'. Optional parameter.
  -set-exit-status
    	set the exit status to 1 if a change is needed/made. Optional parameter.
```

易于安装:

```
brew tap incu6us/homebrew-tap
brew install incu6us/homebrew-tap/goimports-reviser (Mac) or snap install goimports-reviser (Linux)
```

*   “-local”选项是关键
*   “-rm-used”对于清除导入非常有帮助

你可以用一些用例来编写干净的代码。go、*_test.go 和*.pb.go 文件，你可以搜索一个递归搜索它并像这样使用它

```
find . -type f -iname '*.go' ! -iname '*.pb.go' -exec goimports-reviser -rm-unused -local weavelab.xyz -file-path {} \;
```

这是搜索所有。go 文件而不是 pb.go 文件，并对您的导入进行排序。

就这样，新的一天，新的东西要学。

希望你过得愉快:)
谢谢！

参考:
https://github.com/incu6us/goimports-reviser

 [## goimports

### 命令 goimports 更新您的 Go 导入行，添加缺少的行并删除未引用的行。$ go 安装…

pkg.go.dev](https://pkg.go.dev/golang.org/x/tools/cmd/goimports)