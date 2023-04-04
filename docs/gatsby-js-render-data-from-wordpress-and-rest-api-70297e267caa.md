# Gatsby.js —从 WordPress 和 REST API 呈现数据

> 原文：<https://blog.devgenius.io/gatsby-js-render-data-from-wordpress-and-rest-api-70297e267caa?source=collection_archive---------2----------------------->

![](img/e289d9e9928f4e30c5a8cae1e116578c.png)

由 [Alexander Abero](https://unsplash.com/@alexabero?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 从 WordPress 呈现内容

我们可以在 Gatsby 站点上呈现 WordPress 的内容。

为此，我们通过运行以下命令来安装`gatsby-source-wordpress`插件:

```
npm install gatsby-source-wordpress
```

然后，我们将以下代码添加到 Gatsby 项目中:

`gatsby-config.js`

```
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        baseUrl: `wpexample.com`,
        protocol: `https`,
        hostingWPCOM: false,
        useACF: false
      }
    },
  ]
}
```

`baseUrl`应该被设置为 WordPress 站点的 URL。

`hostingWPCOM`设置网站是由 wordpress.com 托管还是自托管。

`useACF`设置网站是否使用高级自定义字段插件。

`gatsby-node.js`

```
const path = require(`path`)
const { slash } = require(`gatsby-core-utils`)
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)
  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      path: edge.node.slug,
      component: slash(postTemplate),
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

我们通过 GraphQL 查询从 WordPress API 获取数据。

然后我们调用`createPage`来创建页面。

我们将`path`设置为 slug。

`component`道具设置为模板路径，即`src/templates/post.js`

而`context`有多余的数据。

`src/template/post.js`

```
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"
class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost
    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}
Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}
export default Post
export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

`post.js`有模板组件。

我们用`post.content`渲染内容。

而`post.title`有标题。

# 从外部来源提取数据并创建没有 GraphQL 的页面

我们可以直接从 API 获取数据，并根据数据创建页面。

为此，我们写道:

`gatsby-config.js`

```
module.exports = {
  plugins: []
}
```

`gatsby-node.js`

```
const axios = require("axios")
const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)
const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["mew", "ditto", "squirtle"])
  createPage({
    path: `/pokemon`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

`all-pokemon.js`

```
import React from "react"
export default function AllPokemon({ pageContext: { allPokemon } }) {
  return (
    <div>
      <ul>
        {allPokemon.map(pokemon => (
          <li key={pokemon.id}>
            <img src={pokemon.sprites.front_default} alt={pokemon.name} />
            <p>{pokemon.name}</p>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

在`gatsby-node.js`中，我们从口袋妖怪 API 中获取数据。

然后，我们通过从 API 获取数据来创建`createPages`函数。

然后我们对`getPokemonData`返回的承诺的解析值调用`createPage`。

我们为页面设置了`path` ,设置了用于呈现数据的`component`,设置了用于呈现数据的`context`。

然后在`src/templates/all-pokemon.js`中，我们呈现从 API 获得的数据。

# 结论

我们可以直接用 Gatsby 从 WordPress 或 REST API 呈现数据。