# 在 Rust 中读写 JSON 文件

> 原文：<https://blog.devgenius.io/reading-and-writing-a-json-file-in-rust-2731da8d6ad0?source=collection_archive---------1----------------------->

本文将向您介绍在 RUST 中解析和编写 JSON 文件。

![](img/5c52ca70e592f44e6dca018e507395d6.png)

[腾雅特](https://unsplash.com/@tengyart?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

作为一名软件开发人员/工程师，我相信您一定听说过 JSON 文件或 JSON 格式。**J**ava**S**script**O**object**N**rotation，也就是众所周知的 **JSON** ，是一种流行的轻量级数据交换格式，人和机器都可以轻松地读取和写入。

JSON 由于其简单性和对不同编程语言的易用性，这些年来越来越受欢迎。JSON 经常被用作模拟数据存储需求的原型工具，或者直接用作数据存储。

下面是一个 JSON 文件的例子，其中包含了一些关于 **Missy 的饮食**的信息(如果你不知道 Missy 是谁，我邀请你在这里阅读我以前的任何一篇文章[。 **PS:请不要告诉她我公开了她的饮食团**](https://medium.com/@anismousse)

密西日常饮食和行为的秘密信息

如您所见，该文件包含一个匿名对象，该对象包含两个数组， **food，**和 **missy_food_schedule。****食物**数组包含了所有大小姐喜欢的食物， **missy_food_schedule** 有大小姐 2022 年 2 月最后三天的饮食时间表以及更多绝密信息。

但是我们如何解析这个 JSON 文件来提取一些需要的信息呢？更新这些信息并保存我们的修改？

在 Rust 中，我们可以用两种不同的方法解析 JSON 文件:

*   **动态方法:**这里的假设是我们并不完全理解 JSON 文件中的数据，所以我们的程序必须动态地检查任何数据字段的存在和类型。
*   **静态方法:**这里假设我们完全理解 JSON 文件中存在的数据，所以我们将使用反序列化来检查任何字段的存在和类型。

## JSON 文件的动态解析

为了动态解析 JSON 文件，我们将使用 crate **serde_json** (更多信息[在这里](https://docs.serde.rs/serde_json/))。这个板条箱需要添加到我们的 **Cargo.toml** 的 dependencies 部分下。

用于动态解析 JSON 文件的项目的 Cargo.toml

下面的程序是为了动态解析文件 **missy_secrets.json** 而创建的，它将 missy 在 2 月最后三天消耗的食物数量翻倍，以反映更准确的现实:)。

用于静态解析 JSON 文件的项目的 main.rs

如您所见，在使用每个元素之前，我们需要检查它的存在和类型。这会导致一些复杂而冗长的代码行。

编写和保存 JSON 文件是一个简单的操作，如我们示例中的第 28 行到第 32 行所示。

为了执行这个程序，我们传递第一个参数 **missy_secrets.json** 文件，第二个参数将是在更新 missy 每天消耗的食物数量后创建的新 json 文件的名称(**miss _ secrets _ dynamic . JSON**)。

```
cargo run missy_secrets.json miss_secrets_dynamic.json
```

程序执行的结果将是以下 JSON 文件:

动态解析后更新的 JSON 文件

正如您可能已经意识到的，对象' **missy_food_shcedule** '中的每一项的' **quantity** 值都如预期的那样增加了一倍。

## 静态解析 JSON 文件

当我们确定 JSON 文件结构时，我们**应该**使用静态类型技术来解析我们的文件。静态解析会直接将 JSON 文件中的数据转换成一些 Rust 对象。

为了实现这样的目标，我们将使用板条箱 **serde** (更多信息[此处](https://docs.serde.rs/serde/index.html))、 **serde_derive** (更多信息[此处](https://docs.serde.rs/serde_derive/index.html))、和 **serde_json** (用于动态解析 json 文件的板条箱)。

用于静态解析 JSON 文件的项目的 Cargo.toml

下面是对同一个 JSON 文件执行静态解析的源代码，和以前一样，该程序的目标是让 Missy 消耗的食物数量翻倍。

用于动态解析 JSON 文件的项目的 main.rs

正如您可能已经意识到的，我们创建了三个结构，在 **missy_secrets.json** 文件中每个对象类型一个。

从面向对象编程的角度来看，结构实际上等同于类。任何链接到结构或类型的行为都是通过 Rust 的**特征**引入的(在我过去的一篇文章[中有更多关于这方面的内容，这里是](https://anismousse.medium.com/why-you-should-learn-rust-fa52d0139b85))。

在我们的代码源中，每个结构前面都有以下属性:

```
#[derive(Deserialize, Serialize, Debug)]
```

*   需要**反序列化**特征来将 JSON 字符串解析(即读取)到这个结构中。
*   需要使用 **Serialize** 特征将这个结构格式化(即写入)成一个 JSON 字符串。
*   **Debug** 特性用于在调试跟踪中打印一个结构。

用下面的命令运行这个程序将产生 JSON 文件的更新版本。

```
cargo run missy_secrets.json miss_secrets_static.json
```

可以看到，这个例子更加简洁。创建一些结构来表示 JSON 文件中的每个对象，这允许我们直接提取所有数据，而无需检查任何元素的存在或类型。

这种方法更具可塑性、可维护性，并且易于扩展。

如果我们更改 JSON 文件中的一个现有对象，我们只需要在程序中表示该对象的结构中做同样的修改。

如果我们在 JSON 结构中添加一个新元素，我们只需要创建一个新的结构来表示这个新元素。

## 结论

由于多样化的板条箱，在 Rust 中利用 JSON 文件是一项可实现的任务，使我们的生活更加轻松。

编写 JSON 文件是一项简单的任务，但是当我们解析和利用现有 JSON 文件中的一些信息时，复杂性就显现出来了。

我们应该总是以静态解析 JSON 文件为目标，假设我们完全理解文件中存在的各种项目以及与每个元素相关联的类型。

本文提到 JSON 文件通常是模拟任何数据存储需求的良好起点。Rust 确实有一些非常酷的板条箱，可以帮助你与 Redis(板条箱 [redis](https://docs.rs/redis/latest/redis/) )、PostgresSQL(板条箱 [postgres](https://docs.rs/postgres/0.15.2/postgres/) )等数据库进行交互。

一如既往，欢迎鼓掌、订阅、评论！！

![](img/fe818324b4b5cbd72b1122c99932f23e.png)

米西幸福地不知道我与世界分享了她的饮食秘密

## 资源

我过去关于铁锈的文章:[这里](https://medium.com/@anismousse)。

JSON 官方页面:[此处](https://www.json.org/json-en.html)。

JSON 的基本介绍:[此处](https://www.w3schools.com/js/js_json_intro.asp)。

Rust 的结构:[此处为](https://doc.rust-lang.org/book/ch05-01-defining-structs.html)。

铁锈的特性:[此处](https://doc.rust-lang.org/book/ch10-02-traits.html)。