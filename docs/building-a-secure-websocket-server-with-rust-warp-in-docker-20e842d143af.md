# 使用 Rust & Warp 和 Docker 构建安全的 WebSocket 服务器

> 原文：<https://blog.devgenius.io/building-a-secure-websocket-server-with-rust-warp-in-docker-20e842d143af?source=collection_archive---------1----------------------->

![](img/8617700f95913e49236c3a5bcb33aa24.png)

照片由[泰勒维克](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

WebSocket 服务器 TLS 入门，使用 Rust 在云中部署安全的应用程序。

那么，您现在有了一个基于 Rust 的 API /低延迟后端，并希望连接到您的前端客户端？本指南将从头到尾介绍如何使用 TLS 设置 HTTP/WebSocket 服务器，并在您自己的云应用程序的容器中运行它。

*注意:在继续之前，请确保您已经安装了最新版本的 Rust* *和*[*Docker*](https://docs.docker.com/get-docker/)*。*

# 设置 Rust 项目

让我们从建立 Rust 项目开始，使用 Cargo。在这种情况下，创建我们的新项目 *hello_rs_wss:*

```
$ cargo new hello_rs_wss
```

我们应该会看到这样的目录结构:

```
|-Cargo.toml 
|-.gitignore 
|-src
| |-main.rs
```

在继续之前，我们需要设置使用 Warp 的依赖关系。为此，修改`Cargo.toml`文件，如下所示:

```
[package]
name = "hello_rs_wss"
version = "0.1.0"
edition = "2022"[dependencies]
tokio = {version = "1.4.0", features = ["rt", "rt-multi-thread", "macros"]}
warp = {version="*", features = ["tls"]}
futures = "*"
```

然后，您可以构建项目来安装依赖项:

```
$ cargo build
```

# 从简单的 HTTP 服务器开始

首先，让我们先用 [Warp](https://github.com/seanmonstar/warp) 设置 HTTP 服务器。

这将在用户请求时返回一个简单的`index.html`页面。首先，在 Rust 项目所在的根目录下创建`index.html`,并将其修改如下:

```
<!DOCTYPE html>
<html>
    <body>
        <script>
            window.onload = () => {
                const BACKEND_URL = "wss://" + window.location.hostname + ":9231/echo"
                const socket = new WebSocket(BACKEND_URL)
                socket.onopen = () =>  { 
                    console.log("Socket Opened")
                    setInterval(_ => socket.send("Hello rust!"), 3000)
                }
                socket.onmessage = (msg) => alert(msg.data)
                socket.onerror = (err) => console.error(err)
                socket.onclose = () => console.log("Socket Closed")
            }
        </script>
    </body>
</html>
```

这就建立了一个基本的 WebSocket 客户端，它将发送“Hello rust！”每 3 秒钟从客户端的浏览器发送到我们的 WebSocket 服务器。

接下来，我们需要生成一个公钥-私钥对，Warp 将使用它来安全地提供内容。在这种情况下，我们可以使用(在项目的根目录中)进行设置:

```
$ openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.rsa -out cert.pem
```

现在，在`main.rs`文件中，我们可以设置 HTTP 服务器，它将返回我们刚刚创建的 HTML，使用一个简单的 Warp 路由:

```
use warp::Filter;#[tokio::main]
async fn main() {    
    let current_dir = std::env::current_dir().expect("failed to read current directory");
    let routes = warp::get().and(warp::fs::dir(current_dir));warp::serve(routes)
        .tls()
        .cert_path("cert.pem")
        .key_path("key.rsa")
        .run(([0, 0, 0, 0], 9231)).await;
}
```

现在，您可以使用以下命令再次运行:

```
$ cargo run
```

这将在[https://0 . 0 . 0 . 0:9231/index . html](https://0.0.0.0:9231/index.html)显示一个空网页，并要求您在浏览器上导航到该站点时接受证书警告。

现在，该页面将尝试连接到尚不存在的 WebSocket 服务器。

# 实现 WebSocket 服务器

然后我们可以设置 WebSocket 服务器，作为 Warp 服务器上的附加路由。您可以修改`main.rs`文件，使其看起来像这样:

```
use futures::StreamExt;
use futures::FutureExt;
use warp::Filter;#[tokio::main]
async fn main() {let echo = warp::path("echo")
        .and(warp::ws())
        .map(|ws: warp::ws::Ws| {
            ws.on_upgrade(|websocket| {
                let (tx, rx) = websocket.split();
                rx.forward(tx).map(|result| {
                    if let Err(e) = result {
                        eprintln!("websocket error: {:?}", e);
                    }
                })
            })
        });

    let current_dir = std::env::current_dir().expect("failed to read current directory");
    let routes = warp::get().and(echo.or(warp::fs::dir(current_dir)));warp::serve(routes)
        .tls()
        .cert_path("cert.pem")
        .key_path("key.rsa")
        .run(([0, 0, 0, 0], 9231)).await;
}
```

这现在创建了一个名为`echo`的附加路由，它公开了一个 WebSocket 端点。上面的实现只是将 WebSocket 上接收到的内容发送回相应的客户端。

现在，如果您使用`cargo run`运行这个程序，并导航到与之前相同的站点([https://0 . 0 . 0 . 0:9231/index . html](https://0.0.0.0:9231/index.html))，它将加载 JavaScript(来自之前创建的`index.html` )，该 JavaScript 将尝试连接到`echo` WebSocket 端点并每 3 秒发送一条消息。

您将能够看到客户端通过浏览器内置的`alert(...)`机制接收到来自服务器的响应。

*注意:这个例子是使用*[*Warp docs*](https://docs.rs/warp/latest/warp/)*和它们的例子的组合构建的——看看* [*基本 TLS*](https://github.com/seanmonstar/warp/blob/master/examples/tls.rs) *和* [*基本 WebSocket*](https://github.com/seanmonstar/warp/blob/master/examples/websockets.rs) *的例子。*

# 使用 Docker 构建和运行

现在我们有了一个正在运行的、安全的 HTTP 和 WebSocket 服务器，让我们创建一个容器，我们可以在其中构建和运行我们的应用程序，以便以后进行部署。

首先，在 Rust 项目的根目录下创建一个名为`Dockerfile`的文件，并向其中添加以下内容:

```
# Tells docker to use the latest Rust official image
FROM rust:latest# Copy our current working directory into the container
COPY ./ ./# Create the release build
RUN cargo build --release# Generate our self signed certs (change these parameters accordingly!)
RUN openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.rsa -out cert.pem \
    -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"# Expose the port our app is running on
EXPOSE 9231# Run the application!
CMD ["./target/release/hello_rs_wss"]
```

这是一个非常基本的例子，说明我们如何在容器中构建 Rust 应用程序，在构建过程中生成自签名证书——您需要根据您的证书信息相应地更改参数。

然后，我们可以使用以下命令构建并运行容器:

```
$ docker build -t hello_rs_wss .
$ docker run -p 9231:9231 -t hello_rs_wss
```

上面将构建一个名为 *hello_rs_wss* 的容器，然后运行该容器，并公开 9231 端口，以便与我们的应用程序进行通信。

与之前类似，当容器运行时，您可以在本地再次导航到与之前相同的 URL，以访问应用程序并测试它是否工作正常—[https://0 . 0 . 0 . 0:9231/index . html](https://0.0.0.0:9231/index.html)(记住接受自签名证书的风险)。

就是这样！您现在已经:

*   设置一个带有货物管理依赖项的 Rust 应用程序。
*   为 HTTP 和 WebSocket 通信的 TLS 加密生成您自己的证书。
*   构建并公开 HTTP 和 WebSocket 端点。
*   演示了一个安全连接到后端 API 的前端客户端。
*   在 Docker 中构建并运行整个应用程序，为快速云部署做好准备。

# 下一步是什么？

现在您已经有了一个可以工作的应用程序，这里有一些可以继续改进您的项目的地方:

*   设置浏览器通过 CA 识别证书(例如[让我们加密](https://letsencrypt.org/getting-started/))
*   通过多个阶段提高您的 Docker 构建规模
*   将您的应用部署到云提供商(例如 [AWS](https://docs.aws.amazon.com/AmazonECS/latest/userguide/docker-basics.html)

如果你也有兴趣在 Rust & Docker 中用 [actix-web](https://actix.rs/) 构建一个 HTTP 服务器，那么看看我的另一个指南:

[](https://awstip.com/rust-docker-helloworld-c38070c0dd9c) [## Rust & Docker HelloWorld！

### Rust 和 Docker 中的 HTTP 服务器入门。

awstip.com](https://awstip.com/rust-docker-helloworld-c38070c0dd9c)