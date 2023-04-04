# Rust:构建 CRUD REST API

> 原文：<https://blog.devgenius.io/building-a-crud-rest-api-with-rust-18e0bcf97d5e?source=collection_archive---------0----------------------->

## 了解如何使用 rust 为 CRUD 操作构建 REST API，以及它与其他 web 框架的比较

![](img/52db18adf940f6f751145aea253e9d63.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 为什么生锈？

铁锈已经存在大约 5 年了。是一种号称“内存安全”且执行速度极快的语言。是一种多用途语言，带有用于命令行、web 程序集、网络和嵌入式应用程序的内置功能。

我想实现的是建立一个项目表的 CRUD 应用程序。最后，将性能与 PHP (lumen)内置的等效应用程序进行比较。

在下面的步骤中，我假设您已经完全安装并运行了 Rust and Cargo (rustup)。

# 柴油迁移

Diesel 是 Rust 的可扩展 ORM 和查询生成器。这是一种与数据库交互的有效方式，因为它对查询进行了抽象。

安装 diesel-CLI 来管理迁移。因为我们将只使用 SQLite，所以在特性标志中使用值 SQLite:

```
cargo install diesel_cli --no-default-features --features sqlite
```

Setup diesel 创建一个迁移文件夹和一个`diesel.toml`

```
# define database url in a dotenv file (diesel needs this)
echo DATABASE_URL=database.db > .env
diesel setup
```

现在我们将生成我们的迁移:

```
diesel migration generate create_items_table
```

这最后一个命令应该在您的 migrations 文件夹中创建一个包含两个文件的新文件夹。上下点 SQL。在 UP 文件中，您将编写 SQL 脚本来创建一个表。并在 DOWN 文件中编写脚本来恢复迁移。在 up 文件中，我还包含 insert 来播种数据库。下面的要点不包含这些插入内容，但您可以在这里访问:[https://gist . githubusercontent . com/jorgefsilva/C2 f 701 AE 9171 a 4c 67 F2 e 4 e 97 df 5 ecaf 5/raw/b 8 af 2c 529d 512442 e 817d 68 c 57 FB 5729 C2 D8 a 0/up . SQL](https://gist.githubusercontent.com/jorgefsilva/c2f701ae9171a4c67f2e4e97df5ecaf5/raw/b8af2cc529d5124442e817d68c57fb5729c2d8a0/up.sql)

运行迁移以在数据库中创建表:

```
diesel migration run
```

此外，运行 print-schema 命令，因为应用程序需要它

```
diesel print-schema > src/schema.rs
```

# 目录结构

我相信拥有更小的文件并由职责分开有利于维护，并且是组织你的项目的更容易的方法。因此，要执行接下来的步骤，您的项目目录中应该有一个 migrations 文件夹。之前由 diesel 生成，一个 src 目录将包含我们的项目逻辑。在 src 中，除了我们的主文件之外，我们还有 handlers 和 models 目录以及两个文件。一个配置文件，它将设置我们的环境变量和由 diesel 生成的模式文件。

在 handlers 文件夹中，我们有处理我们的路线的“控制器”,在 models 中，我认为文件夹名称说明了一切。

# 构建服务

第一件事是在`cargo.toml`文件中写下我们的依赖关系:

```
[package]
name = "rest-api-crud"
version = "0.1.0"
authors = ["Author name <author.name@example.com>"]
edition = "2018"# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html[dependencies]
actix-web = "3"
serde = "1.0.118"
diesel = { version = "1.4.4", features = ["sqlite", "r2d2", "chrono"] }
dotenv = "0.15.0"
r2d2 = "0.8"
chrono = { version = "0.4.10", features = ["serde"] }
```

现在我们将编写我们的`main.rs`文件，这是我们的应用程序开始的地方:

在这里，我们加载应用程序所需的所有依赖项，并将每个处理程序附加到一个路由。为创建、列出全部、列出一个和删除项目定义了 4 条路线。
为了加载环境变量，有一个配置文件，它使用。环境文件

```
let config = config::load_config();
```

我们的模型和处理程序，因为它们是目录，我们必须在每个目录中包含一个`mod.rs`文件。该文件允许在主文件中包含该模块，并且应该只包含您想要导出的内容:

```
pub mod items;
```

在这种情况下，两个`mod.rs`文件将只声明项目，因为处理程序和模型的模块都被称为项目。

我们的`items.rs`我们编写模型的代码。我们有一个从数据库中检索数据的模型和另一个插入数据的模型。这是因为 ID 是自动生成的。

最后缺少的是我们的训练员。

在这个`items.rs`中，文件是列出、删除和创建新项目的处理函数。

API 已经准备好运行`cargo run`命令。

# 特性试验

与其他语言和框架相比，使用 rust 开发 API 非常耗时。但 rust 声称速度超级快，看看我们能多快从数据库中获得所有 1000 条记录。我们将连续这样做 100 次，并计算响应时间的平均值。

使用相同的数据库，我们将比较用 PHP 和 lumen 框架构建的 API 的相同结果。我知道这不是一场公平的比赛。但重点是看我们在选择快速开发而不是性能的时候，牺牲了多少性能。

为了运行性能测试，我使用 postman 编写了一个测试，让 100 个节点到达同一个端点，并输出平均响应时间。

必须将上面的脚本粘贴到要测试的端点的 postman tests 选项卡中。

Rust API 的结果:

```
90 percentile response time 37 is lower than 200, the number of iterations is 100
```

流明 API 的结果:

```
90 percentile response time 184 is lower than 200, the number of iterations is 100
```

这两个 API 做同样的事情，从数据库返回所有 1000 条记录。Rust 的速度非常快，只需 lumen 所需时间的四分之一。

# 结论

在我看来，把 lumen 和 rust 相提并论是不公平的。用 lumen 构建 API 花了我大约 30 分钟。和铁锈一样，它需要更多的时间。有时候你的开发速度比运行速度更重要。

我开始对 rust 着迷，它的运行速度令人印象深刻。我不确定它是否准备好构建大型 web 应用程序。但可以肯定的是，对于微服务来说，性能是最重要的。

你已经用 Rust 构建过任何 web 应用了吗？在评论里告诉我。

[](https://medium.com/dev-genius/is-rust-ready-for-the-web-yet-9ec38c01dcaf) [## Rust 为网络做好准备了吗？

### 铁锈已经成熟 5 年了。但是它准备好构建 web 应用程序了吗？

medium.com](https://medium.com/dev-genius/is-rust-ready-for-the-web-yet-9ec38c01dcaf) [](https://medium.com/swlh/php-8-is-here-85211345576f) [## PHP 8 在这里

### PHP 的历史

medium.com](https://medium.com/swlh/php-8-is-here-85211345576f) [](https://medium.com/dev-genius/5-programming-languages-to-look-for-in-2021-d3d039eb5e5f) [## 2021 年要寻找的 5 种编程语言

### 我认为 2021 年将成为趋势的前 5 种编程语言

medium.com](https://medium.com/dev-genius/5-programming-languages-to-look-for-in-2021-d3d039eb5e5f) [](https://bargainzon.site) [## 亚马逊产品列表| BargainZon

### 2022 年 11 月 11 日【广泛兼容性】USB-C 转 USB-A 适配器连接 USB C 设备(USB C 线/记忆棒/…

bargainzon.site](https://bargainzon.site)