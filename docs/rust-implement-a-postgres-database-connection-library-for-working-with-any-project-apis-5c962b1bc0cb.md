# rust——简单、干净的 Postgres 数据库连接库，可用于任何项目(API、服务)

> 原文：<https://blog.devgenius.io/rust-implement-a-postgres-database-connection-library-for-working-with-any-project-apis-5c962b1bc0cb?source=collection_archive---------2----------------------->

![](img/93e8c4ab07daf2f9173f72b658b3702a.png)

使用数据库是 Web APIs 中最常见的功能之一。如果您正在构建一个应用程序并开始使用，选择 MySQL 或 Postgres 这样的 SQL 数据库通常是一个聪明的主意。大多数情况下，它承担了处理大量复杂问题的额外责任。本文将演示如何创建一个简单的库，它建立了与 Postgres 的连接，并提供了一个使用`sqlx`库处理 Rust 的接口。这个库也可以用于 MySQL。我们的机箱可以作为数据库层在许多开发项目中重用。

# 项目设置

首先，我们将为此创建一个新的 Rust 库。

```
cargo new --lib database-lib
```

现在，编辑`Cargo.toml`来添加使用 Postgres 的依赖项。对于这一个，我们将使用`sqlx`作为我们的底层数据库库。我们还将使用一个助手库`thiserror`从这个库中提供我们自己的错误抽象。

SQLX 还提供了对不同数据库的良好抽象。入门是个可取的选择。

为了编写我们的数据库连接库，我们将编写两个主要组件

1.  错误解析和传播
2.  将作为 Postgres 连接池包装的数据库结构。

通常，我们不会出于学习目的编写错误解析和传播。但是因为我们的目标是在我们构建的每个项目中使用它，我们可以开始正确、干净的方式，然后在它的基础上进行构建。

重要的是，我们要为数据库提供一个基本的包装器，以便在将池移交给我们的应用程序之前，我们可以控制一些池设置。

首先，我们将看到连接到数据库时可能发生的错误，然后继续创建连接。

# 错误处理

由于种种原因，我们的图书馆可能无法与我们的数据库建立连接。可能是由于**无效连接字符串**、**网络** **超时**、**无效池配置**等原因。出于这个原因，我们在 Rust 中创建了自定义的错误枚举。这将负责解析和格式化底层错误。它还会传播底层错误，让应用程序处理任何细节。

随着我们库的增长，我们可以实现更多的功能并减少应用程序开销。

首先在我们的`lib.rs`文件中定义下面的错误枚举。

我们使用了一个第三方库`thiserror`，它为错误显示提供了宏，并实现了定制错误所需的特征。

目前，我们的库只抛出两种类型的错误。我们可以在将来进一步扩展，使之更加具体。

至此，是时候编写我们的数据库结构了。

# 数据库实现

我们的数据库结构很简单。当一个新的数据库被实例化时，我们初始化这个池，并从此拥有这个池。我们可以把这个游泳池提供给任何想使用它的人。为了简单起见，我们将只从数据库结构中提供一个共享的不可变引用。如果对可变池的需求增加，我们可以添加这个。

此外，我们还确保初始化时的所有错误都映射到我们的自定义错误，以便我们可以标准化该库。

一旦我们实现了这一点，我们现在就可以使用它来开始使用我们的数据库并查询它。这就是连接 Rust 中数据库所需的全部内容。如您所见，这只是几行代码。但是我们以一种在任何项目中都不用担心的方式组织了它。并且可以将它作为一个单独的板条箱来不断添加功能。这是一个干净的做法，可以在应用程序中使用。

# 测试

到目前为止，我们已经定义了数据库。我们有一个关联的方法可以创建一个新的连接池。我们现在可以编写一个连接到这个数据库的测试，并开始使用它。

下面是一个测试，它试图打开一个到 PostgreSQL 数据库的连接，并试图执行一个简单的`select`。

尝试在不设置任何环境变量的情况下直接运行该测试。我们会看到这样的输出。

```
cargo test---- tests::test_database_connection_get stdout ----
thread 'tests::test_database_connection_get' panicked at 'Database connection expected: InvalidConnectionString(EnvVar(NotPresent))', src/lib.rs:45:44
note: run with `RUST_BACKTRACE=1` environment variable to display a backtracefailures:
    tests::test_database_connection_gettest result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00serror: test failed, to rerun pass '--lib'
```

我们看到，我们的错误已经正确传播，现在可以在使用我们的库时由应用程序以干净的方式处理。

现在，将环境变量设置为以下内容，并运行测试

```
DATABASE_URL=postgres://postgres:postgres@localhost:5432/testdb?currentSchema=app cargo test 
```

如果连接成功，我们应该会看到一个成功的测试运行。

```
running 1 test
test tests::test_database_connection_get ... oktest result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.19sDoc-tests database-librunning 0 teststest result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

# 最后

这是为 Postgres 编写一个简单、强大的数据库包装器的基本例子。我们现在可以在应用程序中的任何地方使用这个库，并设置环境变量来开始使用我们的数据库。如果这个库中需要，我们还可以为 MySQL 池和其他数据库连接缓存编写特性。

以此为基础，在接下来的几篇文章中，我将讨论如何使用 Postgres 和 Rust 创建高性能的 REST APIs。

> **敬请关注，更多请关注我！**