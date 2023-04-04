# 如何将 webpack 分为开发和生产

> 原文：<https://blog.devgenius.io/how-to-divide-webpack-into-development-and-production-8d1027b03666?source=collection_archive---------1----------------------->

![](img/309bf4df203e5be5f9c6d5f433057e11.png)

图片来自[https://en.wikipedia.org/wiki/Webpack](https://en.wikipedia.org/wiki/Webpack)

**目的**

我将 webpack.file 分成两部分(开发和生产，因为它更智能)，因此我将分享这些知识。

**步骤**

1.  在 package.json 的依赖项中安装 webpack-merge(命令行中的“NPM install web pack-merge—save-dev ”)

2.创建 3 个文件 webpack.common.js、webpack.prod.js 和 webpack.dev.js(文件名由你决定)而不是 webpack.config.js。

3.像下面这样写 3 个文件。Webpack.merge 用于合并用于开发和生产的 Webpack 通用文件。

第一个文件

```
webpack.common.jsNOTICE: almost all webpack.config is here.const path = require("path");
module.exports = {
  entry: "./src/app.tsx",
  output: {
    path: path.join(__dirname, "public", "dist"),
    filename: "bundle.js",
  },
  performance: {
    hints: false,
    maxEntrypointSize: 512000,
    maxAssetSize: 512000,
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: "ts-loader",
        exclude: /node_modules/,
        options: {
          configFile: path.resolve(__dirname, "./ts.config.json"),
        },
      },
      {
        test: /\.svg$/,
        use: [
          {
            loader: "svg-url-loader",
            options: {
              limit: 10000,
            },
          },
        ],
      },
      {
        test: /\.s?css$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
  resolve: {
    modules: ["node_modules"],
    extensions: [".ts", ".tsx", ".js", "jsx", "json"],
  },
  target: ["web", "es5"],
};
```

第二个文件

```
webpack.dev.jsconst path = require("path");
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");
module.exports = merge(common, {
  mode: "development",
  devtool: "cheap-source-map",
  devServer: {
    static: {
      directory: path.join(__dirname, "public"),
    },
    historyApiFallback: true,
  },
});
```

第三个文件

```
webpack.prod.js//NOTICE: you can write much simpler webpack file for production before separating because production file doesn’t need “devtool” and “devServer”.const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");
module.exports = merge(common, {
  mode: "production",
});
```

4.在包 json 的脚本中添加带有— config 的独立文件。

```
"scripts": {
    "build": "webpack --config webpack.prod.js",
    "start": "webpack-dev-server --hot --config webpack.dev.js"
  },
* you don't need to have --hot in scripts.
```

5.完成了。正如我所写的，您可以为生产编写一个简单得多的文件。

**结论**

如果你把文件分开，会多花一点时间。然而，这对于通过省略仅使用开发环境来为生产创建更快的应用程序是有用的。

**参考**

生产用网络包:[https://webpack.js.org/guides/production/](https://webpack.js.org/guides/production/)

感谢您的阅读！！