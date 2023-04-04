# Gatsby.js —自定义 RSS 源

> 原文：<https://blog.devgenius.io/gatsby-js-custom-rss-feed-68ebb267a1d7?source=collection_archive---------6----------------------->

![](img/57609869a4285c901a41fd04e02da6fd.png)

由 [Andrik Langfield](https://unsplash.com/@andriklangfield?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 自定义 RSS 源

我们可以通过在`gatsby-config.js`中添加更多插件选项来定制我们的 RSS 提要。

例如，我们可以写:

`gatsby-config.js`

```
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-plugin-feed`,
      options: {
        query: `
          {
            site {
              siteMetadata {
                title
                description
                siteUrl
                site_url: siteUrl
              }
            }
          }
        `,
        feeds: [
          {
            serialize: ({ query: { site, allMarkdownRemark } }) => {
              return allMarkdownRemark.edges.map(edge => {
                return Object.assign({}, edge.node.frontmatter, {
                  description: edge.node.excerpt,
                  date: edge.node.frontmatter.date,
                  url: site.siteMetadata.siteUrl + edge.node.fields.slug,
                  guid: site.siteMetadata.siteUrl + edge.node.fields.slug,
                  custom_elements: [{ "content:encoded": edge.node.html }],
                })
              })
            },
            query: `
              {
                allMarkdownRemark(
                  sort: { order: DESC, fields: [frontmatter___date] },
                ) {
                  edges {
                    node {
                      excerpt
                      html
                      fields { slug }
                      frontmatter {
                        title
                        date
                      }
                    }
                  }
                }
              }
            `,
            output: "/rss.xml",
            title: "Your Site's RSS Feed",
          },
        ],
      },
    },
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ],
  siteMetadata: {
    siteUrl: `https://www.example.com`,
  },
}
```

我们需要`siteMetadata.siteUrl`来生成 RSS 提要。

`gatsby-node.js`

```
const path = require("path")
const _ = require("lodash")
const { createFilePath } = require(`gatsby-source-filesystem`)exports.onCreateNode = ({ node, actions, getNode }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const value = createFilePath({ node, getNode })
    createNodeField({
      name: `slug`,
      node,
      value,
    })
  }
}exports.createPages = async ({ actions, graphql, reporter }) => {
  const { createPage } = actions
  const blogPostTemplate = path.resolve("src/templates/post.js")
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
}
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

我们将`gatsby-plugin-feed`插件添加到`gatsby-config.js`中。

我们通过运行以下命令来安装插件:

```
npm i gatsby-plugin-feed
```

我们在配置文件中添加了一些选项。`options.query`属性包含 GraphQL 查询获取站点元数据的字符串。

`feeds`属性有`serialize`方法从查询中获取数据，并为检索到的每个条目返回一个对象。

`description`有帖子的描述。

`date`有发文的日期。

`url`有帖子的网址。

`guid`是我们生成的唯一 ID。

`custom_elements`有我们想要包含在 RSS 提要中的额外元素。

`feeds.query`已经查询得到的帖子。

`output`具有 RSS 源文件的路径。

`title`有提要的标题。

在`gatsby-node.js`中，我们使用 GraphQL 查询来获取 Markdown 帖子，并调用`createPage`来创建帖子。

而`post.js`通过从`data`道具中获取数据来显示帖子。

现在，当我们运行`npm run build`时，我们看到生成了`public/rss.xml`文件。

我们应该看到`item`元素和`title`、`description`、`link`、`guid`、`pubDate`以及`content:encoded`元素。

# 结论

我们可以用 Gatsby 为我们的站点创建一个定制的 RSS 提要。