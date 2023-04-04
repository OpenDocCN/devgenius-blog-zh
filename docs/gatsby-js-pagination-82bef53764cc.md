# Gatsby.js —分页

> 原文：<https://blog.devgenius.io/gatsby-js-pagination-82bef53764cc?source=collection_archive---------3----------------------->

![](img/9efde8f2b707e981546e71136c53afab.png)

照片由[真诚媒体](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 页码

我们可以用 Gatsby 为我们的页面添加分页。

为此，我们写道:

`gatsby-config.js`

```
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ],
}
```

`gatsby-node.js`

```
const path = require("path")
const { createFilePath } = require("gatsby-source-filesystem")
exports.createPages = async ({ graphql, actions, reporter }) => {
  const { createPage } = actions
  const result = await graphql(
    `
      {
        allMarkdownRemark(
          sort: { fields: [frontmatter___date], order: DESC }
          limit: 1000
        ) {
          edges {
            node {
              fields {
                slug
              }
            }
          }
        }
      }
    `
  )
  if (result.errors) {
    reporter.panicOnBuild(`Error while running GraphQL query.`)
    return
  }const posts = result.data.allMarkdownRemark.edges
  const postsPerPage = 6
  const numPages = Math.ceil(posts.length / postsPerPage)
  Array.from({ length: numPages }).forEach((_, i) => {
    createPage({
      path: i === 0 ? `/blog` : `/blog/${i + 1}`,
      component: path.resolve("./src/templates/blog-list-template.js"),
      context: {
        limit: postsPerPage,
        skip: i * postsPerPage,
        numPages,
        currentPage: i + 1,
      },
    })
  })
}
exports.onCreateNode = ({ node, actions, getNode }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const value = createFilePath({ node, getNode })
    createNodeField({
      name: `slug`,
      node,
      value,
    })
  }
}
```

`src/templates/blog-list-template.js`

```
import React from "react"
import { graphql } from "gatsby"export default class BlogList extends React.Component {
  render() {
    const posts = this.props.data.allMarkdownRemark.edges
    return (
      <div>
        {posts.map(({ node }) => {
          const title = node.frontmatter.title || node.fields.slug
          return <div key={node.fields.slug}>{title}</div>
        })}
      </div>
    )
  }
}
export const blogListQuery = graphql`
  query blogListQuery($skip: Int!, $limit: Int!) {
    allMarkdownRemark(
      sort: { fields: [frontmatter___date], order: DESC }
      limit: $limit
      skip: $skip
    ) {
      edges {
        node {
          fields {
            slug
          }
          frontmatter {
            title
          }
        }
      }
    }
  }
`
```

我们添加了`gatsby-source-filesystem`插件来从文件系统中读取 Markdown 文件。

然后我们使用`gatsby-transformer-remark`插件将 Markdown 文件解析成 HTML。

`gatsby-node.js`文件通过 GraphQL 查询获取 Markdown 帖子。

然后创建一个长度为页数的数组。

然后我们用`forEach`遍历它们，并在回调中调用`createPage`来传递`blog-list-template`文件的路径以及上下文中的其他数据。

`context`属性有更多我们传递到`blog-list-template`文件中的数据。

它们包括`postsPerPage`、要跳过的项目数、`numPages`即页数，以及`currentPage`。

在`blog-list-template.js`文件中，我们用`skip`和`limit`参数生成`blogListQuery`以从给定页面中获取项目。

在`BlogList`组件中，我们调用`posts`上的`map`来呈现文章标题。

我们从`src/content`文件夹中读取降价文件。

然后当我们去[http://localhost:8000/blog](http://localhost:8000/blog)的时候，我们看到的是第一页。

[http://localhost:8000/blog](http://localhost:8000/blog/2)有第 2 页等等。

# 结论

我们可以用 Gatsby 轻松添加分页。