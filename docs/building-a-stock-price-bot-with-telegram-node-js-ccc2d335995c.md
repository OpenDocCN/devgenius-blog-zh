# 用 Telegram & Node.js 构建股价机器人

> 原文：<https://blog.devgenius.io/building-a-stock-price-bot-with-telegram-node-js-ccc2d335995c?source=collection_archive---------2----------------------->

![](img/8bea1cdb82992452ebd0c2dd9b39d1fa.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本教程中，我们将创建一个简单的电报机器人，检查用户输入的股票代码/股票代号(如 AAPL，谷歌，FB，TSLA，AMZN 等。)，然后发送今天的收盘价通知用户。

# **工具&要求**

我们需要为这个循序渐进的教程准备必要的 API 键和工具:

*   **Node.js** & **NPM** 安装
*   **电报账户**
*   **电报 bot** (您可以请求 [**Bot 支持**](http://t.me/BotSupport) 创建 bot &获取 API 密钥)
*   **AlphaVantage API Key** (此处 可要求 [**)**](https://www.alphavantage.co/support/#api-key)
*   安装了 VScode (或者任何你喜欢的 IDE)

# ***环境设置***

我将保持它的简单，这样它将很容易被记住和被遵循。

1.  打开终端(您可以使用 VScode 终端或简单地使用 Windows 命令提示符或 Mac 终端)
2.  创建文件夹 **/StockPrice，**使用命令 **cd /StockPrice** 将目录更改为 **/StockPrice**
3.  使用命令 **node -v** 检查节点版本
4.  安装 NPM 软件包如下:
    **npm 安装 telegraf 节点-获取**
5.  在 **/StockPrice** 中创建文件 **app.js**

# **导入包，API 测试，&代码**

1.  编写导入包行并创建继承 **Telegraf、& node-fetch** 对象的常量变量。对于 API 密钥，用**电报机器人支持** & **AlphaVantage** 中的 **API 密钥**替换下面两个粗体文本

> **app.js**

```
"use strict";const { Telegraf } = require("telegraf");
const fetch = require("node-fetch")const bot = new Telegraf(***'insert bot API token here***');
let url ='https://www.alphavantage.co/query?function=*TIME_SERIES_MONTHLY*&interval=5min&apikey=***demo***';
```

因为只需要今天的收盘价，所以可以用 ***函数=TIME_SERIES_MONTHLY*** 来简化 API 的响应。JSON 回复的第一行显示了本月的今日数据。
此外，您可以打开文档来探索 AlphaVantage 可以做什么:
[**AlphaVantage 文档**](https://www.alphavantage.co/documentation/)

**2。**现在通过编写 Telegraf 的机器人功能命令 **/start** 使机器人互动:

> **app.js**

```
bot.command("start", (ctx) => {
   console.log(ctx.from);
   bot.telegram.sendMessage(
      ctx.chat.id,
      "Hello from your own StockPrice bot.
      Please input your desired ticker symbol, for example .GOOG or
      .AAPL."
   );
});
```

*说明:*

```
bot.command("start", (ctx) => {
```

我们创建函数 **bot.command("start")** 来启动 bot 响应，同时我们开始与 bot 聊天。

```
console.log(ctx.from);
   bot.telegram.sendMessage(
      ctx.chat.id,
      "Hello from your own StockPrice bot.
      Please input your desired ticker symbol, for example .GOOG or
      .AAPL."
   );
```

机器人将参考用户的 **ctx.chat.id** 向用户发送消息，因此机器人被正确识别出谁是发送者。消息可以是任何类似欢迎用户使用机器人的内容。

**3。为了检索我们的输入，机器人需要听到我们的输入。因此，我们创建了 **bot.hears()** 函数，并放置了我们的正则表达式，以便它们能够识别股票代码/股票代号。**

> **app.js**

```
bot.hears(/([.])\w{0,4}/, async (ctx) => {let final_uri = url + `&symbol=${ctx.message.text.substring(1)}`;
let today = new Date(ctx.message.date * 1000);let formattedDate =
    today.getFullYear() + "-" +
    (today.getMonth() + 1) + "-" +
    (today.getDate() - 1);
```

*说明:*

```
bot.hears(/([.])\w{0,4}/, async (ctx) => {
```

我们用两个参数创建函数**bot . heares()**，第一个参数是股票代码的 **regex** ，第二个参数是**函数**，它是关于我们在接收到输入后下一步要做什么。

```
let final_uri = url + `&symbol=${ctx.message.text.substring(1)}`;
```

我们将 AlphaVantage API 的 **url** 和用户输入的股票代码(谷歌、AAPL 等)连接/合并到变量 **final_uri** 中。

```
let today = new Date(ctx.message.date * 1000);
```

我们从由用户发送的输入的纪元时间组成的 **ctx.message.date** 创建变量 **today** 。并将其转换成 Javascript 的 Date 对象。我们需要将其乘以 1000，因为 Javascript 内部使用毫秒，而普通的 Epoch / UNIX 时间戳通常以秒为单位。

```
let formattedDate =
    today.getFullYear() + "-" +
    (today.getMonth() + 1) + "-" +
    (today.getDate() - 1);
```

我们将匹配 AlphaVantage API 响应格式的日期合并到 **formattedDate 中。**

需要 **today.getMonth() + 1** 是因为月份是作为索引读取的，而 **today.getDate() - 1** 是为了正确适应时区，其中 AlphaVantage API 的默认时区是使用东部时区。我是基于东南亚，所以它需要把减法 1。

**4。**我们将获取包含用户输入(GOOG，AAPL)的 **final_uri** 中的 AlphaVantage API URL，然后捕捉**响应**，用 **response.json()** 将其转换为 JSON 格式，并在输入的股票代码中只选择今天的收盘价。

> **app.js**

```
fetch(final_uri)
    .then((response) => response.json())
    .then((data) => {
         ctx.reply( "The stock price of " +
         data["Meta Data"]["2\. Symbol"] +
         " as for today is " +
         parseFloat(
               data["Monthly Time Series"]
                   [formattedDate]
                   ["4\. close"]) +
         " USD"
         );
     });
});
```

*说明:*

```
fetch(final_uri)
```

使用 **final_uri** 完整 URL 获取 AlphaVantage API

```
.then((response) => response.json())
```

将成功响应转换为 JSON 格式

```
.then((data) => {
         ctx.reply( "The stock price of " +
         data["Meta Data"]["2\. Symbol"] +
         " as for today is " +
         parseFloat(
               data["Monthly Time Series"]
                   [formattedDate]
                   ["4\. close"]) +
         " USD"
         );
     });
```

转换后的 JSON 响应，即**数据**将仅被选择用于今天的价格。

**数据["元数据"]["2。符号“]** 代表股票符号，而**数据[“月度时间序列”][formattedDate]["4。close"]** 代表我们之前谈过的前**格式化日期**过滤的收盘价。

然后我们把需要回复的消息的单词放进去，然后 **ctx.reply()** 函数把消息回复给用户。

**5。**最后，使用 **bot.launch()** 启动 bot 并保存文件:

```
bot.launch();
```

# 完整的剧本

> **app.js**

# 执行脚本并测试机器人

1.  在终端的 **/StockPrice** 文件夹中，使用命令 **node app.js** 执行脚本
2.  打开电报客户端，搜索你已经创建的机器人。例如，如果您先前创建了名为 **TestStockPriceBot** 的 Bot，您将被搜索到该名称，或者您可以使用 t.me/<your bot name>:
    **t.me/TestStockPriceBot**来访问该 bot
3.  点击电报客户端下方的**开始**按钮开始聊天。
4.  写下信息(例如。GOOG，。AAPL)，看看回答是否正确。当正确时，它将发送所发送股票代码的今天收盘价。

![](img/b9b129f80578a76f109864b7d58cc30f.png)

行动中捕获的 StockPrice 机器人

# 包扎

电报机器人可以用来从任何地方获取信息，而不需要打开另一个应用程序。只需使用电报客户端，无需构建客户端应用程序，并根据您的要求自行构建电报机器人内的应用程序。

以下参考资料将帮助您在 Telegram bot & AlphaVantage 中构建自己的应用程序:

*   [https://www.alphavantage.co/](https://www.alphavantage.co/)
*   [https://www . section . io/engineering-education/telegram-bot-in-nodejs/](https://www.section.io/engineering-education/telegram-bot-in-nodejs/)
*   [https://www.npmjs.com/package/node-fetch](https://www.npmjs.com/package/node-fetch)