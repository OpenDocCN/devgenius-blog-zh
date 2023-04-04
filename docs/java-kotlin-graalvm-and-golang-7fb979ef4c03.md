# Java/Kotlin/GraalVm 和 Golang。是什么情况。最具性能的框架，消耗内存，易于开发。

> 原文：<https://blog.devgenius.io/java-kotlin-graalvm-and-golang-7fb979ef4c03?source=collection_archive---------2----------------------->

有很多故事说 Golang 比 Java 快。我看到了许多基准测试，它们什么也没说，只是比较特定的框架。

目前 Java 的行业标准是 Spring。它是一个庞然大物，与轻量级 Golang 框架相比没有任何意义。Spring 的主要目标是标准化大型应用程序的开发，并为开发人员提供针对业务逻辑的能力。相反，Golang 用于微服务。它没有如此庞大的框架。

同样让我们看看 https://www.techempower.com/benchmark。在前 20 个版本中有很多轻量级的 Java 框架，但是没有 Golang 框架。Java 是高度可定制，设计成可以在任何地方运行。java 也有 GraalVm 来解决 AOT 问题，减少内存消耗，从而降低基础设施成本。

我想至少为我自己回答这个问题:)。

我将尝试指出 3 个领域中最轻量级和高性能的框架:

1.  Http 服务器
2.  序列化/反序列化
3.  数据库驱动程序

对 3 个方面进行分析:易于开发、内存消耗和性能

从商业角度来看，场景是无用的，但技术场景尽可能全面:

*   发送带有 4 个字段的用户创建请求:id，姓名，电子邮件，年龄
*   反序列化请求
*   在 Pstgres 数据库中创建用户
*   从数据库中选择用户
*   删除用户
*   序列化选定的用户
*   发送响应

# **Jvm HTTP 解决方案**

For Development 将把 Kotlin 作为主要的编程语言

关于 HTTP 服务器毫无疑问会采取普通 Netty 服务器没有任何额外的框架+反应器，方便地使用 Mono 和 Flux。java 方 VertX 的 [techempower](https://www.techempower.com/benchmark) 基准测试的领导者之一使用它。我也每天使用它，非常熟悉:)

将避免代理、Beans 和其他 java 常用的东西。只是手动启动。对于我的版本，gradle 将添加以下几行:

```
implementation(platform("io.projectreactor:reactor-bom:2022.0.0-RC1"))
implementation("io.projectreactor.netty:reactor-netty-core")
implementation("io.projectreactor.netty:reactor-netty-http")
```

主文件看起来像这样:

```
fun main() {
    val port = 8089
    val server = HttpServer.create()
        .route { routes: HttpServerRoutes ->
            routes.post(
                "/api"
            ) { request, response -> Handler.handle(request, response) }
        }
        .host("0.0.0.0")
        .port(port)
        .bindNow()

    println("Starting on port $port ...")

    server.onDispose()
        .block()
}
```

正如我提到的，我的微服务将有一个端点，可以处理所有传入的请求。

# Jvm 序列化解决方案

T [echempower](https://www.techempower.com/benchmark) benchmark 有 tab，专门用于序列化。从这个选项卡中，我可以得出有趣的结论。例如 Vertx 在所有基准测试中都很高，除了序列化。它为此使用了杰克逊。可能大部分 java 框架都在使用它。我开始寻找现成的、性能更好的东西。我找到了这个基准:[https://github.com/fabienrenaud/java-json-benchmark](https://github.com/fabienrenaud/java-json-benchmark)

根据这个基准测试[，dsl-json](https://github.com/ngs-doo/dsl-json) 是 Java 世界中序列化性能的领导者。千万不要去这个图书馆。
但是检查其他的基准测试，我发现串行化并不是 java 最强的部分。:)【https://github.com/kostya/benchmarks
[你看领导果然是铁锈和 C++。但是这个库可以认为是最快的。添加到 gradle 文件:](https://github.com/kostya/benchmarks)

```
implementation("com.dslplatform:dsl-json-java8:1.9.9")
kapt("com.dslplatform:dsl-json-java8:1.9.9")
```

和请求处理器:

```
// Init object mappper
val objectMapper = DslJson(Settings.withRuntime<Any>().includeServiceLoader())
......................................
request.receive()
                .aggregate()
                .asByteArray()
                .map { bytes -> objectMapper.deserialize(UserDto::class.java, bytes.inputStream()) }
.......................................................
// serialization stuff for the response
 .map {
                    val outputStream = ByteArrayOutputStream()
                    objectMapper.serialize(it, outputStream)
                    outputStream.toByteArray()
                }
```

在这之后，我尝试了一个简单的场景，用普通的 json 接收请求，反序列化它，打印到控制台，再次序列化并发送响应。创建了 docker 容器并执行了 apache 基准测试:

```
// warm up jvm
ab -p performance/test.json -T application/json -c 100 -n 20000 http://localhost:8089/api
// regular benchmark
ab -p performance/test.json -T application/json -c 100 -n 2000000 http://localhost:8089/api
```

最佳转速为 18K 转/秒，中等转速约为 16K 转/秒

# 添加 GraalVm

我猜 jvm 的性能很高，但是内存使用和工件大小呢？我如何降低基础设施成本？我想这是在微服务架构中使用 java 的主要人员的主要问题。因为 java 不是关于低内存消耗的。让我们试着用 GraalVm 解决这个问题…..

我估计很多人都可以这么想:)。对我来说，这和欢迎来到地狱是一样的:)

我的目标是得到一个可执行的二进制文件。我需要静态图像。当然，默认情况下，没有什么对我有用。:)

