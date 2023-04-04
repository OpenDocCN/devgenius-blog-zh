# Gatsby.js —滚动位置、动态导航和链接状态

> 原文：<https://blog.devgenius.io/gatsby-js-scroll-position-dynamic-navigation-and-link-states-c272d7696112?source=collection_archive---------3----------------------->

![](img/703e7385373903099f3b3310c4b41bbb.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Gatsby 是一个基于 React 的静态网站框架。

我们可以用它从外部数据源创建静态网站等等。

在本文中，我们将看看如何用 Gatsby 创建一个站点。

# 卷轴修复

我们可以用盖茨比恢复刷新后的滚动位置。

例如，我们可以写:

```
import React from "react"
import { useScrollRestoration } from "gatsby"const IndexPage = () => {
  const ulScrollRestoration = useScrollRestoration(`page-component-ul-list`)return (
    <ul style={{ height: 200, overflow: `auto` }} {...ulScrollRestoration}>
      {Array(100).fill().map((_, i) => i).map(n => (
        <li key={n}>{n}</li>
      ))}
    </ul>
  )
}export default IndexPage
```

我们使用带有`'page-component-ul-list'`参数的`useScrollRestoration`钩子来创建一个对象来恢复滚动位置。

然后我们将该对象的属性作为`ul`的道具，让我们恢复滚动位置。

# 道具的位置数据

我们可以从道具中获取位置数据。

为此，我们写道:

`gatsby-config.js`

```
module.exports = {
  siteMetadata: {
    siteURL: 'http://example.com'
  }
}
```

`src/pages/foo.js`

```
import React from "react"
import { graphql } from "gatsby"const FooPage = ({ location, data }) => {
  const canonicalUrl = data.site.siteMetadata.siteURL + location.pathname
  return <div>The URL of this page is {canonicalUrl}</div>
}export const query = graphql`
  query PageQuery {
    site {
      siteMetadata {
        siteURL
      }
    }
  }
`export default FooPage
```

我们通过 GraphQL 查询从`gatsby-config.js`中获得`siteMetadata.siteURL`。

然后我们获取`location`道具的`pathname`属性来获取`FooPage`的路径。

所以我们应该看到:

```
The URL of this page is http://example.com/foo
```

当我们转到[http://localhost:8000/foo](http://localhost:8000/foo)时显示。

# 向链接组件提供状态

我们可以向一个`Link`组件提供状态。

例如，我们写道:

`src/pages/index.js`

```
import { Link } from "gatsby"
import React from "react"const IndexPage = () => {
  return <>
    <div>hello world</div>
    <Link
      to={'/foo'}
      state={{ id: 1 }}
    >
      go to foo
    </Link>
  </>
}export default IndexPage
```

`src/pages/foo.js`

```
import React from "react"const FooPage = ({ location }) => {
  const { state = {} } = location
  const { id } = state
  return <div>id: {id}</div>
}export default FooPage
```

我们将一个对象传递到`state`道具中。

然后在`FooPage`中，我们获取`location`道具的`state.id`属性来获取我们传递给`state`道具的`id`属性。

# 动态导航

我们可以用 Gatsby 动态添加导航。

为此，我们写道:

```
module.exports = {
  siteMetadata: {
    title: 'Gatsby Starter',
    menuLinks: [
      {
        name: 'home',
        link: '/'
      },
      {
        name: 'foo',
        link: '/foo'
      }
    ]
  },
  plugins: []
}
```

`src/components/layout.js`

```
import React from "react"
const { StaticQuery, Link } = require("gatsby");const Header = ({ siteTitle, menuLinks }) => (
  <header
    style={{
      background: "green",
      marginBottom: "1.45rem",
    }}
  >
    <div>
      <h1 style={{ margin: 5, flex: 1 }}>
        <Link
          to="/"
          style={{
            color: "white",
            textDecoration: "none",
          }}
        >
          {siteTitle}
        </Link>
      </h1>
      <div>
        <nav>
          <ul style={{ display: "flex", flex: 1 }}>
            {menuLinks.map(link => (
              <li
                key={link.name}
                style={{
                  listStyleType: `none`,
                  padding: `1rem`,
                }}
              >
                <Link style={{ color: `white` }} to={link.link}>
                  {link.name}
                </Link>
              </li>
            ))}
          </ul>
        </nav>
      </div>
    </div>
  </header>
)const Layout = ({ children }) => (
  <StaticQuery
    query={graphql`
        query SiteTitleQuery {
          site {
            siteMetadata {
              title
              menuLinks {
                name
                link
              }
            }
          }
        }
      `}
    render={data => (
      <>
        <Header menuLinks={data.site.siteMetadata.menuLinks} siteTitle={data.site.siteMetadata.title} />
        <div
          style={{
            margin: '0 auto',
            maxWidth: 960,
            padding: '0px 1.0875rem 1.45rem',
            paddingTop: 0,
          }}
        >
          {children}
        </div>
      </>
    )}
  />
)export default Layout;
```

`src/pages/index.js`

```
import { Link } from "gatsby"
import React from "react"
import Layout from "../components/layout"const IndexPage = () => {
  return <>
    <Layout>
      <div>hello world</div>
    </Layout>
  </>
}export default IndexPage
```

`src/pages/foo.js`

```
import React from "react"
import Layout from "../components/layout"const FooPage = () => {
  return <div>
    <Layout>
      <div>foo</div>
    </Layout>
  </div>
}export default FooPage
```

我们将`menuLinks`数组添加到`gatsby-config.js`中，为链接添加数据。

然后在`layout.js`中，我们获取链接数据，然后渲染它们。

`Header`组件接受`siteTitle`和`menuLinks`道具，并将数据呈现到 HTML 中。

`siteTitle`拥有站点的标题。

`menuLinks`是我们从`gatsby-config.js`的`menuLinks`属性中得到的一组数据。

然后在`Layout`组件中，我们添加了`StaticQuery`组件来查询菜单链接数据。

`render`属性在`data`参数中有查询的结果。

我们用`Header`组件呈现链接和标题。

`div`拥有我们在`Layout`组件标签中拥有的子组件。

# 结论

我们可以用盖茨比刷新后恢复滚动位置。

另外，我们可以从道具中获取位置数据。

我们可以从网站的元数据中动态创建链接。