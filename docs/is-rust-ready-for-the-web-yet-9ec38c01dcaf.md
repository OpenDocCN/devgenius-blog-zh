# Rust 为网络做好准备了吗？

> 原文：<https://blog.devgenius.io/is-rust-ready-for-the-web-yet-9ec38c01dcaf?source=collection_archive---------0----------------------->

## 技术构建

## 铁锈已经成熟 5 年了。但是它准备好构建 web 应用程序了吗？

![](img/0c728a4aaf4a11cd0d05e66f449a7fe2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Zsolt Palatinus](https://unsplash.com/@sunitalap?utm_source=medium&utm_medium=referral) 拍摄的照片

# 为什么要用铁锈？

> “鉴于我们对 Rust 的依赖，我们需要深入的内部 Rust 专业知识，就像我们对 Java 和其他基础技术一样，”AWS 的开源高管 Matt Asay 说。

Rust 于 2015 年 5 月 15 日发布了第一个稳定版本，这是一种相对较新的编程语言。Rust 的语法与 C++类似，但它可以通过使用借用检查器来验证引用，从而保证内存安全。这意味着它不需要垃圾收集。

Rust 越来越受欢迎也值得注意。自 2016 年以来，Rust 每年都在 StackOverflow 开发者调查中被选为“最受欢迎的编程语言”。Rust 很快在微软、亚马逊网络服务(Amazon Web Services)和其他科技公司建立了粉丝群。

有许多迹象表明 Rust 会继续存在，并帮助我们创造更好的软件和产品。但它准备好创建网络应用了吗？

# 用 Rust 构建 API

在他们的网页中，Rust 说它带有命令行、web 汇编、网络和嵌入式内置。在本文中，我将继续讨论网络部分。我们将构建一个带有“Hello World”JSON 响应的简单 API 示例。

首先，我们需要安装 Rust。我们就跟着锈推荐`rustup`。要安装 Rust，只需在 shell 中运行以下命令。如果已经有了就忽略！

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

为了构建一个 web 服务器，经过一些研究，我选择使用“`actix`”框架。Actix 是一个强大、实用、速度极快的 fast web 框架。鉴于这个描述看起来可能会解决我们的问题(建立一个 API)。用 cargo 创建一个新项目非常简单:

```
cargo new myapp
cd myapp
```

在你的项目目录中，你应该有一个`Cargo.toml`。这个文件就像 PHP 的`composer.json`。这是一个清单，您可以在其中描述项目依赖关系。转到您的`Cargo.toml`文件并添加`actix`依赖项:

```
[dependencies]
actix-web = "3"
```

为了检查是否一切正常，web 服务是否响应了我们的请求，让我们在 web 页面上打印经典的“hello-world ”:

```
**use** actix_web::{get, post, web, App, HttpResponse, HttpServer, Responder};

#[get("/")]
async **fn** hello() -> **impl** Responder {
    HttpResponse::Ok().body("Hello world!")
}#[actix_web::main]
async **fn** main() -> **std**::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

在项目目录中找到你的终端，点击`cargo run`。

该命令将下载所有依赖项，并将代码编译成二进制文件。该应用程序将在端口 8080 上运行，等待请求到达。如果您转到`127.0.0.1:8080`，您会在网络浏览器上看到“`Hello world!`”。

我们的网络服务器正在工作。现在，正如我之前所说的，让我们得到一个带有相同消息的 JSON 响应。要使用 JSON，我们需要一个额外的依赖项。Serde 允许有效且通用地对 Rust 数据结构进行*序列化和 *de* 序列化。在您的`Cargo.toml`中追加到新的依赖项:*

```
serde = “1.0.118”
```

下一步是更改我们的 hello 方法，用 JSON 代替纯文本进行响应:

```
use actix_web::{get, App, HttpResponse, HttpServer, Responder};
use serde::{Deserialize, Serialize};#[derive(Serialize, Deserialize)]
struct MyObj {
    message: String,
}#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().json(MyObj {
        message: String::from("Hello, world!"),
    })
}#[actix_web::main]
async **fn** main() -> **std**::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

在这里，我们声明将被返回的 JSON (MyObj)结构，在响应中，我们组成 JSON。main 保持不变，因为它只负责路由和打印方法 hello 给出的响应。

# 结论

这是一个非常基本的例子，一点也不具有挑战性。并不能证明 Rust 适合 web 开发。但我相信这是构建网络应用的必要条件。我知道一些创业公司正在将一些以前用其他语言(PHP、node 等)编写的服务转移到 Rust 等更新的语言上。开始很容易。因为它是一种编译语言，而且是内存安全的，这使得它成为未来开发各种软件的一个很好的工具。

下次我会试着做一个真正的网络应用。像待办事项列表或类似的东西，使用数据库，post 和 get 请求。构建 web 应用程序所需的所有东西。使用 Rust 很有趣，将来在我的项目中会更频繁地使用它，我很高兴能学到更多。

[1][https://insights . stack overflow . com/survey/2020 #最喜欢最害怕最想要的](https://insights.stackoverflow.com/survey/2020#most-loved-dreaded-and-wanted)

[](https://medium.com/dev-genius/building-a-crud-rest-api-with-rust-18e0bcf97d5e) [## Rust:构建 CRUD REST API

### 了解如何使用 rust 为 CRUD 操作构建 REST API，以及它与其他 web 框架的比较

medium.com](https://medium.com/dev-genius/building-a-crud-rest-api-with-rust-18e0bcf97d5e) [](https://bargainzon.site) [## 亚马逊产品列表| BargainZon

### 2022 年 11 月 11 日【广泛兼容性】USB-C 转 USB-A 适配器连接 USB C 设备(USB C 线/记忆棒/…

bargainzon.site](https://bargainzon.site)