在 Oracle 文档中我找到了如何安装[https://docs . Oracle . com/en/graalvm/enterprise/20/docs/reference-manual/native-image/static images/](https://docs.oracle.com/en/graalvm/enterprise/20/docs/reference-manual/native-image/StaticImages/)

花了几个小时在我的 Ubuntu 上设置`musl-gcc`编译器和 zlib。我不知道 windows 用户是如何克服我在安装时发现的这么多问题的。终于让它工作了，但这并不是我冒险的结束。我花时间从编译中排除不同的类

```
--initialize-at-run-time
```

我使用的库也有很多代理类，这就是为什么需要在编译中添加 reflection-config.json 和一个标志。默认情况下，Netty 不是编译的。需要调查那里的存储库并逐个解决问题，创建了这个反射配置。

```
-H:ReflectionConfigurationResources=${.}/reflection-config.json
```

loke 中的初始化次数如下:

```
..................................
{
  "name": "io.netty.channel.epoll.EpollServerSocketChannel",
  "methods": [
    { "name": "<init>", "parameterTypes": [] }
  ]
},
...........................................
```

还有 JNI 配置

```
-H:JNIConfigurationResources=${.}/jni-config.json
```

所有这些东西都可以在知识库中找到。

最后命令:

```
export PATH={{TOOLCHAIN_DIR}}/bin:$PATH && {{JAVA_HOME}}/bin/native-image - static - libc=musl - no-fallback -cp ./build/libs/fast-http-1.0-SNAPSHOT-all.jar -H:Name=app -H:Class=com.fasthttp.MainKt -H:+JNI -H:+ReportUnsupportedElementsAtRuntime
```

已被执行，我得到了本地图像。还安装了 upx 实用程序来压缩 Linux 二进制文件，并为我的 Netty 服务器安装了 5.5 mb 大小的二进制文件。

让我们打包到 docker 并执行 Apache 基准测试。正如所料，我得到的结果在性能方面比 JIT 版本更差。

最佳执行速度约为 14K r/s。一般情况下，速度约为 12K r/s。图像越小，性能也越低。:)

# JVM 数据库驱动程序

这是最简单的解决方案。只需访问[https://www.techempower.com/benchmarks/#section=data-r21&测试=查询](https://www.techempower.com/benchmarks/#section=data-r21&test=query)。在用于 dingle 查询的基准中，postgres vertx 驱动程序位于第二个位置，对于多个查询，它位于第三个位置。看起来不错，它比 c++和 rust libs 的大部分都要好，而且是异步的。只需添加到 gradle 文件:

```
implementation("io.vertx:vertx-pg-client:4.3.5")
```

不幸的是，要编译 GraalVm 版本，需要添加几个库:

```
implementation("com.ongres.scram:common:2.1")
implementation("com.ongres.scram:client:2.1")
```

GraalVm 版本也面临日期序列化的问题。在使用 Locale.Root 运行时，DataTypeCodec 类失败。我找到了有关本机运行的此问题的几个解释。

无法在 lib 中直接解决这个类的问题，所以用我自己的同名同包的类覆盖了它:)。对 java 有好处。

实现非常简单。创建的普通 SQL 常量:

