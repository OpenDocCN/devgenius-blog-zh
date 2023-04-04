# 聊天机器人驱动的开发

> 原文：<https://blog.devgenius.io/chatbot-driven-development-fbb4392ff5b4?source=collection_archive---------10----------------------->

## 用 OpenAI 的 ChatGPT 分享我的编码实验

你可能听说过 [ChatGPT](https://openai.com/blog/chatgpt/) ，它自发布以来人气飙升，已经[突破一百万用户](https://twitter.com/sama/status/1599668808285028353?s=20&t=i1HtKPGeYadE397wB6bX9Q)。人们用这个工具做一些绝对令人惊讶的事情，比如在里面建一个[虚拟机](https://twitter.com/317070/status/1599152176344928256?s=20&t=yW4LjC4-7IY8RyiQ_FYtaA)或者[写学术论文](https://www.theguardian.com/technology/2022/dec/04/ai-bot-chatgpt-stuns-academics-with-essay-writing-skills-and-usability)。

我也不能错过尝试的机会。目前，我正在学习 Rust 编程语言(而且觉得很神奇！)，并构建小项目来实践。如果我用人工智能助手来促进我的学习会怎么样？我们已经有了像 [Copilot](https://github.com/features/copilot) 或 [Ghostwriter](https://blog.replit.com/ai) 这样的专用工具来帮助编写代码。但是也许有可能更进一步，用一个通用的聊天机器人来构建软件，它可以给我们反馈并帮助我们改进。

事实上，使用自然语言来构建代码并不是一个新的想法。OpenAI 之前发布了一个 [JavaScript playground](https://beta.openai.com/codex-javascript-sandbox) ，允许用户使用文本命令构建 web 应用。然而，由于使用了大型语言模型，ChatGPT 并不局限于任何特定的编程语言，而且功能更加强大。

在这篇文章中，我将分享我使用 ChatGPT 构建一个简单的 Rust HTTP 客户端的经验，该客户端向…一个 OpenAI 模型发送请求！这是一个非常基本的概念验证，甚至不是一个 MVP。但是它应该让您对 ChatGPT 的能力有所了解。我们开始吧！

> 在这里你可以找到这篇文章中创建的代码。

![](img/a25be1ce439c2f1fe0fd0d91884ffe2f.png)

我情不自禁地为这篇文章生成了一张封面图片，并且使用了 [OpenAI 模型](https://openai.com/dall-e-2/)。

# 0.介绍

我使用以下格式规则来组织文章。

*   我发布到聊天室的所有文本片段都以 ***我开始。***
*   在某些情况下，我不仅写了纯文本，还写了小代码片段来引导对话。在这种情况下，我用降价式的三个反勾号将代码段包装起来。
*   从模型接收的响应以 ***ChatGPT*** 开始。
*   在某些(大多数)情况下，这些回答被编辑以排除用简单英语写的过于冗长的片段或缩短重复的片段，并且只保留最相关的部分。
*   我和 GPT 的信息都是用斜体字写的。文本的其余部分以普通字体书写(就像这个介绍一样),以包含附加的注释。
*   在某些地方，我使用`[sentenses in square brackets]`来添加“内嵌”评论，也就是说，它不是来自聊天的一字不差的消息。
*   最后，当我使用聊天机器人时，它不是很稳定，所以，我不得不从头开始对话几次，或者使用之前创建的片段重新生成答案。

# I .编写 API 客户端

***我:*** *我需要你帮我写一个 Rust 程序。参见要求:*

1.  该程序是一个简单的 CLI 客户端，用于打开基于 AI GPT 的模型。
2.  *程序将一个字符串作为提示，向 API 发出请求。*
3.  *它从 env 变量或。env 文件(如果存在)。*
4.  *如果没有提供 API 密钥/令牌，它会要求验证。*
5.  程序可以使用任何你认为有助于达到要求结果的板条箱。

***ChatGPT:*** `[phrases in plain English are omitted]`

```
use std::env;

// Import the OpenAI API client crate
use openai::Client;

// Read the API credentials from environment variables
let api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");

// Create a new client with the API key
let client = Client::new(api_key);

// Prompt the user for input
println!("Enter a prompt:");
let mut prompt = String::new();
stdin().read_line(&mut prompt)?;

// Use the client to make a request to the API
let response = client.completions(prompt, None, None, None)?;

// Print the response
println!("{:?}", response);
```

***我:*** *好吧，就说我没有* `*openai*` *板条箱吧。为我写一个简单的实现，用* `*reqwest*` *板条箱代替。*

***ChatGPT:***

```
use std::env;

// Import the reqwest crate
use reqwest;

// Read the API credentials from environment variables
let api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");

// Prompt the user for input
println!("Enter a prompt:");
let mut prompt = String::new();
stdin().read_line(&mut prompt)?;

// Build the request URL with the prompt and API key
let url = format!("https://api.openai.com/v1/completions/models/davinci/versions/1?prompt={}&api_key={}", prompt, api_key);

// Send the request and parse the JSON response
let response: serde_json::Value = reqwest::get(url)?.json()?;

// Print the response
println!("{:?}", response);
```

***Me:*** *请将创建好的线条隐藏在专用客户端对象后面。它可能看起来如下:*

```
pub struct Client {
    /// add any fields you deem necessary
}
impl Client {
    pub fn authenticate(&mut self) {
        /// does API authentication if not authenticated yet
    }
    pub fn prompt(&self, message: &String) -> OpenAIResult {
        /// implement the request and return result as a dedicated result structure
    }
    /// any other methods you might need
}
```

***ChatGPT:***

```
use std::env;
use std::error::Error;

// Import the reqwest crate
use reqwest;

// Import the serde_json crate
use serde_json;

// Define a result type for the API response
type OpenAIResult = Result<serde_json::Value, Box<dyn Error>>;

// Define the Client struct
pub struct Client {
    api_key: String,
    authenticated: bool,
}

impl Client {
    // Implement the authenticate method
    pub fn authenticate(&mut self) {
        if !self.authenticated {
            // Read the API key from environment variables
            self.api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");
            self.authenticated = true;
        }
    }

    // Implement the prompt method
    pub fn prompt(&self, message: &String) -> OpenAIResult {
        // Build the request URL with the prompt and API key
        let url = format!("https://api.openai.com/v1/completions/models/davinci/versions/1?prompt={}&api_key={}", message, self.api_key);

        // Send the request and parse the JSON response
        let response: serde_json::Value = reqwest::get(url)?.json()?;

        // Return the response
        Ok(response)
    }
}
```

***我:*** *编辑一下你的解决方案。不要使用任何硬编码的字符串，而是将它们放入全局变量和/或动态构造。*

***ChatGPT:***

```
use std::env;
use std::error::Error;

// Import the reqwest crate
use reqwest;

// Import the serde_json crate
use serde_json;

// Define a result type for the API response
type OpenAIResult = Result<serde_json::Value, Box<dyn Error>>;

// Define global variables for the API endpoint and model
const API_ENDPOINT: &str = "https://api.openai.com/v1/completions/models/";
const MODEL: &str = "davinci";
const MODEL_VERSION: &str = "1";

// Define the Client struct
pub struct Client {
    api_key: String,
    authenticated: bool,
}

impl Client {
    // Implement the authenticate method
    pub fn authenticate(&mut self) {
        if !self.authenticated {
            // Read the API key from environment variables
            self.api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");
            self.authenticated = true;
        }
    }

    // Implement the prompt method
    pub fn prompt(&self, message: &String) -> OpenAIResult {
        // Build the request URL with the prompt, model, and API key
        let url = format!("{}{}/versions/{}?prompt={}&api_key={}", API_ENDPOINT, MODEL, MODEL_VERSION, message, self.api_key);

        // Send the request and parse the JSON response
        let response: serde_json::Value = reqwest::get(url)?.json()?;

        // Return the response
        Ok(response)
    }
}
```

***我:*** *写一系列命令，我需要执行这些命令来设置你上面创建的项目。*

***ChatGPT:*** `[returns a response with lots of text between commands]`

***我:*** *对于上面你给我的答案，去掉所有用英文写的“纯文本”，创建一个单独的代码片段，作为 bash 命令的序列，来实现结果。*

下面是一系列 bash 命令，您可以使用它们在 Rust 中为基于 OpenAI GPT 的模型建立一个简单的 CLI 客户端项目。

```
# Install the Rust programming language
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Create a new project directory and navigate to it
mkdir openai-client
cd openai-client

# Initialize a new Rust project with cargo
cargo init

# Edit the Cargo.toml file and add the reqwest and serde_json crates as dependencies
echo '[dependencies]' >> Cargo.toml
echo 'reqwest = "0.10"' >> Cargo.toml
echo 'serde_json = "1.0"' >> Cargo.toml

# Create a new file named src/main.rs
touch src/main.rs

# Copy the code for the Client struct into src/main.rs
echo 'use std::env;
use std::error::Error;

// Import the reqwest crate
use reqwest;

// Import the serde_json crate
use serde_json;

// Define a result type for the API response
type OpenAIResult = Result<serde_json::Value, Box<dyn Error>>;

// Define global variables for the API endpoint and model
const API_ENDPOINT: &str = "https://api.openai.com/v1/completions/models/";
const MODEL: &str = "davinci";
const MODEL_VERSION: &str = "1";

// Define the Client struct
pub struct Client {
    api_key: String,
    authenticated: bool,
}

impl Client {
    // Implement the authenticate method
    pub fn authenticate(&mut self) {
        if !self.authenticated {
            // Read the API key from environment variables
            self.api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");
            self.authenticated = true;
        }
    }

    // Implement the prompt method
    pub fn prompt(&self, message: &String) -> OpenAIResult {
        // Build the request URL with the prompt, model, and API key
        let url = format!("{}{}/versions/{}?prompt={}&api_key={}", API_ENDPOINT, MODEL, MODEL_VERSION, message, self.api_key);

        // Send the request and parse the JSON response
        let response: serde_json::Value = reqwest::get(url)?.json()?;

        // Return the response
        Ok(response)
    }
}' >> src/main.rs

# Build and run the project
cargo run
```

***我:*** *差不多了！但是，您创建的* `*main.rs*` *文件无效。它不具备* `*main*` *功能。请编辑您创建的代码片段，使其包含一个基本的 main 函数，该函数从 args 获取用户提示，并向 API 发送请求。*

***ChatGPT:*** `[updates the script adding main function; showing the updated part only]`

```
... // [the rest of the code goes above]

// Define the main function
fn main() {
    // Create a new client
    let mut client = Client {
        api_key: "".to_string(),
        authenticated: false,
    };

    // Authenticate the client
    client.authenticate();

    // Read the prompt from command-line arguments
    let args: Vec<String> = env::args().collect();
    if args.len() < 2 {
        eprintln!("Usage: openai-client <prompt>");
        process::exit(1);
    }
    let prompt = &args[1];

    // Use the client to make a request to the API
    let response = client.prompt(prompt).unwrap();

    // Print the response
    println!("{:?}", response);
}
```

***我:*** *现在，还有一件事。请创建一个* `*Client::new*` *来创建一个客户端，其* `*api_key*` *字符串为空，并且* `*false*` *字段的值为* `*authenticated*` *字段，并使用它来代替创建客户端的实例。因此，更新后的行应该如下所示:*

```
let mut client = Client::new();
```

***ChatGPT:*** `[unfortunately, this time, the conversation stopped abruptly; the snippet was trimmed half-way, and the model was thinking indefinitely without any further outout; so, I left the snippet as is]`

哦，真令人印象深刻！该模型准确地跟踪对话，并根据请求对代码进行适当的修改。关键时刻:在本地复制粘贴代码，添加一个缺少的主函数，并尝试运行它！

```
chmod +x create_client.sh
./create_client.sh
```

> 我们到了吗？嗯，不完全是…

```
error[E0433]: failed to resolve: use of undeclared crate or module `process`
  --> src/main.rs:62:9
   |
62 |         process::exit(1);
   |         ^^^^^^^ use of undeclared crate or module `process`

error[E0277]: the trait bound `std::string::String: IntoUrl` is not satisfied
   --> src/main.rs:40:56
    |
40  |         let response: serde_json::Value = reqwest::get(url)?.json()?;
    |                                           ------------ ^^^ expected an implementor of trait `IntoUrl`
    |                                           |
    |                                           required by a bound introduced by this call
    |
    = note: required because of the requirements on the impl of `IntoUrl` for `std::string::String`
note: required by a bound in `reqwest::get`
   --> [...].cargo/registry/src/github.com-1ecc6299db9ec823/reqwest-0.10.10/src/lib.rs:256:21
    |
256 | pub async fn get<T: IntoUrl>(url: T) -> crate::Result<Response> {
    |                     ^^^^^^^ required by this bound in `reqwest::get`
help: consider borrowing here
    |
40  |         let response: serde_json::Value = reqwest::get(&url)?.json()?;
    |                                                        +

error[E0277]: the `?` operator can only be applied to values that implement `Try`
  --> src/main.rs:40:43
   |
40 |         let response: serde_json::Value = reqwest::get(url)?.json()?;
   |                                           ^^^^^^^^^^^^^^^^^^ the `?` operator cannot be applied to type `impl Future<Output = Result<Response, reqwest::Error>>`
   |
   = help: the trait `Try` is not implemented for `impl Future<Output = Result<Response, reqwest::Error>>`

Some errors have detailed explanations: E0277, E0433.
For more information about an error, try `rustc --explain E0277`.
error: could not compile `openai-client` due to 3 previous errors
```

你可能已经注意到了，这个片段不太正确。

1.  生成的 bash 脚本使用`>>`操作符将创建的代码附加到`main.rs`中，而不是使用`>`从头开始创建文件。(所以，你最后得到了*两个*主函数。)
2.  它还向我手动修复的 TOML 文件中写入了一个额外的`[dependencies]`行，以便让`cargo run`工作。
3.  漏掉了包括`std::process`。
4.  依赖版本很重要:`reqwest=0.10`默认使用异步 API，因此不能与 ChatGPT 生成的代码一起工作。可能的解决方案:用`#[tokio::main]`修饰主函数，使其异步，或者降级版本使用同步 API。
5.  最后，它应该使用`&url`而不仅仅是`url`。

我在尝试运行生成的脚本和监听编译器时发现了这些错误，并在编辑器中一个接一个地修复它们。但除此之外，它看起来很有道理！并且，准确地遵循说明。

如果我们能让机器人帮我们修复 bug 呢？我们来试试吧！

# 二。修复 bug

***我:*** *我有一个锈程序，不能编译。我会给你看我程序的内容，还有错误。请告诉我哪里有错误，并生成一个新的文件(或文件)与更新的内容，我可以用来成功地编译代码。*

*下面是我的源代码:* `[copy-pasted main.rs script that doesn't compile]`

*下面是我的错误:* `[copy-pasted error output coming from the Rust compiler]`

***ChatGPT:****代码中没有定义* `*process*` *模块。* `*process*` *模块是* `*std::process*` *机箱的一部分，需要在文件开头导入。另外，第 40 行的* `*url*` *变量是一个* `*String*` *，需要将其转换为* `*Url*` *类型，以便作为参数传递给* `*reqwest::get*` *方法。*

*下面是代码的更新版本，编译时不会出错:*

```
use std::env;
use std::error::Error;
use std::process;

// [same as before]

impl Client {
    // Implement the authenticate method
    pub fn authenticate(&mut self) { ... }

    // Implement the prompt method
    pub fn prompt(&self, message: &String) -> OpenAIResult {
        // Build the request URL with the prompt, model, and API key
        let url = format!("{}{}/versions/{}?prompt={}&api_key={}", API_ENDPOINT, MODEL, MODEL_VERSION, message, self.api_key);

        // Convert the URL to a `Url` type and send the request
        let url: reqwest::Url = url.parse()?;
        let response: serde_json::Value = reqwest::get(url)?.json()?;

        // Return the response
        Ok(response)
    }
}

// [same as before]
```

它捕捉到了`std::process`的问题，也对`url`变量做了一些修改。有用吗？

```
 Compiling openai-client v0.1.0 (...)
error[E0277]: the `?` operator can only be applied to values that implement `Try`
  --> src/main.rs:42:43
   |
42 |         let response: serde_json::Value = reqwest::get(url)?.json()?;
   |                                           ^^^^^^^^^^^^^^^^^^ the `?` operator cannot be applied to type `impl Future<Output = Result<Response, reqwest::Error>>`
   |
   = help: the trait `Try` is not implemented for `impl Future<Output = Result<Response, reqwest::Error>>`

For more information about this error, try `rustc --explain E0277`.
error: could not compile `openai-client` due to previous error
```

还没有！让我们要求一个解决方案…

***我:*** *嗯，片段还是失败。这一次，错误是:[复制粘贴上面的错误]*

***ChatGPT:****该错误消息表明* `*?*` *运算符不能与* `*impl Future<Output = Result<Response, reqwest::Error>>*` *类型的值一起使用。在 Rust 中，* `*?*` *运算符只能应用于实现* `*Try*` *特征的值。*

*`*reqwest::get*`*方法返回一个* `*Future*` *，它是一个对象，表示一个可能还不可用的值。为了将* `*?*` *运算符与* `*reqwest::get*` *方法的结果一起使用，必须使用* `*await*` *关键字将* `*Future*` *转换为* `*Result*` *。**

**下面是代码的更新版本，编译时不会出错:**

```
*use std::env;
use std::error::Error;
use std::process;

// Import the reqwest crate
use reqwest;

// Import the serde_json crate
use serde_json;

// Define a result type for the API response
type OpenAIResult = Result<serde_json::Value, Box<dyn Error>>;

// Define global variables for the API endpoint and model
const API_ENDPOINT: &str = "https://api.openai.com/v1/completions/models/";
const MODEL: &str = "davinci";
const MODEL_VERSION: &str = "1";

// Define the Client struct
pub struct Client {
    api_key: String,
    authenticated: bool,
}

impl Client {
    // Implement the authenticate method
    pub fn authenticate(&mut self) {
        if !self.authenticated {
            // Read the API key from environment variables
            self.api_key = env::var("OPENAI_API_KEY").expect("OPENAI_API_KEY must be set");
            self.authenticated = true;
        }
    }

    // Implement the prompt method
    pub async fn prompt(&self, message: &String) -> OpenAIResult {
        // Build the request URL with the prompt, model, and API key
        let url = format!("{}{}/versions/{}?prompt={}&api_key={}", API_ENDPOINT, MODEL, MODEL_VERSION, message, self.api_key);

        // Convert the URL to a `Url` type and send the request
        let url: reqwest::Url = url.parse()?;
        let response: serde_json::Value = reqwest::get(url).await?.json().await?;

        // Return the response
        Ok(response)
    }
}

// Define the main function
#[tokio::main]
async fn main() {
    // Create a new client
    let mut client = Client {
        api_key: "".to_string(),
        authenticated: false,
    };

    // Authenticate the client
    client.authenticate();

    // Read the prompt from command-line arguments
    let args: Vec<String> = env::args().collect();
    if args.len() < 2 {
        eprintln!("Usage: openai-client <prompt>");
        process::exit(1);
    }
    let prompt = &args[1];

    // Use the client to make a request to the API
    let response = client.prompt(prompt).await.unwrap();

    // Print the response
    println!("{:?}", response);
}*
```

**注* `[it is defintely wanted to add some comment, but stopped here.]`*

*这是很好的进步！这一次，代码片段使用了异步 API，编译错误的数量减少了。然而，代码仍然不工作，因为我们还需要对 TOML 文件添加一些修正。(也许我也应该把文件的内容传递给机器人？)*

```
*[dependencies]
tokio = { version = "0.2", features = ["full"] }
reqwest = { version = "0.10", features = ["json"] }*
```

*否则，代码片段这次成功编译！但是真的管用吗？*

```
*Object {"error": Object {"code": Null, "message": String("Invalid URL (GET /v1/completions/models/davinci/versions/1)"), "param": Null, "type": String("invalid_request_error")}}*
```

*显然，端点和/或参数是错误的。让我们再一次寻求帮助！这一次，我们使用文档中[的一个例子来提示这个模型。](https://beta.openai.com/docs/api-reference/making-requests)*

# *三。发送请求*

****Me*** *:我用下面的代码片段做一个打开 AI API 的请求。**

```
*const API_ENDPOINT: &str = "https://api.openai.com/v1/completions/models/";
const MODEL: &str = "davinci";
const MODEL_VERSION: &str = "1";

pub async fn prompt(&self, message: &String) -> OpenAIResult {
    // Build the request URL with the prompt, model, and API key
    let url = format!("{}{}/versions/{}?prompt={}&api_key={}", API_ENDPOINT, MODEL, MODEL_VERSION, message, self.api_key);

    // Convert the URL to a `Url` type and send the request
    let url: reqwest::Url = url.parse()?;
    let response: serde_json::Value = reqwest::get(url).await?.json().await?;

    // Return the response
    Ok(response)
}*
```

**但是，它失败了，并显示错误:**

```
*Object {"error": Object {"code": Null, "message": String("Invalid URL (GET /v1/completions/models/davinci/versions/1)"), "param": Null, "type": String("invalid_request_error")}}*
```

**我猜是参数或者网址，或者两者都是错的。你能给我看一下正确配置的代码片段吗？这是一个使用* `*curl*` *来查询 API 的 bash 脚本，并且工作正常。**

```
*curl https://api.openai.com/v1/completions \
-H "Content-Type: application/json" \
-H "Authorization: Bearer YOUR_API_KEY" \
-d '{"model": "text-davinci-003", "prompt": "Say this is a test", "temperature": 0, "max_tokens": 7}'*
```

**请修改我的代码，以实现与上面显示的* `*curl*` *相同的请求。**

****ChatGPT:****您的代码试图使用* `*GET*` *HTTP 方法向 OpenAI API 发出请求，这是不正确的。您提供的* `*curl*` *命令使用了* `*POST*` *方法，这是发送带有正文的请求时使用的正确方法。**

**要修复您的代码，您需要将* `*reqwest::get*` *调用改为* `*reqwest::Client::post*` *，并将请求体作为参数传递给* `*post*` *方法。您还需要在请求中设置* `*Content-Type*` *和* `*Authorization*` *头，如* `*curl*` *命令所示。**

**这是你的代码的更新版本，应该可以工作:**

```
*const API_ENDPOINT: &str = "https://api.openai.com/v1/completions";
const MODEL: &str = "text-davinci-003";
const MODEL_VERSION: &str = "1";

pub async fn prompt(&self, message: &String) -> OpenAIResult {
    // Build the request body with the prompt, model, and temperature
    let request_body = json!({
        "prompt": message,
        "model": MODEL,
        "temperature": 0,
        "max_tokens": 7,
    });

    // Build the request URL with the model and version
    let url = format!("{}/models/{}/versions/{}", API_ENDPOINT, MODEL, MODEL_VERSION);

    // Convert the URL to a `Url` type and send the request
    let url: reqwest::Url = url.parse()?;
    let client = reqwest::Client::new();
    let response: serde_json::Value = client.post(url)
        .header("Content-Type", "application/json")
        .header("Authorization", format!("Bearer {}", self.api_key))
        .json(&request_body)
        .send()
        .await?
        .json()
        .await?;

    // Return the response
    Ok(response)
}*
```

*干得好，非常接近！对应该改变什么的解释也很棒。(这是一个将命令或代码片段从一种语言或工具映射到另一种语言或工具的好工具。)让它工作的一个编辑是实际替换所请求的 URL。因为模型仍然坚持使用错误的链接。我们应该使用下面的 URL。*

```
*let url = String::from(API_ENDPOINT);*
```

*修复并重新编译后，输出如下:*

```
*> OPENAI_API_KEY="<your-key>" cargo run -- "Hello, Davinci"

Object {"choices": 
    Array [Object {
        "finish_reason": String("length"), 
        "index": Number(0), 
        "logprobs": Null, 
        "text": String("\n\nHello! How can I")}
     ], 
     "created": Number(1670342922), 
     "id": String("cmpl-6KUmowybKJ9NVIboliVpD2yaPeIv7"), 
     "model": String("text-davinci-003"), 
     "object": String("text_completion"), 
     "usage": Object {
         "completion_tokens": Number(7), 
         "prompt_tokens": Number(5), 
         "total_tokens": Number(12)
     }
}*
```

> *有用！🎉*

*现在，是时候做一些重构来使代码更干净，并用 CLI 选项替换硬编码的参数了。而且，我们有一个助手随时准备帮助我们…*

# *四。清理*

****Me:*** *在下面的代码中，去掉* `*Client::authenticate*` *方法。创建一个新的静态方法* `*Client::new*` *，该方法实现认证逻辑并返回一个* `*Result*` *对象，该对象包含一个新创建的客户端(如果带有密钥的环境变量存在)，否则返回 error。同样，重构* `*prompt*` *方法以接受* `*request_body*` *作为参数。请不要在回答中包含任何文本，只包含更新后的代码片段。**

*`[copy-pasted the so far created code as the previously started conversation was lost]`*

****ChatGPT:****

```
*pub struct Client {
    api_key: String,
}

impl Client {
    // Implement the new method
    pub fn new() -> Result<Client, Box<dyn std::error::Error>> {
        // Read the API key from environment variables
        let api_key = env::var("OPENAI_API_KEY")?;
        Ok(Client{ api_key })
    }

    // Implement the prompt method
    pub async fn prompt(&self, request_body: &serde_json::Value) -> OpenAIResult {
        // Build the request URL with the model and version
        let url = String::from(API_ENDPOINT);

        // Convert the URL to a `Url` type and send the request
        let url: reqwest::Url = url.parse()?;
        let client = reqwest::Client::new();
        let response: serde_json::Value = client.post(url)
            .header("Content-Type", "application/json")
            .header("Authorization", format!("Bearer {}", self.api_key))
            .json(request_body)
            .send()
            .await?
            .json()
            .await?;

        // Return the response
        Ok(response)
    }
}*
```

****我:*** *太棒了！现在，创建一个新的异步 main 函数，该函数从 CLI 读取请求体值，并使用客户端发出请求。如果缺少任何参数，则终止程序。**

***ChatGPT:***

```
*use structopt::StructOpt;

#[derive(StructOpt, Debug)]
struct Cli {
    // The prompt to generate text for
    #[structopt(short = "p", long = "prompt")]
    prompt: String,

    // The model to use for text generation
    #[structopt(short = "m", long = "model")]
    model: String,

    // The temperature to use for text generation
    #[structopt(short = "t", long = "temperature")]
    temperature: f64,

    // The maximum number of tokens to generate
    #[structopt(short = "x", long = "max_tokens")]
    max_tokens: usize,
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Parse CLI arguments
    let args = Cli::from_args();

    // Create a new client instance
    let client = Client::new()?;

    // Build the request body
    let request_body = json!({
        "prompt": &args.prompt,
        "model": &args.model,
        "temperature": args.temperature,
        "max_tokens": args.max_tokens,
    });

    // Generate text using the prompt
    let response = client.prompt(&request_body).await?;

    // Print the response
    println!("{}", response);

    Ok(())
}*
```

****我:*** *我应该用什么版本的* `*structopt*` *板条箱才能让上面的片段工作？**

****ChatGPT:****你应该使用最新版本的* `*structopt*` *机箱。为此，您需要更新您的* `*Cargo.toml*` *文件中的版本号，如下所示:**

```
*[dependencies]
structopt = "^0.3.12"*
```

**这将使用与* `*0.3.12*` *版本兼容的* `*structopt*` *板条箱的最新版本。**

**或者，您可以使用最新版本的* `*structopt*` *板条箱，无需指定版本号，如下:**

```
*[dependencies]
structopt = "*"*
```

**这将使用最新版本的* `*structopt*` *板条箱可用在* [*板条箱. io*](https://crates.io/) *上。但是，如果在新版本中引入了重大更改，这可能会导致兼容性问题。**

*好的，这个看起来比上一个版本好一点。我特别喜欢关于使用哪种板条箱以及有哪些选项的解释。*

*现在，让我们检查它是否仍然工作。*

```
*> cargo run -- -p 'hello, openai!' -m text-davinci-003 -t 0 -x 12

{
    "choices": [{
        "finish_reason":"stop",
        "index":0,"logprobs":null,
        "text":"\n\nHi there!"
    }],
    "created":1670360311,
    "id":"cmpl-6KZJHJ8vW8CB5LyTE0C2hfMv7DY6z",
    "model":"text-davinci-003",
    "object":"text_completion",
    "usage":{"completion_tokens":5,"prompt_tokens":5,"total_tokens":10}
}*
```

*太棒了。*

# *动词 （verb 的缩写）结尾部分*

*人工智能领域的发展速度令人印象深刻。每个月(甚至每周！)带来了新的发展，比如强大的新模型的发布，这些模型可以有效地玩[复杂的棋盘游戏](https://techcrunch.com/2022/12/01/now-ai-can-outmaneuver-you-at-both-stratego-and-diplomacy/?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAEZvoiFQiW6I3q77eFPrthoj02eDMH2xoPfD15tf8SyGBzLhkV0AOle55NqPyFZbliqVC0NI5N_LW8AB9LshgIaE6-8TFoONqA0Ze39TiWt5hxw0g6_IlrY3QGPNy2FFywXhS0o1-MxahhxGZ0383QiebjVt1jr7ylVG39XYrS0q)，或者推动[新算法](https://www.nature.com/articles/s41586-022-05172-4)的发现。近年来取得的进展确实令人瞩目，想到这些进展的潜在应用令人兴奋。随着人工智能研究的不断发展，我们可以期待在未来看到更多令人兴奋的发展。*

*当然，这些进步也有局限性。一个最重要的限制是，有时这些模型产生的答案非常*合理*，但不完全正确。它们可能需要对提示进行(微小的)编辑或重新措辞。此外，虽然 Rust 等语言拥有编译器和 linters 等工具，可以验证代码的正确性并检查漏洞，但将这些检查应用于其他用途并不容易，如[解释科学](https://www.technologyreview.com/2022/11/18/1063487/meta-large-language-model-ai-only-survived-three-days-gpt-3-science/)或[回答 StackOverflow 上的编码问题](https://www.theverge.com/2022/12/5/23493932/chatgpt-ai-generated-answers-temporarily-banned-stack-overflow-llms-dangers)。有时，大型语言模型(LLM)容易变得[非常自信，但自相矛盾，而且大错特错](https://twitter.com/itstimconnors/status/1599544717943123969?s=20&t=Hwc370nJT2eQH66Wrpwjkg)。*

*然而，这些工具和模型在每次迭代中都在不断改进。很难相信，就在几年前，做我们在这篇文章中讨论的事情似乎还是纯粹的科幻小说。看到人工智能研究将在未来把我们带到哪里，令人兴奋。*

*`[Parts of this post were edited with ChatGPT's assistance ;)]`*

*你喜欢这篇文章吗？不要犹豫，给它鼓掌！还有，你可以在 [*Twitter*](https://twitter.com/devforfu) *或*[*LinkedIn*](https://www.linkedin.com/in/ilia-zaitsev)*上找到我，如果你有任何问题，或者访问* [*我的网站*](https://iliazaitsev.me/) *查看我在互联网上的其他帖子。**