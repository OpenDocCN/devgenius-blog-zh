# 使用 Go 在子目录中提供嵌入的静态文件

> 原文：<https://blog.devgenius.io/serving-embedded-static-file-inside-subdirectory-using-go-96e9d01a85dd?source=collection_archive---------11----------------------->

![](img/c64be96a48533640ad8ffa194fbfead4.png)

Unsplash 上[@ iamrcup](https://unsplash.com/@iammrcup)的照片

# 介绍

正如 Golang v1.16 发布的包一样，现在你可以将你的任何文件包含到你的 Golang 二进制文件中。您还可以将嵌入的文件作为静态站点。但是当涉及到子目录时，您需要稍微不同地处理它。

在本文中，您将了解如何将嵌入式文件作为存储在子目录中的静态站点。

# 目录结构

```
$ tree .
.
├── frontend
│   └── public
│       ├── assets
│       │   ├── css
│       │   │   └── style.css
│       │   └── js
│       │       └── index.js
│       └── index.html
├── go.mod
└── main.go
```

# HTML 内容

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>
    <script src="/assets/js/index.js"></script>
</body>
</html>
```

为了测试是否可以使用`absolute`和`relative` URL 导入`assets`，可以像上面的代码片段一样编写`link rel`和`script src`。你可以在你的`style.css`和`index.js`里面放任何东西，只是为了确保它已经被加载。

# 问题是

在`embed`包发布之前，你曾经使用`http.Dir`来服务一个静态站点，使用如下的 Go:

```
package mainimport (
    "net/http"
)func dirHandler() http.Handler {
    return http.FileServer(http.Dir("frontend/public"))
}func main() {
    http.ListenAndServe(":8000", dirHandler())
}
```

是的，它非常有效。但是，说到`embed`:

```
package mainimport (
    "embed"
    "net/http"
)//go:embed frontend/public
var public embed.FSfunc fsHandler() http.Handler {
    return http.FileServer(http.FS(public))
}func main() {
    http.ListenAndServe(":8000", fsHandler())
}
```

它不起作用，因为你没有指定哪个子目录将被服务。`http.FS`将提供来自`root`的嵌入文件，这就是为什么你需要使用`[http://localhost:8000/frontend/public/](http://localhost:8000/frontend/public/.)` [而不是`http://localhost:8000/`来访问它。](http://localhost:8000/frontend/public/.)

但另一个问题是，它使您的`absolute`导入无法工作。`/assets/js/index.js`将不工作，因为它从`root`访问文件。

# 解决方案

为了解决这些问题，您可以使用`fs.Sub`来获得`embedded`文件的子树。所以您可以开始将`frontend/public`目录作为根目录。

```
//go:embed frontend/public
var public embed.FSfunc fsHandler() http.Handler {
    sub, err := fs.Sub(public, "frontend/public")
    if err != nil {
        panic(err)
    }return http.FileServer(http.FS(sub))
}
```

现在，您可以使用`http://localhost:8000/`访问该站点，并且您的`relative`和`absolute`导入再次工作！

感谢您的阅读！

*最初发布于 2021 年 12 月 30 日*[*https://clavinjune . dev*](https://clavinjune.dev/en/blogs/serving-embedded-static-file-inside-subdirectory-using-go/)*。*