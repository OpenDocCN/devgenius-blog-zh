# Rust 中的 WebSockets 和 JSON 数据入门

> 原文：<https://blog.devgenius.io/getting-started-with-websockets-and-json-data-in-rust-84434ddbfc21?source=collection_archive---------1----------------------->

本指南涵盖了设置一个简单的 WebSocket 客户端，从服务器传输 JSON 数据，并使用[wong tenite](https://docs.rs/tungstenite/latest/tungstenite/)&[Serde](https://serde.rs/)将数据解析回 Rust。

![](img/5f857f34131ecd49dae729a10f9db380.png)

[Cam Adams](https://unsplash.com/@camadams?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

WebSockets 在 web 服务应用中已经变得越来越流行，当你的应用在 Rust 中运行时，你可以以闪电般的速度传输、解析和处理数据。

这个例子包括建立一个 Rust 项目，创建一个基本的 WebSocket 服务器(用 Python ),实现 JSON 格式数据的流式传输和反序列化，所有这些都在 Rust 中完成。

*注意:在继续之前，请确保您已经安装了最新版本的 Rust* *和 Python**[*。*](https://www.python.org/downloads/)*

# *设置 Rust 项目*

*让我们首先使用`cargo new`命令创建 Rust 项目:*

```
*$ cargo new hello_rs_ws_client*
```

*这将创建一个类似如下的目录结构:*

```
*|-Cargo.toml 
|-.gitignore 
|-src
| |-main.rs*
```

*然后，我们需要通过修改`Cargo.toml`来设置项目依赖关系，使其看起来像这样:*

```
*[package]
name = "hello_rs_ws_client"
version = "0.1.0"
edition = "2022"# See more keys and their definitions at [https://doc.rust-lang.org/cargo/reference/manifest.html](https://doc.rust-lang.org/cargo/reference/manifest.html)[dependencies]
tungstenite = "0.16.0"
serde_json = "1.0.78"
url = "2.2.2"*
```

*所以现在如果你运行`cargo run`，它将安装依赖项(钨、Serde 和 URL)并运行 Hello World 示例项目。*

# *创建测试 WebSocket 服务器*

*您可以使用您想要的 WebSocket 服务器(例如，一个公开可用的流)，但是对于本指南，我将用 Python 创建一个基本的 WebSocket 服务器，这样您就可以模拟从上游返回的数据——这在任何情况下对于测试您的应用程序都是有用的。*

*首先，在项目中创建一个名为`testserver`的新目录，这样您的项目结构将看起来像这样:*

```
*|-Cargo.toml 
|-.gitignore
|-testserver 
|-src
| |-main.rs*
```

*然后在`testserver`目录中创建一个`main.py`,并修改它的内容，看起来像这样:*

```
*import asyncio
import websockets
import jsonasync def echo(websocket):
    async for message in websocket:
        response = {
            'error': None,
            'result': message
        }
        await websocket.send(json.dumps(response))async def main():
    async with websockets.serve(echo, "localhost", 8765):
        await asyncio.Future()  # run foreverasyncio.run(main())*
```

*这将创建一个服务器，该服务器将读取任何消息，将其包装在一个基本的 JSON 对象中，并将其发送回客户端。*

*要运行这段 Python 代码，我们首先需要安装`websockets`库，这可以通过在您的系统上全局安装:*

```
*$ pip install websockets*
```

**或可选的*在虚拟环境中完成这一切:*

```
*$ python3 -m venv venv
$ source venv/bin/activate
(venv) $ pip3 install websockets*
```

*这将仅在虚拟环境中本地安装依赖性。*

*然后，您可以通过运行 python 脚本来运行服务器(如果您正在使用虚拟环境，请确保您已经在虚拟环境中获得了源代码)，即:*

```
*$ python3 main.py*
```

# *实现 WebSocket 客户端*

*现在您已经有了一个基本的服务器设置，您可以构建您的 Rust 客户机并检查通信是否按预期工作。*

*您可以修改`main.rs`文件，使其看起来像这样:*

```
*use tungstenite::{connect, Message};
use url::Url;
use serde_json;fn main() { // Connect to the WS server locally
    let (mut socket, _response) = connect(Url::parse("ws://localhost:8765").unwrap()).expect("Can't connect"); // Write a message containing "Hello, Test!" to the server
    socket.write_message(Message::Text("Hello, Test!".into())).unwrap();

    // Loop forever, handling parsing each message
    loop {
        let msg = socket.read_message().expect("Error reading message");
        let msg = match msg {
            tungstenite::Message::Text(s) => { s }
            _ => { panic!() }
        };
        let parsed: serde_json::Value = serde_json::from_str(&msg).expect("Can't parse to JSON");
        println!("{:?}", parsed["result"]);
    }
}*
```

*这就建立了一个 WebSocket 客户端，它连接到本地 web 服务器，发送*“你好，测试！”*并从返回的 JSON 对象中解析出`result`键。*

*我们可以通过首先在一个终端中运行 Python WebSocket 服务器来运行它并检查一切是否正常:*

```
*$ cd testserver
$ python3 main.py*
```

*从另一个新终端运行:*

```
*$ cargo run*
```

*您应该看到 Rust 客户端打印出:*

```
*String("Hello, Test!")*
```

*太好了！现在，您已经设法:*

*   *创建 Rust 项目并设置与 Cargo 的依赖关系*
*   *用 Python 构建一个可运行的轻量级 WebSocket 服务器进行测试*
*   *使用 JSON 反序列化构建并测试 Rust WebSocket 客户端*

# *下一步是什么？*

*现在您已经有了一个工作设置，这里有一些后续的想法，让您的应用程序更进一步:*

*   *使用 Docker 部署您的客户端，并在云中运行 Rust 应用程序。*
*   *[使用 Serde 处理更复杂的 JSON 对象类型。](https://docs.serde.rs/serde_json/)*
*   *[使用 Rust WebSocket 服务器和 TLS 构建整个后端。](/building-a-secure-websocket-server-with-rust-warp-in-docker-20e842d143af)*

*如果你喜欢这个指南，看看我的其他 Rust 开发帖子:*

*[](/building-a-secure-websocket-server-with-rust-warp-in-docker-20e842d143af) [## 使用 Rust & Warp 和 Docker 构建安全的 WebSocket 服务器

### WebSocket 服务器 TLS 入门，使用 Rust 在云中部署安全的应用程序。

blog.devgenius.io](/building-a-secure-websocket-server-with-rust-warp-in-docker-20e842d143af) [](https://awstip.com/rust-docker-helloworld-c38070c0dd9c) [## Rust & Docker HelloWorld！

### Rust 和 Docker 中的 HTTP 服务器入门。

awstip.com](https://awstip.com/rust-docker-helloworld-c38070c0dd9c) 

如果你想注册 medium，请查看我的推荐链接:

https://medium.com/@justanotherdev/membership*