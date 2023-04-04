# 如何在 5 分钟内用 Vue 前端设置一个节点项目

> 原文：<https://blog.devgenius.io/how-to-setup-a-node-project-with-a-vue-frontend-in-5-minutes-fd3ee3e52574?source=collection_archive---------10----------------------->

![](img/57f087dc77a88d362f035f10adae1393.png)

获取编码！

当我有了灵感时，我想尽可能快地开始构建。当我想快速完成一个项目时，我会使用 MEVN 栈(Mongo、Express、Vue、Node)。以下是我如何在 5 分钟或更短时间内设置这些项目。

## 打开终端

导航到存储项目的位置

```
mkdir myproject
cd myproject
mkdir server
touch server/app.js
vue create client
```

这就建立了“服务器/客户机”所需的整个框架

## 接下来安装服务器

初始化节点项目

```
cd server
npm init
```

安装依赖项

```
npm install express cors
```

并在您喜欢的编辑器中打开服务器目录:

在 package.json 中，更新脚本部分，以便我们可以使用`npm run dev`和`npm run start`运行服务器

```
"scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
}
```

将服务器代码添加到 index.js 文件中:

```
// app.js
const express = require('express')
const app = express()
const cors = require('cors')app.use(cors())app.get('/', (req, res) => { res.json({success: true, message: 'welcome to the api'})})app.listen(3000, () => {console.log('API Running on http://localhost:3000')})
```

## 最后设置客户端

在客户端安装 axios

```
npm install axios
```

在您喜欢的编辑器中打开客户端目录，编辑`src/main.js`文件

```
import App from './App.vue'const axios = require('axios');window.api = axios.create({baseUrl: 'http://localhost:3000'})new Vue({
    render: *h* => h(App),
}).$mount('#app')
```

你就完了。要从 Vue 中的任何组件调用 api，只需使用`api`，例如:

```
api.get('/', result => {
    console.log(result) // Will log "Welcome to the api"
})
```

## 出去做点什么吧！

## 当你在这里的时候。在推特上给我一个关注！

【https://twitter.com/andyhartnett12 