```
const val GET_USER = """SELECT * FROM users as u
WHERE u.id=%d"""

const val INSERT_USER = """INSERT INTO users (name, email, age)
values ('%s', '%s', %d) RETURNING id"""

const val DELETE_USER = """DELETE FROM users as u
WHERE u.id=%d"""
```

而初始化只是创建连接:

```
init {
    val connectOptions = PgConnectOptions().setPort(5432)
        .setHost("db")
        .setDatabase("postgres")
        .setUser("postgres")
        .setPassword("postgres");
    val poolOptions = PoolOptions().setMaxSize(5)
    client = PgPool.client(connectOptions, poolOptions);
}
```

查询实现也很简单:

```
val  future: Future<RowSet<Row>> = client
    .query(String.format(DELETE_USER, usr.id))
    .execute()
```

对 db 的每个查询都应该返回 Mono。我不能使用 Mono.fromFuture()，因为 Vertx Future 不是一个可完成的 future，但是我查看了代码，发现我可以从 vertx future 获得 CompletionStage，这样我就可以用 Mono.fromCompletionStage(..)

```
Mono.fromCompletionStage(future.toCompletionStage())
```

在这之后，我可以使用单声道和通量的东西进行任何转换。资源库中提供了完整的代码。

最终获得端到端工作的解决方案。这就是 java。

# Golang HTTP 解决方案

幸运的是，Golang 没有太多的解决方案:)。围棋通常只有一条路。但是这里有两个解决方案。Golang 拥有强大的 http 库。但是为了性能，存在 fasthttp lib。关于 Techempower 基准测试，性能最好的是基于该库的光纤。我决定采用 fasthttp 进行测试，原因如下:【https://github.com/valyala/fasthttp[。与 Kotlin 的实现非常相似:](https://github.com/valyala/fasthttp)

```
func main() {
   fmt.Println("Starting server on ", "8089")
   err := fasthttp.ListenAndServe(":8089", Handle)
   if err != nil {
      panic(err)
   }
}
```

# Golang 电子监管解决方案

对于序列化，我使用了与 java 相同的基准。https://jsoniter.com/发现了。Per benchmarks 为 golang 提供了最高性能的序列化/反序列化库。它在几个地方优于 java 版本。同样，jsoniter java 版本也存在，但由于某些原因，它没有在基准测试中提到:(。

此外，这个库与标准库兼容。只需要调用这个结构:

```
var json = jsoniter.ConfigCompatibleWithStandardLibrary
```

您可以使用性能更高的常规方法:

```
b := ctx.Request.Body()
user := dto.User{}
if err := json.Unmarshal(b, &user); err != nil {
......... 
```

```
b, err = json.Marshal(&usr)
...........
```

代码可以在资源库中找到。已执行 Apache 基准测试。GraalVm 版本的结果甚至更差，但已经很接近了。

最好的执行速度大约是 13K 转/秒。一般来说，大约是 8K 转/秒。有趣的是，似乎 Netty 做了这个神奇的表演。:) .但是压缩后的二进制文件大小减少了两倍，即 2.5 mb。

# Golang 数据库驱动程序

我决定使用与 java 相同的方法。一些低水平和高性能的 postgres。与 pgx 驱动程序一起使用的是一个低级的高性能接口，它公开了 PostgreSQL 特有的特性。同样的方法。创建了 3 个常量并执行了普通的 sql 查询。扫描也是手动进行的。

举个例子。代码可在回购中获得。非常简单，不需要额外的解释:

```
import . "github.com/jackc/pgx/v5/pgxpool"

....
dbPool, err := New(context.Background(), dbString)
.........
usr := dto.User{}
 err = con.QueryRow(ctx, GET_USER, id).Scan(
  &usr.Id,
  &usr.Username,
  &usr.Email,
  &usr.Age,
 )
```

# **测试结果**

让我们试着分析一下这个例子:

1.  **神器尺寸:**

Java JIT: jar 文件是 12 MB。当然结果 docker 容器会更大。我们需要有 JVM 在里面。

Java AOT(GraalVm):粗略的二进制文件大小为 49Mb。但是用 upx 工具压缩后，我有 11Mb。

Golang:粗略的二进制大小为 14 Mb。压缩后，它有 5.5Mb

2.**码头集装箱尺寸:**

Java JIT:它有最大的容器——306 MB

Java AOT(GraalVm): 30Mb

