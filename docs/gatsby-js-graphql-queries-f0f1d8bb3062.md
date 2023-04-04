# Gatsby.js — GraphQL 查询

> 原文：<https://blog.devgenius.io/gatsby-js-graphql-queries-f0f1d8bb3062?source=collection_archive---------3----------------------->

![](img/5a2940ade9caeb72efb79ac4a4d45492.png)

卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 限制

我们可以限制 GraphQL 查询结果中返回的结果数量。

例如，我们可以写:

```
{
  allMarkdownRemark(limit: 3) {
    totalCount
    edges {
      node {
        frontmatter {
          title
        }
      }
    }
  }
}
```

使用`title`字段获取前 3 个降价帖子。

# 跳跃

我们可以用`skip`参数跳过一些结果:

```
{
  allMarkdownRemark(skip: 3) {
    totalCount
    edges {
      node {
        frontmatter {
          title
        }
      }
    }
  }
}
```

# 过滤器

我们可以用`filter`参数过滤结果:

```
{
  allMarkdownRemark(filter: {frontmatter: {title: {ne: ""}}}) {
    totalCount
    edges {
      node {
        frontmatter {
          title
        }
      }
    }
  }
}
```

我们将`filtet`设置为一个对象，其`title`不等于`frontmatter`中的空字符串。

我们可以结合多个领域。

例如，我们可以写:

```
{
  allMarkdownRemark(filter: {frontmatter: {title: {ne: ""}}, wordCount: {words: {gte: 1}}}) {
    totalCount
    edges {
      node {
        frontmatter {
          title
        }
      }
    }
  }
}
```

搜索标题不等于空字符串且字数大于或等于 1 的降价帖子。

# 经营者

运营商的完整列表包括:

*   `eq`:等号的简称，必须与给定数据完全匹配
*   `ne`:不等于的简称，必须与给定数据不同
*   `regex`:正则表达式的简称，必须匹配给定的模式。注意反斜杠需要转义两次，所以`/\w+/`需要写成`"/\\\\w+/"`。
*   `glob`:global 的缩写，允许我们使用通配符`*`，作为任何非空字符串的占位符
*   `in`:数组中的简称，必须是数组的一个元素
*   `nin`:not in a array 的简称，不能是数组的元素
*   `gt`:大于的简称，必须大于给定值
*   `gte`:大于等于的简称，必须大于等于给定值
*   `lt`:小于的简称，必须小于给定值
*   `lte`:小于或等于的简称，必须小于或等于给定值
*   `elemMatch`:元素匹配的简称。我们用这个运算符找到与给定值匹配的元素。

# 分类

我们可以对结果中返回的项目进行排序。

为此，我们写道:

```
{
  allMarkdownRemark(sort: {fields: [frontmatter___date], order: ASC}) {
    totalCount
    edges {
      node {
        frontmatter {
          title
          date
        }
      }
    }
  }
}
```

添加`order`字段，让我们对`frontmatter.date`字段进行排序。

# 格式化日期

我们可以格式化我们的结果。为了格式化日期，我们写:

```
{
  allMarkdownRemark(filter: {frontmatter: {date: {ne: null}}}) {
    edges {
      node {
        frontmatter {
          title
          date(formatString: "dddd DD MMMM YYYY")
        }
      }
    }
  }
}
```

然后我们得到完整的日期，大概是:

```
{
  "data": {
    "allMarkdownRemark": {
      "edges": [
        {
          "node": {
            "frontmatter": {
              "title": "My First Post",
              "date": "Wednesday 10 July 2019"
            }
          }
        }
      ]
    }
  },
  "extensions": {}
}
```

# 结论

我们可以使用 Gatsby 的 GraphQL API 进行各种查询。