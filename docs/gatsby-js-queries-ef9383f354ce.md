# Gatsby.js —查询

> 原文：<https://blog.devgenius.io/gatsby-js-queries-ef9383f354ce?source=collection_archive---------6----------------------->

![](img/4cf87250d8e5a9b229ec6296dff563c1.png)

[国家癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 页面查询

我们可以查询网站的数据，并显示在我们的页面上。

为此，我们写道:

`gatsby-config.js`

```
module.exports = {
  plugins: [],
  siteMetadata: {
    title: `Gatsby`,
    siteUrl: `https://www.gatsbyjs.com`,
    description: `Blazing fast modern site generator for React`,
  },
}
```

`src/pages/index.js`

```
import React from "react"
import { graphql } from "gatsby"export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`export default function Home({ data }) {
  return <div>{data.site.siteMetadata.title}</div>
}
```

我们在`gatsby-config.js`中设置站点的元数据。

然后我们可以在页面中获得带有`graphql`标签的数据。

然后在页面组件中，我们从`data`道具中获取数据。

# 使用 StaticQuery 组件查询数据

我们还可以用`StaticQuery`组件查询数据。

这对于获取非页面组件的数据非常有用。

例如，我们可以写:

`gatsby-config.js`

```
module.exports = {
  plugins: [],
  siteMetadata: {
    title: `Gatsby`,
    siteUrl: `https://www.gatsbyjs.com`,
    description: `Blazing fast modern site generator for React`,
  },
}
```

`src/pages/index.js`

```
import React from "react"
import { StaticQuery, graphql } from "gatsby"const NonPageComponent = () => (
  <StaticQuery
    query={graphql`
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data
    ) => (
        <h1>
          {data.site.siteMetadata.title}
        </h1>
      )}
  />
)export default function Home() {
  return <div>
    <NonPageComponent />
  </div>
}
```

我们创建了`NonPageComponent`来从站点的元数据中获取`title`。

我们使用`StaticQuery`组件来获取数据。

然后在`render`道具中，我们有一个函数，以我们想要的方式渲染`title`。

# 使用 useStaticQuery 挂钩查询数据

另一种查询数据的方法是使用`useStaticQuery`钩子。

为此，我们写道:

`gatsby-config.js`

```
module.exports = {
  plugins: [],
  siteMetadata: {
    title: `Gatsby`,
    siteUrl: `https://www.gatsbyjs.com`,
    description: `Blazing fast modern site generator for React`,
  },
}
```

`src/pages/index.js`

```
import React from "react"
import { useStaticQuery, graphql } from "gatsby"const NonPageComponent = () => {
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  return (
    <h1>
      {data.site.siteMetadata.title}
    </h1>
  )
}export default function Home() {
  return <div>
    <NonPageComponent />
  </div>
}
```

我们用 GraphQL 查询对象调用`useStaticQuery`钩子，该对象是从`graphql`标签创建的。

它返回数据，我们可以渲染它。

# 用 GraphQL 限制

我们可以使用 GraphQL API/来限制返回结果的数量

为此，我们可以运行以下查询:

```
{
  allSitePage(limit: 3) {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

在[http://localhost:8000/_ _ graph QL](http://localhost:8000/__graphql)中，结果被限制为 3 个条目。

此外，我们可以通过运行以下命令来添加排序:

```
{
  allSitePage(sort: {fields: path, order: ASC}) {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

现在我们按照升序对响应中的`path`字段进行排序。

# 结论

我们可以使用 Gatsby 查询我们的站点元数据，并以各种方式配置查询。