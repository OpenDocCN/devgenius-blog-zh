# 用一个函数将 API 请求转换成 Mongo 过滤器——标准化您的 API 查询。

> 原文：<https://blog.devgenius.io/mongo-filter-generator-safe-client-side-filtering-with-mongoose-f9968a410e18?source=collection_archive---------5----------------------->

大家好！感谢您的加入！

今天，我想分享一个我过去谈论过的库的更新——Mongo Filter Generator——版本 0 . 3 . 0[版本 0.3.3 发布时！】正式发售。

自从上次我有机会分享这个库以来，我已经引入了一些新的选项来进一步扩展 API 的过滤功能！

![](img/f1cdb08387dd55818e42e5f019fa5ec5.png)

# MFG 将服务器请求转换为 Mongo 过滤器

`mongo-filter-generator`的主要目标是 ***标准化*** 客户端如何从使用 mongose/MongoDB 的服务器请求数据。标准化使事情变得简单和可预测！

MFG 以一种超级简单的方式做到了这一点。

通过一个快捷函数发送传入请求，返回 Mongo `filter`！将那个`filter`传入你喜欢的`model.find()`方法，正确的文档就找到了。

不再需要编写代码来处理每种过滤可能性的每种情况。在 3 行中，您可以用`mongo-filter-generator`将*过滤后的*和*分页后的*数据返回给客户端。

```
const resolver = {
  getUsers: async (_, args) => {
    const { filter } = GenerateMongo({fieldFilters: args});
    const users = User.findAndPaginate(filter);
    return users;
  }
}
```

# 具有提供的类型的标准化查询

该库为 restful 和 graphql APIs 提供了类型(SDL 和类型脚本！)可以应用于预期的请求，服务器端。这意味着无论查询什么资源，服务器都可以期待相同的形状查询。

这些类型被称为`fieldFilters`——有几个可供选择，如`StringFieldFilter`、`IntFieldFilter`、`BooleanFieldFilter`、`DateFieldFilter`和`StringArrayFieldFilter`。

使用字段过滤器定义输入类型(GraphQL)或请求正文类型定义(REST)。

将字段过滤器应用到 GraphQL 服务器很简单。

```
type User {
  _id: ID!
  name: String!
  age: Int!
  married: Boolean!
  favFoods: [String!]!
  createdAt: DateTime!
}input GetUsersInput {
  _id: StringFieldFilter
  name: [StringFieldFilter]
  age: IntFieldFilter
  married: BooleanFieldFilter
  favFoods: [StringArrayFieldFilter]
  createdAt: DateFieldFilter
}type GetUsersResponse {
  data: [User!]!
}type Query {
  getUsers(getUsersInput: GetUsersInput): GetUsersResponse!
}
```

请放心，您可以以多种方式使用字段过滤器，包括在数组或嵌套对象中。

上面使用的字段过滤器和标量由 MFG 包提供，存储库中的 README.md 有关于如何向应用程序添加类型(包括标量)的简单说明。我不打算在这里看完整的教程，因为这是一个更高层次的制造视图。

# 扩展客户端的功能

因为字段过滤器不仅仅是基本的输入类型，它们扩展了客户端查找、过滤和分页请求的能力。过滤器提供了标准化的输入，允许客户端决定应该如何查询数据库。

## 字段过滤器中的附加选项

除了查询本身之外，字段筛选器还为客户端提供了其他筛选选项。选项的范围取决于过滤器的类型，这使得每个过滤器对于客户端来说都是强大且唯一的。

下面的请求显示了客户端如何使用提供的过滤器来指定查询。

例如，下面的变量/请求体可用于查询用户最喜欢的食物有 ***比萨饼或鸡翅—*—**以及与 ***jim 或 john 部分匹配的名称(regex)。***

您可以在示例中看到各种查询选项。

```
{
  "getUsersInput": {
    "favFoods": [
      {
        "arrayOptions": "IN",
        "filterBy": "REGEX",
        "string": ["pizza"],
        "operator": "OR",
        "groups": ["favFoodsGroup.and"]
      },
      {
        "arrayOptions": "IN",
        "filterBy": "REGEX",
        "string": ["wings"],
        "operator": "OR",
        "groups": ["favFoodsGroup.and"]
      }
    ],
    "name": [
      {
        "filterBy": "REGEX",
        "operator": "OR",
        "string": "jim",
        "groups": ["namesGroup.or"]
      },
      {
        "filterBy": "REGEX",
        "operator": "OR",
        "string": "john",
        "groups": ["namesGroup.or"]
      }
    ]
  }
}
```

`arrayOptions`、`filterBy`、`operator`和`groups`都是我们上面应用的字段过滤类型自动提供的选项。

# 转换请求 GenerateMongo 函数

类型化允许您轻松地扩展过滤选项，并为客户端标准化查询，但除了形成预期的请求之外，别无他用。这就是所提供的方法有用的地方。

由于类型应该符合特定的形状，我们可以对数据做更多的事情，包括使用`GenerateMongo`方法将字段过滤器转换成 MongoDB 过滤器。

只需将请求体或 graphql 输入传递给`GenerateMongo`方法，解析嵌套字段过滤器的请求对象，这些过滤器作为 MongoDB 过滤器返回。

```
const {filter, options} = GenerateMongo({
    fieldFilters: args.getUsersInput 
  });
```

如果您对我们上面的查询中解析的`filter`对象感兴趣，请查看一下！

```
{
  "$or": [
    {
      "$or": [
        {
          "name": {...}
        },
        {
          "name": {...}
        }
      ]
    }
  ],
  "$and": [
    {
      "$or": [
        {
          "favFoods": {
            "$in": [{...}]
          }
        },
        {
          "favFoods": {
            "$in": [{...}]
          }
        }
      ]
    }
  ]
}
```

把`filter`对象传给 Mongo 就这样！

```
const users = User.find(filter);
```

或者，将它传递给同样提供的 Find 和 Paginate 方法。查看文档了解更多相关信息！

# 0.3.0 版的功能

嘿，谢谢你坚持到现在！我希望您可以看到 MFG 库可以帮助标准化和扩展使用 Mongoose 的 API 的客户端过滤选项！除了使客户机请求过滤数据标准化之外，该库还处理将请求转换成 Mongo 过滤器的繁重工作。

版本 0.3.0 刚刚推出，包含了一些新特性，允许客户端在查询 API 时拥有更大的能力。

## 字段过滤运算符

像`and`和`or`这样的操作符已经从过滤器配置转移到现场过滤器本身。这允许每个过滤器选择它是否应该作为一个`$and`操作在查询中被要求，或者对于一个`$or`操作是可选的。

## 日期字段过滤器

新的日期字段筛选器允许您搜索符合日期限制的文档。

```
type DateFieldFilter = {
  date: Date;
  filterBy: 'EQ' | 'NE' | 'LT' | 'GT' | 'LTE' | 'GTE';
  operator?: 'AND' | 'OR';
  groups?: string[];
};
```

## 组

字段过滤器中的`groups`属性允许客户端混合匹配条件和操作符，以进行更广泛的过滤。

有了组，我们现在可以在`and/or`组中组合属性，就像这样…

我们可以找到这样的用户:

`(name === 'Nick' || age === 22) && (married === false && favoriteFood === "Pizza")`

注意分组——姓名和年龄现在组合在一个`or`组中，而已婚和`favoriteFoods`在一个`and`组中。

现在，客户端可以指定这些自定义分组，您不再需要编写自定义代码来处理客户端可能请求的各种选项。

您可以对每个查询应用任意多个组，因为您可以为每个组选择自定义名称。这意味着您可以编写一个字段筛选器并将其应用于多个组。

这个库非常有用！我非常兴奋能够有方法将过滤功能扩展到客户端，同时保持简单易用的服务器端代码。

如果你喜欢它的发展方向，请随意查看这个库，用 GitHub 注册表中的 NPM 安装它，并告诉我你的想法。下面是链接！

[](https://github.com/The-Devoyage/mongo-filter-generator/packages/1228449) [## 软件包 mongo-filter-generator

### 使用 Typescript 和 GraphQL 类型为 Mongoose 查找、过滤和分页插件。-mongo-filter-generator 包…

github.com](https://github.com/The-Devoyage/mongo-filter-generator/packages/1228449) 

如果你想支持我在这个项目中添加更多的功能，请随时在这里的评论中或 Github 中提出请求。

感谢您的入住！希望这有助于塑造你的下一个 API！