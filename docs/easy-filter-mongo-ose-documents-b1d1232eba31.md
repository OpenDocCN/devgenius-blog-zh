# 轻松过滤蒙古文(ose)文档

> 原文：<https://blog.devgenius.io/easy-filter-mongo-ose-documents-b1d1232eba31?source=collection_archive---------16----------------------->

嘿大家好！

今天，我想分享我在为 API 调用编写过滤方法时是如何使用可重用代码的！具体来说，我想分享我是如何用 mongose/Mongo 用几行代码实现一些非常高级的过滤的！

目标很简单:

***客户端应该有一个标准化和类型化的策略，从连接到 Mongo DB(使用 Mongoose)的 API 请求过滤后的文档。***

![](img/94bb5b467b1f1906f47b9e144a088211.png)

# 在几分钟内将过滤添加到 API 中！

## 文件

首先，看一个示例模式。我们可以在整篇文章中引用这一点。

```
const ProductSchema = new Schema(
  {
    name: {
      type: String,
      required: true,
      lowercase: true,
      unique: true,
    },
    rating: {
      type: Number,
      required: true,
    },
    active: {
      type: Boolean,
      default: true,
      required: true,
    },
    allergies: [
      {
        type: String,
      },
    ],
  },
  { timestamps: true }
);
```

这个模式是一个很好的例子，因为我们可以通过几种不同的类型进行过滤— `string`、`boolean`、`array`、`object id`和`number`！

## 为预期的请求创建类型

客户端将向 API 发送请求，以便接收过滤后的文档。正如我们在本文开头的目标中所讨论的，我们希望客户拥有一个标准化的、类型化的策略来请求文档。

为此，我们需要输入我们期望从客户端收到的请求。然后，我们可以与客户端共享该类型，这将在前端工作时启用自动完成。

## 安装 Mongo 过滤器生成器

首先，安装包`@the-devoyage/mongo-filter-generator`！此包提供了可用于形成请求正文的类型。

请记住——如果您发现自己正在使用该软件包，并想送上一份小礼物表示感谢，您可以通过我们的 [Basetools 付费墙来完成！](https://basetools.io/checkout/vyOL9ATx)

从您的项目的根目录，运行以下命令登录到 NPM Github 注册表，将`@the-devoyage`范围添加到您的。npmrc，并安装软件包。

```
npm login --registry=https://npm.pkg.github.comecho @the-devoyage:registry=[https://npm.pkg.github.com](https://npm.pkg.github.com) >> .npmrcnpm i @the-devoyage/mongo-filter-generator
```

## 创建请求正文类型

现在您已经安装了 MFG 包，使用它提供的`fieldFilter`类型来键入请求体。

*提示:使每个字段可选，以允许客户端对请求有更多的控制。*

```
import {
  StringFieldFilter,
  IntFieldFilter, 
  BooleanFieldFilter, 
  StringArrayFilter 
} from "@the-devoyage/mongo-filter-generator";export interface GetProducts {
  _id?: StringFieldFilter;
  name?: StringFieldFilter;
  rating?: IntFieldFilter;
  active?: BooleanFieldFilter;
  allergies?: StringArrayFilter;
}
```

## 解析过滤器并找到文档

差不多了——接下来，用从 MFG 包导出的`GenerateMongo`函数解析过滤器。这将采取过滤器，并将其转换为 Mongo 过滤器。

使用标准 Mongo 风格的滤镜，带`model.find()`。

```
import { GenerateMongo } from "@the-devoyage/mongo-filter-generator";
import { ProductModel } from "models";
import { GetProducts } from "types";app.post("/products", async (req, res) => {
  const fieldFilters: GetProducts = req.body; const { filters } = GenerateMongo(fieldFilters); const products = await ProductModel.find(filters); res.json(products);
});
```

就是这样！编码完成！您的 API 现在可以返回超级过滤的结果。

现在让我们来看一些示例查询！从客户端，我们现在可以要求一些产品。

# 五个示例查询

不要忘记与客户端共享您为请求主体创建的类型，因为它可以提供自动完成功能，这对前端团队或您自己来说非常好！

## **查找所有准确名称为“印度淡色麦酒”的产品。**

```
import { GetProducts } from 'types';// ...const query: GetProducts = {
  name: {
    string: "India Pale Ale",
    filterBy: "MATCH",
  },
};const response = await fetch("/products", {
  method: "POST",
  body: JSON.stringify(query),
});const products = await response.json();
```

## **查找部分名称为“Ale”的所有产品。**

```
import { GetProducts } from 'types';// ...const query: GetProducts = {
  name: {
    string: "Ale",
    filterBy: "REGEX",
  },
};// ...
```

## **查找评分高于数字 3 的所有产品。**

```
import { GetProducts } from 'types';// ...const query: GetProducts = {
  rating: {
    int: 3,
    filterBy: "GT",
  },
};// ...
```

## **查找对象 ID 与“12345”匹配的所有产品。**

对象 id 是字符串过滤器的一部分。在过滤之前，对象 ID `filterBy` param 会将字符串转换为服务器端的对象 ID。

```
import { GetProducts } from 'types';// ...const query: GetProducts = {
  _id: {
    string: "12345",
    filterBy: "OBJECTID",
  },
};// ...
```

## **查找排除过敏“小麦”的所有产品。**

字符串数组过滤器可以在指定的数组中查找包含或排除查询字符串的文档。

```
import { GetProducts } from 'types';// ...const query: GetProducts = {
  allergy: {
    string: "wheat",
    filterBy: "MATCH",
    arrayOptions: "NIN",
  },
};// ...
```

# 包装东西

嘿，我希望`@the-devoyage/mongo-filter-generator`包能让你的前端和服务器端代码都变得简单一点！

如果你认为会的话，送去一些爱。你的资金将直接用于我带狗狗去卖美味 IPA 的狗狗公园。是的，这是一个酷狗公园！

![](img/d10fe190e9ccc0c815835e01303dd3c6.png)

MFG 包提供的不仅仅是简单的过滤，我今天有机会向您展示了—分页选项、默认搜索选项、覆盖值、嵌套过滤和/或操作符过滤…仅举几个例子，我还会不断添加更多！

如果你想了解更多关于这个包的内容，可以看看我的文章，[即时 API 分页和使用 Mongo-Filter-Generator 过滤](https://thedevoyage.medium.com/instant-api-pagination-and-filtering-with-mongo-filter-generator-228a57182116)！

嘿！我的名字是 Nick——我是一名来自德克萨斯州达拉斯的开发人员，非常高兴有机会在 Medium 上分享我的一些作品。如果你喜欢啤酒、狗、javascript 或狗，一定要点击 ***跟随按钮*。**

谢谢，

缺口