戈朗:17 兆字节

3. **Apache 性能指标评测:**

我执行了以下脚本:

```
ab -p performance/test.json -T application/json -c 100 -n 20000 http://localhost:8089/api
ab -p performance/test.json -T application/json -c 100 -n 1000000 http://localhost:8089/api
```

首先是预热应用程序。这对 java 很重要。:)

首先，我尝试将内存限制为 50 Mb。Java (AOP & JIT)因 OOM 而失败。Golang 即使是 20 Mb 也没问题，性能没有明显下降。对于 10 Mb 的内存来说甚至还可以。

java 的最小内存是 100Mb。这就是为什么每个应用程序的 Docker 将从以下设置开始:

```
docker run -it -d --memory=100m --cpus=4 -p 8089:8089 --network fast-http ${image name}

// postgres and container should be in the same network
```

游戏开始:) :

1.  **Java AOT (GraalVm):**

```
Memory usage: 
  Cold start: "Out of the box it allocates 75 Mb from the beginning"
  When tests started: "It was 99 Mb all the time test was running"
  CPU: "Intensive usage. Always shown more than 100 %." - 150 - 250
RPS: 
  1\. For first 20K requests it shown 1172 reqests/second
  2\. For the main benchmark it shown 337  requests/second - seems single instance of postgres is overloaded. But I haven't seen any high load on postgres side
```

2.戈朗:

```
Memory usage: 
  Cold start: "Out of the box it allocates 2 Mb from the beginning"
  When tests started: "It was 22 Mb all the time test was running"
  CPU: "High Intensive usage. Always shown more than 100 %." - 250 - 330
RPS: 
  1\. For first 20K requests it shown 259 reqests/second
  2\. For the main benchmark it shown 223  requests/second - stable. Not familiar with pgx driver
```

3. **Java JIT:**

```
Memory usage: 
  Cold start: "Out of the box it allocates 34Mb from the beginning"
  When tests started: "It was 75 Mb all the time test was running"
  CPU: "Low Intensive usage. From the beginning was about 62%. Then when JVM warms up it become 45% or even lower" 30% some time
RPS: 
  1\. For first 20K requests it shown 1185 reqests/second
  2\. For the main benchmark it shown 332  requests/second - same as for AOT. Guess have some DB limitations 
```

# 结论

正如所料，jvm 需要资源。Golang 可以在非常有限的资源下工作。

与 JIT 相比，GraalVM 无疑解决了工件大小的问题，但是在编译阶段的环境中增加了很多复杂性。如果你认为可以在 GraalVM 下编译普通的 JVM 应用程序，这是不正确的。与 golang 相比，这是一种痛苦。

使用 GraalVM 进行开发需要时间。每次都可以喝咖啡休息一下:)。为了加快速度，我在开发时使用普通的 JVM。但不能保证它会与 AOT 合作。:)

golang 的错误管理简直是地狱。但这可能是我来自 Java 世界。:)

易于开发:我看不出 Java JIT 和 Golang 有什么困难。Golang 的编译速度当然是最高的。不会有咖啡刹车:)。对于 GraalVM 来说，这并不容易。应该花更多的时间让这些东西工作起来。

性能。对于所选的框架，Java 优于 Golang。但我想对于更常见的选择情况可能是相反的。也许如果我尝试调整 JVM，结果会有所不同。

同时，我试着在 Rust 中实现这个东西，在所有优化之后，工件的大小小于 1Mb。:)

顺便说一句。*关于性能结果没用:)。为项目选择具体的堆栈是更复杂的任务，与那里描述的内容无关。*

存储库可用

[](https://github.com/aviplayer/fast-web-java) [## GitHub - aviplayer/fast-web-java:调查性能 java

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/aviplayer/fast-web-java) [](https://github.com/aviplayer/fast-http-golang) [## GitHub-avi player/fast-http-Golang:Golang 性能困扰

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/aviplayer/fast-http-golang) 

对于配置、编译和执行，我使用了 justfile。请将 Justfiles 顶部的变量更改为您的路径。

需要在考试前开始 postgres。只需 cd 到 infra 目录并运行 docker compose up -d

然后从项目的根开始:

对于 java JIT:

```
just docker-java
```

对于 java AOT 来说，所有这些怪异的设置都完成了:)

```
just docker-native
```

对于 Golang 来说，从项目的根

```
just run
```

要从 java 项目的根目录执行测试:

```
just test
```