# 使用 node.js 构建 Web-Scrapper

> 原文：<https://blog.devgenius.io/build-web-scrapper-using-node-js-40438757661d?source=collection_archive---------6----------------------->

![](img/3ecc370d5c6dc4e61df7b125cdfde9d5.png)

你好，太棒了😎今天的读者在这篇文章中，我将教你如何使用 Node.js 构建一个简单而神奇的 web scraper。

因此，如果没有，首先设置您的节点开发环境。如果你已经得到了祝贺🎉你已经准备好进入下一步了…

进入技术层面👨‍💻东西…

首先，我们需要一些包装。

*   [Axios](https://www.npmjs.com/package/axios)
*   [再见](https://www.npmjs.com/package/cheerio)

看完这些包的名字后，这个问题肯定会让你想到这些包是用来做什么的，所以我会给你简单而准确的定义。

axios——浏览器和 node.js 的基于 Promise 的 HTTP 客户端

cheerio——专为服务器设计的快速、灵活和精简的核心 jQuery 实现。

所以，让我们从真正的交易开始…

打开你的任何首选编辑器/IDE 我使用 Visual Studio 代码，因为它很容易使用，这就是为什么…

在编辑器中打开一个终端，输入命令-

`npm init`

之后，您将看到一个 package.json 文件将被创建，它将保存关于该包及其版本的所有配置，我不会在这个包文件中做太深入的讨论，那是另一篇文章的内容。

打开 Package.json 文件，您会看到一个带花括号的脚本对象，如下所示

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

在脚本对象中，将对象更改为启动对象，如下所示

```
"scripts": {
    "start": "nodemon index.js"
  }
```

看完这个你会问我为什么加这个，会发生什么？

因此，添加这个文件后，您可以运行 index.js 文件

如果你发现有 nodemon 错误，试着先安装它，然后试着运行

```
npm i nodemon
```

然后创建 javascript 文件 index.js，让我们开始编码。

首先安装软件包。Axios 和 cheerio，使用以下命令:-

```
npm i axiosnpm i cheerio
```

安装软件包后，首先使用 const 导入软件包

```
const axios = require('axios')
const cheerio = require('cheerio')
```

然后创建一个包含你想要抓取的网站链接的变量名 url，我在本教程中使用的是 [The Guardians news](https://www.theguardian.com/uk) 。

使用 Axios，我们从 URL 获取数据。

```
const url ='https://www.theguardian.com/uk'
axios(url)
    .then(response =>{
        const html = response.data
        const ele =cheerio.load(html)
        const article =[]
```

使用 cheerio，我们操作我们想要的数据类型，我们创建了包含数据的 ele 变量和包含从 URL 获取的数据列表的数组。

我们使用包含数据的 ele 变量

```
ele('.fc-item__title', html).each(function(){
           const title = ele(this).text()
           const url = ele(this).find('a').attr('href')
            article.push({
                title,
                url
            })
        }) console.log(article)
```

首先，访问网站，因为你应该知道你想要什么类型的数据，所以你会看到文章列表，选择任何文章，检查元素，并从开发工具中找到 div 名称。

虽然我终于找到了 div 的名字。你可以在代码中看到。fc-item__title，我们使用每个函数来迭代数据集合。

你可以看到两个变量，其中包含标题和 URL，在这一行之后，你可以看到我们在文章中使用的 push 函数。因此，我们将标题和 URL 推送到包含提取数据的文章。在底部，您会看到输出数据的 console.log()。

完整的代码，如果你坚持❤

```
const axios = require('axios')
const cheerio = require('cheerio')const url ='https://www.theguardian.com/uk'
axios(url)
    .then(response =>{
        const html = response.data
        const ele =cheerio.load(html)
        const article =[]
 ele('.fc-item__title', html).each(function(){
           const title = ele(this).text()
           const url = ele(this).find('a').attr('href')
            article.push({
                title,
                url
            })
        }) console.log(article) }).catch(err=> console.log(err) )
```

希望这篇文章对你有所帮助，如果对你有帮助，请在评论框中发表你的意见，也请关注并点赞👍