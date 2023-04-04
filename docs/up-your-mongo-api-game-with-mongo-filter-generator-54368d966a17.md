# 使用 Mongo 过滤器生成器升级您的 Mongo API 游戏

> 原文：<https://blog.devgenius.io/up-your-mongo-api-game-with-mongo-filter-generator-54368d966a17?source=collection_archive---------12----------------------->

大家好，感谢加入！

今天，我想分享一个我已经工作了 8 个月，并在生产中使用的库的新版本！

Mongo Filter Generator 快速添加高级过滤和分页选项，同时标准化您的 API 请求，而无需编写定制的重复代码。

向`@the-devoyage/mongo-filter-generator v0.4.0`问好！ [Github。](https://github.com/The-Devoyage/mongo-filter-generator)

![](img/a3e28585c2541e9b4a6b45efda43d68b.png)

美国宇航局在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 您的 API 已扩展

MFG 将发送到 API 的请求标准化，同时使其易于查找、过滤和分页返回的数据。

## 转换请求

将传入的请求直接传递给`GenerateMongo`函数，以立即创建可用于原生 Mongoose 方法的 Mongo 过滤器。

```
const { filter, options } = GenerateMongo({
  fieldFilters: req.body
});

const results = await User.find(filter, options);
```

## 对响应进行分页

将上面生成的`filter`和`options`属性直接传递给提供的`findAndPaginate`方法，以快速放大 API 的功能。

```
const paginatedResponse = await User.findAndPaginate(filter, options);
```

## 有用的统计数据和历史统计数据(新！)

`findAndPaginate`方法还返回有用的数据，可以帮助您分页或了解模型的统计历史。

```
type Stats = {
  remaining: Number;
  total: Number;
  page: Number;
  cursor: Date;
  history: HistoricalStats[];
};
```

Historical Stats 允许您请求关于具有自定义指定时间范围的模型的信息，允许您使用历史统计数据轻松地创建图形和图表。

例如，您可能想计算从 3 月到 11 月每个月在 API 中创建的用户数量。

# 简单的查询

使用 MFG 允许客户机轻松地请求它需要的数据，而不必编写定制的处理程序，同时使客户机请求数据所用的格式标准化。

## 字段过滤器

只需使用提供的`fieldFilter`类型来形成发送到服务器的查询，MFG 将处理剩下的工作。

有五个内置的过滤器，允许您查询具有几乎任何类型属性的文档。

```
type IntFieldFilter = {
  filterBy: "EQ" | "GT" | "GTE" | "LT" | "LTE" | "NE";
  int: number;
  operator?: "AND" | "OR";
  groups: string[];
};type StringFieldFilter = {
  filterBy: "MATCH" | "REGEX" | "OBJECTID";
  string: string;
  operator?: "AND" | "OR";
  groups: string[];
};type BooleanFieldFilter = {
  filterBy: "EQ" | "NE";
  bool: Boolean;
  operator?: "AND" | "OR";
  groups: string[];
};type DateFieldFilter = {
  date: Date;
  filterBy: "EQ" | "NE" | "LT" | "GT" | "LTE" | "GTE";
  operator?: "AND" | "OR";
  groups: string[];
};type StringArrayFieldFilter = {
  filterBy: "MATCH" | "REGEX" | "OBJECTID";
  string: string[];
  arrayOptions: "IN" | "NIN";
  operator?: "AND" | "OR";
  groups: string[];
};
```

MFG 可以解析对象或字段过滤器数组，允许您根据需要筛选数据。

## 形成请求

只需使用与您请求的文档相同的形状发送请求。

该文档:

```
interface User {
  _id: string;
  name: string;
  age: number;
  active: boolean;
}
```

查找姓名中包含字母`jo`的活跃用户:

```
const requestBody = {
  name: { string: "jo", filterBy: "REGEX" },
  active: { bool: true, filterBy: "MATCH" }
}
```

查找年龄大于 20 岁的用户:

```
const requestBody = {
  age: { int: 20, filterBy: "GT" }
}
```

将请求传递给`GenerateMongo`并使用您认为合适的过滤器。

```
const { filter } = GenerateMongo({
  fieldFilters: requestBody
})const users = await User.find(filter)
```

# 签出存储库

存储库有文档向您展示高级特性，如`and/or`过滤、分组条件、字段规则等等。

[](https://github.com/The-Devoyage/mongo-filter-generator) [## GitHub-The-devo yage/mongo-filter-generator:为 Mongoose 提供查找、过滤和分页插件

### 只需几行代码就可以轻松地在 API 中添加查找、过滤和分页功能。Mongo 过滤器生成器…

github.com](https://github.com/The-Devoyage/mongo-filter-generator) 

感谢支持，如果你认为这很酷，请点击明星，或者如果你发现有什么要补充的，请做出贡献。