# 所有你需要知道开始混音反应！

> 原文：<https://blog.devgenius.io/get-started-with-remix-react-in-1-2-3-9e0b762be096?source=collection_archive---------6----------------------->

![](img/1bb4c0ac468f618fd38597e7638e92de.png)

混音标志

Remix 是一个创建服务器端渲染应用的伟大框架。我刚刚开始使用它。开发者体验极佳。混合是围绕路由器构建的，这对于有多条路由的应用程序来说完全有意义。用户一次只看一个页面，Remix 很好地处理了这一点，只加载用户当前访问页面的必要资源。Remix 是关于用户体验和开发者体验的。我认为他们已经获得了用户和开发者的双重体验。

## 数据加载

Remix 使得获取页面数据变得容易。Remix 提供了 LoaderFunction，它负责加载页面中可访问的数据。

LoaderFunction 仅在位于 routes 目录下的页面组件中工作。Remix 不会在 routes 目录之外的组件中调用 LoaderFunction。

要访问组件中的数据，你只需要使用钩子*useLoaderData()；*下面的例子演示了如何进行简单的数据提取。

```
export const loader: LoaderFunction = async () => {
  return await fetchHomePage();
};

const Index = (): JSX.Element => {
  const fetchedDataFromTheLoaderFunction = useLoaderData();
  return (
     ...
  );
}
```

请随意阅读关于文档中数据加载的更多信息，【https://remix.run/docs/en/v1/guides/data-loading 

## 式样

Remix 使页面和组件的样式变得简单。Remix 提供了一个 links 函数，我们可以在其中应用要添加到文档中的样式表。下面的例子演示了 LinkFunction 的使用有多简单。

```
import styles from "./my-page.css";

export const links: LinksFunction = () => [{ rel: 'stylesheet', href: styles }];

const Index = (): JSX.Element => {
  return (
     <h1 className="my-page-title">My Page Title</h1>
  );
}
```

请随意阅读更多关于链接功能的文档，【https://remix.run/docs/en/v1/guides/styling 

## 搜索引擎优化

通过使用元函数，可以毫不费力地将您指定的元标签应用到文档的标题中，从而使页面 SEO 友好。元函数将当前页面上获取的数据作为参数。这使得动态地将数据应用到将被添加到文档头的元标签变得容易。

Remix 将元功能从组件功能本身中分离出来。在你的组件中不需要 SEO 组件渲染。

```
export const meta: MetaFunction = ({ data }) => ({
  charset: "utf-8",
  description: data.seo.description,
  keywords: data.seo.keywords,
  author: data.seo.author,
  viewport: "width=device-width, initial-scale=1.0",
});

const Index = (): JSX.Element => {
  return (
    <>
     <h1>My Component Code</h1>
     <p>No SEO Component needs to be render within this component</p>
    </>
  );
}
```

请随意阅读文档中关于元函数的更多内容，[https://remix.run/docs/en/v1/api/conventions#meta](https://remix.run/docs/en/v1/api/conventions#meta)

## 按指定路线发送

Remix 正在映射 routes 目录中的文件夹结构来创建路由。请看下面的几个例子。

```
routes/
  - index.tsx

The folder stucture above will be mapped to be the root, mydomain.com/

routes/
 - about-us.tsx

The folder stucture above will be mapped to mydomain.com/about-us

routes/
 - $page.tsx

The $-sign is used for dynamic routes wich needs paramters, could be a slug or
id from the fetched CMS-content. The route avobe will be resolved to
mydomain.com/slug-from-cms.
```

上面的例子说明了路由目录中的文件名将决定路径名。

路由是混音中的一个重要概念。Remix 围绕路由器构建了许多模式。我喜欢将路由器视为混合架构的核心。

请随意阅读更多关于文档内路由的信息，[https://remix.run/docs/en/v1/guides/routing#defining-routes](https://remix.run/docs/en/v1/guides/routing#defining-routes)

## 错误处理

Remix 比其他任何人都更容易处理错误。我一直在使用客户端渲染(CSR) react 应用程序。其中我们手动错误处理每个发送到后端的 HTTP 请求，然后将错误消息呈现给用户。这是一个手动过程，记住如果后端抛出错误，要处理对后端的每个影响用户体验的请求。

在 Remix 中，我们不需要使用 try/catch 或 catch 对后端发出的每个请求。Remix 用错误边界处理错误。如果 LoaderFunction 无法获取当前页面所需的数据，则将呈现错误边界组件。

```
export const MyCustomErrorBoundry= ({ error }) => {
  return (
    <html>
      <head>
        <title>Snap, something went wrong!</title>
        <Meta />
        <Links />
      </head>
      <body>
        <MyErrorMessageCompoent satus={error.status}>
          { error.message }
        </MyErrorMessageCompoent>
      </body>
    </html>
  );
}
```

上面的代码是我们的自定义 ErrorBoundryComponent，如果 LoaderFunction 抛出错误，它将被呈现。为了提供这个自定义的 ErrorBoundryComponent，我们需要在页面组件中指定我们的 ErrorBoundary。请参见下面的代码示例。

```
export const loader: LoaderFunction = async () => {
  return await fetchHomePage();
};

export const ErrorBoundary: ErrorBoundaryComponent = ({ error }) => {
  return <MyCustomErrorBoundry error={error} />;
};

const Index = (): JSX.Element => {
  const fetchedDataFromTheLoaderFunction = useLoaderData();
  return (
     ...
  );
}
```

请随意阅读更多关于文档中的错误处理，[https://remix.run/docs/en/v1/guides/errors](https://remix.run/docs/en/v1/guides/errors)

## 创建项目

Remix 有一个很棒的命令行界面(CLI)来创建新项目。CLI 为 JavaScript 和 TypeScript 提供模板。您可以在模板变体之间进行选择，例如仅包含入门所需的最低限度的基本模板或完整的生产就绪模板。

打开您的终端并编写命令`npx create-remix@latest`，然后 CLI 会在创建项目的过程中向您提问。当命令完成后，您就可以在您最喜欢的 IDE 或编辑器中打开项目并开始编码了。

应该没有必要进行大量的配置。这些项目甚至带有现成的 alias-path，~/components/my-component。

请随意阅读关于在 docs 中创建项目的更多信息，[https://remix . run/docs/en/v1/tutorials/blog # creating-the-project](https://remix.run/docs/en/v1/tutorials/blog#creating-the-project)

# 感谢阅读！

感谢阅读，并随时分享或评论！:)