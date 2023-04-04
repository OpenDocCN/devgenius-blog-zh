# Gatsby.js —标签和帖子页面

> 原文：<https://blog.devgenius.io/gatsby-js-tags-and-post-page-a6559a2c076b?source=collection_archive---------4----------------------->

![](img/9e345be42ebc2e4df6cc5db060ad1177.png)

[Louie Martinez](https://unsplash.com/@thetalkinglens?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 博客文章的标签页面

我们可以为博客文章创建标签页。

为此，我们需要处理几个文件:

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
const _ = require("lodash")
const { createFilePath } = require(`gatsby-source-filesystem`)
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}
exports.createPages = async ({ actions, graphql, reporter }) => {
  const { createPage } = actions
  const blogPostTemplate = path.resolve("src/templates/post.js")
  const tagTemplate = path.resolve("src/templates/tags.js")
  const result = await graphql(`
    {
      postsRemark: allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 2000
      ) {
        edges {
          node {
            fields {
              slug
            }
            frontmatter {
              tags
            }
          }
        }
      }
      tagsGroup: allMarkdownRemark(limit: 2000) {
        group(field: frontmatter___tags) {
          fieldValue
        }
      }
    }
  `)
  if (result.errors) {
    reporter.panicOnBuild(`Error while running GraphQL query.`)
    return
  }
  const posts = result.data.postsRemark.edges
  posts.forEach(({ node }) => {
    const slug = node.fields.slug
    createPage({
      path: slug,
      component: blogPostTemplate,
      context: { slug },
    })
  })
  const tags = result.data.tagsGroup.group
  tags.forEach(tag => {
    createPage({
      path: `/tags/${_.kebabCase(tag.fieldValue)}/`,
      component: tagTemplate,
      context: {
        tag: tag.fieldValue,
      },
    })
  })
}
```

`src/templates/tags.js`

```
import React from "react"
import PropTypes from "prop-types"
import { Link, graphql } from "gatsby"const Tags = ({ pageContext, data }) => {
  const { tag } = pageContext
  const { edges, totalCount } = data.allMarkdownRemark
  const tagHeader = `${totalCount} post${totalCount === 1 ? "" : "s"
    } tagged with "${tag}"`
  return (
    <div>
      <h1>{tagHeader}</h1>
      <ul>
        {edges.map(({ node }) => {
          const { slug } = node.fields
          const { title } = node.frontmatter
          return (
            <li key={slug}>
              <Link to={slug}>{title}</Link>
            </li>
          )
        })}
      </ul>
      <Link to="/tags">All tags</Link>
    </div>
  )
}
Tags.propTypes = {
  pageContext: PropTypes.shape({
    tag: PropTypes.string.isRequired,
  }),
  data: PropTypes.shape({
    allMarkdownRemark: PropTypes.shape({
      totalCount: PropTypes.number.isRequired,
      edges: PropTypes.arrayOf(
        PropTypes.shape({
          node: PropTypes.shape({
            frontmatter: PropTypes.shape({
              title: PropTypes.string.isRequired,
            }),
            fields: PropTypes.shape({
              slug: PropTypes.string.isRequired,
            }),
          }),
        }).isRequired
      ),
    }),
  }),
}
export default Tags
export const pageQuery = graphql`
  query($tag: String) {
    allMarkdownRemark(
      limit: 2000
      sort: { fields: [frontmatter___date], order: DESC }
      filter: { frontmatter: { tags: { in: [$tag] } } }
    ) {
      totalCount
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

`src/templates/post.js`

```
import React from "react"
import { graphql } from "gatsby"export default function BlogPost({ data }) {
  const post = data.markdownRemark
  return (
    <div>
      <h1>{post.frontmatter.title}</h1>
      <div dangerouslySetInnerHTML={{ __html: post.html }} />
    </div>
  )
}
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
```

`src/content/post.md`

```
---
title: "A Trip To the Zoo"
tags: ["animals", "Chicago", "zoos"]
date: "2020-01-01"
---
I went to the zoo today. It was terrible.
```

在`gatsby-config.js`中，我们添加了`gatsby-source-filesystem`插件来读取`src/content`文件夹中的降价文件。

我们添加了`gatsby-transformer-remark`插件来将 Markdown 转换成 HTML。

接下来，在`gatsby-node.js`中，我们用`onCreateNode`方法创建了`slug`字段。

我们调用`createFilePath`函数从文件路径创建`slug`字段值。

然后我们调用`createNodeField`来创建`slug`字段，其中对象的`name`设置为`'slug'`。

该值是我们从`createFilePath`创建的 slug 字符串。

在`createPages`函数中，我们通过查询来获取帖子数据。

`result.data.postRemark.edges`有帖子数据。

我们在它上面调用`forEach`,这样我们就可以在回调中的每个条目上调用`createPage`来创建页面。

`node.field.slug`具有我们从`onCreateNode`创建的废料值。

`component`设置为`blogPostTemplate`，这样我们就可以显示该站点。

然后为了得到标签，我们使用了`result.data.tagsGroup.group`属性。

我们将`slug`传递给第一个`forEach`中的`context`属性，以将数据传递给页面。

我们用再次调用`createPage`函数的回调来调用`forEach`。

但是这一次，我们用它来创建标签页。

`path`是标签页面的 URL。

`component`拥有我们用来呈现标签页面的模板。

`context`有我们想要在标签页上呈现的数据。

在`tags.js`中，我们有了标签页面模板。

我们用`pageQuery`对象查询带有给定标签的项目。

然后在`tags`组件中，我们从`data` prop 获取所有返回的数据。

`edges`拥有带有给定标签的物品。

我们调用`map`来呈现返回的内容。

`slug`属性有 slug。`title`有标题。

我们为每个条目准备了`Link`组件，这样我们就可以转到给定 slug 的页面。

在`post.js`中，我们渲染页面。

我们从`data`道具的`markdownRemark`属性中获取数据。

然后我们看到显示的数据。

我们使用`query`对象进行查询，以获得带有给定 slug 的帖子。

# 结论

我们可以添加一个标签页来显示带有给定标签的 Markdown 文件，并添加一个 post 页来显示带有 Gatsby 的给定 slug 的 Markdown 文件。