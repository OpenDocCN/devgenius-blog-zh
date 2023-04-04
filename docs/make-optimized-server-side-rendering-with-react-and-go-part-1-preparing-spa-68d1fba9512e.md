# 使用 React and Go 优化服务器端渲染:第 1 部分—准备 SPA

> 原文：<https://blog.devgenius.io/make-optimized-server-side-rendering-with-react-and-go-part-1-preparing-spa-68d1fba9512e?source=collection_archive---------0----------------------->

当我们谈到用 React 应用程序完成的 SSR 时，我们通常会考虑像 NextJS 或 NestJS 这样的框架。这种方法非常适合内容丰富的交互式网站。但是如果…

1.React 中已经内置了一个 SPA，只实现客户端渲染？

2.这是用 create-react-app 等第三方 React 模板工具创建的？

3.您不喜欢为服务器构建创建单独的 Webpack 配置吗？

4.你正在寻找一种便捷的渲染优化方法(不同的缓存策略取决于页面等)。)带轻量级运行时？

那么，你不是一个人！在这一系列的文章中，我将向你解释，我是如何实现我自己的便捷 SSR 解决方案的，我将在第二部分给你 Golang 样板文件的链接。以下是该系列文章的链接:

链接至第 1 部分—此处

链接到第 2 部分—[https://medium . com/@ jzx 777/make-optimized-server-side-rendering-with-react-and-go-Part-2-programming-server-application-EAE 03 f 834702](https://medium.com/@jzx777/make-optimized-server-side-rendering-with-react-and-go-part-2-programming-server-application-eae03f834702)

这一部分将探讨为了让 SSR 发挥作用，您需要对前端应用程序做些什么。让我们开始我们的旅程。耶；3

**1。SPA 的 index.tsx 条目通常是这样的:**

```
// src/index.tsx

/* eslint-disable no-restricted-globals */
import React from 'react';
import ReactDOM from "react-dom";
import App from "./components/app";

const root = document.getElementById("root");

ReactDOM.render(
   <App />,
   root,
);
```

比如说，我们只需要在页面/屏幕"/pageA "、"/pageB "、"/pageC "上做 SSR，而不需要在其他页面/屏幕上做。您还想保留以前构建 SPA 的开发经验(包括所有客户端渲染内容)。因此，首先，我们需要一种方法来区分浏览器和非浏览器环境。让我们创建简单的全局 var 检查:

```
// is-client.ts

export const IS_CLIENT = typeof window !== 'undefined';
```

那我们就在半路上了！我们的“index.tsx”文件可以通过以下方式进行修改，以处理上述选择性水合情况:

```
// index.tsx

/* eslint-disable no-restricted-globals */
import React from 'react';
import ReactDOM from "react-dom";
import App from "./components/app";
import { IS_CLIENT } from './is-client';

const getRoot = () => {
    return document.getElementById("root");
};

// It can be more smart or quick check depending on your use-case
const isHydratedPage = (path: string) =>
    path.startsWith('/pageA') ||
    path.startsWith('/pageB') ||
    path.startsWith('/pageC');

const initializeApp = () => {
    const root = getRoot();

    // @ts-ignore
    if (process.env.NODE_ENV === 'development') {
        ReactDOM.render(
            <App />,
            root,
        );
        return;
    }

    if (root.childNodes.length && isHydratedPage(window.location.pathname)) {
        ReactDOM.hydrate(
            <App />,
            root,
        );
    } else {
        ReactDOM.render(
            <App />,
            root,
        );
    }
};

if (IS_CLIENT) {
  initializeApp();
}
```

客户端工作的所有修改都已完成。但是我们的服务器渲染器应该如何从页面获取 html 呢？

**2。我们需要记住几件事:**

*   区分不同的页面是服务器的责任，因为我们需要用不同种类的资源和缓存策略来获取和缓存页面；
*   浏览器应用程序应该在启动时以某种方式恢复其应用程序状态。如果你用像 Redux 这样的库或者你自己的解决方案来区分应用程序和 UX 状态(我目前正在使用 Redux，所以将会给出关于它的例子)；
*   运行在服务器上的 JS 应用程序，不能监听 URL 更新，也不知道当前的 URL。因此，我们需要一些来自服务器的预定义路由路径(我将展示一种关于 React Router lib 的方法)；
*   启动时应用程序应避免 FOUC(非样式内容的闪存)问题，以获得更好的用户体验；

对于第一部分，让我们添加仅由服务器使用的端点，以便呈现所需的页面:

```
// index.tsx 
// ... rest of client-side startup code we previously wrote

// External endpoints for server renderer
if (!IS_CLIENT) {
    const ssrAPage = (args: string[]) => {
        const [language, pageAData, requestedPath] = args;

        const serverStatePatch = (state: TRootModel) => {
            const res = {
                ...state,
                globalSettings: {
                    ...state.globalSettings,
                    language: language,
                },
                dataA: JSON.parse(pageAData),
            }

            return res;
        };

        const html = ReactDOMServer.renderToString(
            <App serverStatePatch={serverStatePatch} requestedPath={requestedPath} />,
        );

        return html;
    };
    // @ts-ignore
    ssrExports.ssrAPage = ssrAPage;

    // ... Rest of endpoints are analogous to it 
}
```

我想，在这一点上你有一个问题，比如:“为什么你把 SSR 端点的参数写成一个字符串数组？”。我这样写是因为 JavaScript 解析库的古怪，我将在下一部分介绍它。然而，它工作得很好；

如第二点所述，为了使恢复 Redux 状态成为可能，我将添加另一个 SSR 端点来修补初始 Redux 状态:

```
// index.tsx 
// ... rest of client-side startup code we previously wrote

// External endpoints for server renderer
if (!IS_CLIENT) {
    const ssrAStatePatch = (args: string[]) => {
        const [language, pageAData] = args;

        return `
            <script>
                window.__INIT_STATE_PATCH__ = function(state) {
                    var res = {
                        ...state,
                        globalSettings: {
                          ...state.globalSettings,
                          language: "${language}",
                        },
                        dataA: ${pageAData},
                    };

                    return res;
                };
            </script>
        `;
    };
    // @ts-ignore
    ssrExports.ssrAStatePatch = ssrAStatePatch;

    const ssrAPage = (args: string[]) => {
        const [language, pageAData, requestedPath] = args;

        const serverStatePatch = (state: TRootModel) => {
            const res = {
                ...state,
                globalSettings: {
                    ...state.globalSettings,
                    language: language,
                },
                dataA: JSON.parse(pageAData),
            }

            return res;
        };

        const html = ReactDOMServer.renderToString(
            <App serverStatePatch={serverStatePatch} requestedPath={requestedPath} />,
        );

        return html;
    };
    // @ts-ignore
    ssrExports.ssrAPage = ssrAPage;

    // ... Rest of endpoints are analogous to it 
}
```

我们在状态初始化函数中使用这个精心制作的补丁，如下所示:

```
// State
import { globalSettingsInitState } from './global-settings';
// ... A bunch of init states ...
// Is client
import { IS_CLIENT } from '@is-client';
// Types
import { TRootModel } from './types';

export const createrRootInitState = (): TRootModel => {
  let res = {
    globalSettings: globalSettingsInitState,
    // ... A bunch of init states ...
  };

  // @ts-ignore
  if (IS_CLIENT && window.__INIT_STATE_PATCH__) {
    // @ts-ignore
    return window.__INIT_STATE_PATCH__(res);
  }

  return res;
} 
```

应用程序入口的最后一个代码片段包含“App”组件的一些奇怪属性，如“serverStatePatch”。此属性使服务器上的 Redux 状态初始化成为可能，方法如下:

```
// State
import { globalSettingsInitState } from './global-settings';
// ... A bunch of init states ...
// Is client
import { IS_CLIENT } from '@is-client';
// Types
import { TRootModel } from './types';

export const createrRootInitState = (serverPatch?: (state: TRootModel) => TRootModel): TRootModel => {
  let res = {
    globalSettings: globalSettingsInitState,
    // ... A bunch of init states ...
  };

  if (serverPatch) {
    return serverPatch(res);
  }

  // @ts-ignore
  if (IS_CLIENT && window.__INIT_STATE_PATCH__) {
    // @ts-ignore
    return window.__INIT_STATE_PATCH__(res);
  }

  return res;
}
```

“createRootInitState”用于存储初始化函数:

```
// eslint-disable-next-line @typescript-eslint/no-unused-vars
import { createStore } from 'redux';
import { makeRootReducer } from '@redux-reducers';
import { createrRootInitState } from '@redux-models';
// IS_CLIENT
import { TRootModel } from '@/redux-models/types';

export const initializeStore = (serverPatch?: (state: TRootModel) => TRootModel) => {
  return createStore(
    makeRootReducer(),
    createrRootInitState(serverPatch),
  );
}
```

store 在“App”组件中初始化，以处理客户端和服务器的工作，如下所示:

```
// ../app.tsx

import React, { useMemo } from "react";
import { Provider as StoreProvider } from 'react-redux';
import { ThemeProvider } from "styled-components";
import { BrowserRouter, Route, Routes as Switch } from "react-router-dom";
import { StaticRouter } from 'react-router-dom/server';
import { DefaultTheme } from "@themes";
import { Navigate } from 'react-router';
// Screens
import PageA from '@screens/PageA';
// Components
import SomeToolbar from '@components/SomeToolbar';
// Store
import { initializeStore } from '@redux-store';
// Utils
import { withAuth } from './withAuth';
import { TRootModel } from "@/redux-models/types";
// Contexts
import { ServicesProvider } from '@contexts/services';
// Is client
import { IS_CLIENT } from '@is-client';

interface TAppProps {
  serverStatePatch?: (state: TRootModel) => TRootModel;
  requestedPath?: string;
  requestedQuery?: string;
}

const Router: React.FC<{
  requestedPath?: string;
  requestedQuery?: string;
  children?: React.ReactNode;
}> = ({
  requestedPath,
  requestedQuery,
  children,
}) => {
  if (IS_CLIENT) {
    return (
      <BrowserRouter>
        {children}
      </BrowserRouter>
    );
  }

  return (
    <StaticRouter
      location={{
        pathname: requestedPath,
        search: requestedQuery, 
      }}
    >
      {children}
    </StaticRouter>
  );
}

const App: React.FC<TAppProps> = ({ serverStatePatch, requestedPath, requestedQuery }) => {
  const store = useMemo(
    () => {
      return initializeStore(serverStatePatch);
    },
    // eslint-disable-next-line react-hooks/exhaustive-deps
    [],
  );

  return (
    // @ts-ignore
    <StoreProvider store={store}>
      <ServicesProvider>
        <ThemeProvider theme={DefaultTheme}>
          <Router requestedPath={requestedPath} requestedQuery={requestedQuery}>
            <SomeToolbar />
            <Switch>
              <Route
                path='/pageA'
                element={<PageA />}
              />
              {/* ... rest of pages ... */}
            </Switch>
          </Router>
        </ThemeProvider>
      </ServicesProvider>
    </StoreProvider>
  )
};

export default App;
```

是的，我们还宣布了依赖于环境的选择性路由器。属性“requestedPath”和“requestedQuery”用于第三点中描述的目的；

最后一点也取决于你使用的造型方法。我使用“样式组件”,所以我在 SSR html 渲染端点中添加了一个可爱的小补丁:

```
// index.tsx 
// ... rest of client-side startup code we previously wrote

// External endpoints for server renderer
if (!IS_CLIENT) {
    // ... ssr state patch endpoint we previously wrote ...
    const ssrAPage = (args: string[]) => {
        const [language, pageAData, requestedPath] = args;

        const serverStatePatch = (state: TRootModel) => {
            const res = {
                ...state,
                globalSettings: {
                    ...state.globalSettings,
                    language: language,
                },
                dataA: JSON.parse(pageAData),
            }

            return res;
        };

        const styleSheet = new ServerStyleSheet();
        const html = ReactDOMServer.renderToString(
            styleSheet.collectStyles(
              <App serverStatePatch={serverStatePatch} requestedPath={requestedPath} />
            ),
        );
        const cssTags = styleSheet.getStyleTags();

        return {
          html,
          cssTags,
        }
    };
    // @ts-ignore
    ssrExports.ssrAPage = ssrAPage;

    // ... Rest of endpoints are analogous to it 
}
```

这就是选择性 SSR 的 SPA 代码准备的全部内容。根据您使用的库和模式(例如小心‘react-router-DOM’中的‘useSearchParams’:这个钩子在初始化阶段严重依赖浏览器环境，在非浏览器环境下需要省略)；

**3。准备 HTML 基础模板，以便服务器可以对其应用补丁:**

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="example content"
    />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>Example SPA</title>
    <link rel="stylesheet" href="%PUBLIC_URL%/styles.css" />
    <style id="sc-placeholder-root"></style>
  </head>
  <body>
    <div id="root"></div>
    <script id="state-patch-root"></script>
  </body>
</html>
```

**4。让我们做一些 Webpack 配置和额外的工作脚本**

我们需要做的就是声明我们自己的全局变量，这样 Webpack 就不会破坏它，这样服务器就能够理解它们:

```
// my-project/config/global-vars.patch.js

module.exports = {
  ssrExports: "ssrExports",
  ssrImports: "ssrImports",
  // I declared ssrImports for some useful server APIs like logging and debugging JS code
};
```

在我们的 Webpack 配置中:

```
// my-project/config/webpack.config.js
const globalVarsPatch = require('./global-vars-patch');

// ...

  plugins: [
    // ... a bunch of other plugins ...,
    new webpack.DefinePlugin({
        ...env.stringified,
        ...globalVarsPatch,
    }),
    // ... a bunch of other plugins ...,
  ],

// ...
```

我们还需要通过对 Terser 插件的一些修改来阻止篡改 SSR 端点:

```
// my-project/config/webpack.config.js
// ...

  minimizer: [
    new TerserPlugin({
          terserOptions: {
            parse: {
              ecma: 8,
            },
            compress: {
              ecma: 5,
              warnings: false,
              comparisons: false,
              inline: 2,
            },
            mangle: {
              safari10: true,
            },
            keep_classnames: isEnvProductionProfile,
            // This line makes things work
            keep_fnames: /^ssr[a-zA-Z]+Page|ssr[a-zA-Z]+StatePatch$/,
            output: {
              ecma: 5,
              comments: false,
              ascii_only: true,
            },
          },
        }),
    // ... a bunch of other plugins ...,
  ],

// ...
```

最后要做的是创建 JS 构建复制脚本。服务器需要 JS 构建的稳定名称，但是默认情况下，Webpack 在构建名称中包含散列字符串(但是，在文件缓存的情况下，这对于在客户端强制 JS 捆绑包更新很有用):

```
// my-project/job-scripts/copy-build-for-server/index.js

const fs = require('fs/promises');
const path = require('path');
const getFileExtension = require('./file').getFileExtension;

const main = async () => {
  const pathToStatic = path.resolve(
    __dirname,
    "..",
    "..",
    "build",
    "static",
    "js"
  );
  const fileNames = await fs.readdir(pathToStatic);

  for (const fName of fileNames) {
    const ext = getFileExtension(fName);
    if (fName.startsWith('main') && ext === 'js') {
      const jsFile = await fs.readFile(
        path.resolve(pathToStatic, fName),
        { encoding: 'utf-8' },
      );

      const writePath = path.resolve(__dirname, "..", "..", "build", "server", "js");
      await fs.mkdir(writePath, { recursive: true });
      await fs.writeFile(
        path.resolve(writePath, "build.js"),
        jsFile,
        { encoding: 'utf-8' },
      );

      console.log('Server build is ready!');
      return;
    }
  }
};

main().catch(err => console.error(err));
// my-project/job-scripts/copy-build-for-server/file.js
```

```
// my-project/job-scripts/copy-build-for-server/file.js
const getFileExtension = (path) => {
  let ptr = path.length - 1;
  while (ptr >= 0 && path[ptr] !== '.') {
    ptr--;
  }

  return ptr >= 0 ? path.slice(ptr + 1) : '';
};

module.exports = {
  getFileExtension,
};
```

并将该脚本链接到“package.json”中的“build”命令:

```
"build": "node scripts/build.js && node job-scripts/copy-build-for-server/index.js",
```

你的温泉之旅结束了！您仍然可以在开发服务器上将它作为客户端呈现的应用程序进行测试。核心内容将在系列的下一部分描述；

*请随意留下你的经验等任何建议。我感谢任何有益的反馈；)*

*我的 git lab:*[*https://gitlab.com/jbyte*](https://gitlab.com/john-byte)*777*