# 使用 Create React 应用程序设置 ESLint+beauty+AirBnB 风格

> 原文：<https://blog.devgenius.io/setup-eslint-prettier-airbnb-style-with-create-react-app-f2b4fda1ce5a?source=collection_archive---------1----------------------->

*原发于我的* [*博客*](https://www.andrewmin.info/blog/react-setup/)

![](img/b3b31037e403526641048f566cd75fcf.png)

React、ESLint、React 和 AirBnB 徽标。

# 概观

有许多工具可以帮助 lint 和格式化您的 JavaScript 代码，以至于设置一个项目会变得很复杂。我将展示如何用一些最流行的 [ESLint](https://eslint.org/) 和[prettle](https://prettier.io/)来建立一个 React 项目，同时也集成 AirBnB 流行的[风格指南](https://airbnb.io/javascript/)。

对于本指南，您需要安装 Node.js，因为它捆绑了`npm`和`npx`、Node**P**package**M**anager 和**N**pm**P**package e**X**executor。`npm`将软件包安装到您的项目中，而`npx`运行软件包二进制文件。

# 创建 React 应用程序

如果你还没有 React 应用，使用[创建 React 应用](https://github.com/facebook/create-react-app#create-react-app--)为你设置一个。它将自动创建一个具有依赖关系的单页 React 应用程序(React、Babel、Webpack 等)。)和基本项目结构。README 有一个完整的指南，但是本质上你所要做的就是运行 Create React App package 脚本，用`npx`然后`cd`进入项目目录。

```
npx create-react-app my-app
cd my-app
```

# ESLint + AirBnB

ESLint 是一个 **linter** ，它将分析你的代码并找到常见问题，同时还会识别与 AirBnB 的风格指南不一致的风格(如果配置了的话)。

为了安装 ESLint 并设置一个配置文件，我们将使用另一个`npx`包脚本。

```
npx eslint --init
```

该脚本将询问一些问题，然后继续将其依赖项安装到`./package.json`的`devDependencies`部分。它还创建了包含所有配置的`./.eslintrc.json`。

```
? How would you like to use ESLint? To check syntax, find problems, and enforce code style
? What type of modules does your project use? JavaScript modules (import/export)
? Which framework does your project use? React
? Does your project use TypeScript? No
? Where does your code run? Browser
? How would you like to define a style for your project? Use a popular style guide
? Which style guide do you want to follow? Airbnb: https://github.com/airbnb/javascript
? What format do you want your config file to be in? JSON
```

然而，Create React App 中的 react-scripts 包需要一个旧版本的 ESLint，这可以从运行`npm start`会抛出一条很长的错误消息看出。为了解决这个问题，我手动将`./package.json`中的`eslint`降级为错误消息中显示的所需版本。截至发稿时，我将 ESLint 从`^7.5.0`降级为`^6.6.0`，然后运行`npm install`。

此外，Create React App 工具链使用 [Babel](https://babeljs.io/) 将新的 JavaScript 特性移植到旧版本中，以便在旧浏览器中运行。然而，ESLint 解析器没有跟上 JavaScript 的变化，所以我们需要使用`babel-eslint`解析器。在`./.eslintrc.json`中，添加:

```
"parser": "babel-eslint"
```

要运行 linter，对 src 目录中的一个文件或每个`.js`和`.jsx`文件运行 ESLint 包脚本。

```
npx eslint 'src/**/*.{js,jsx}'
```

# 较美丽

更漂亮的是一个代码格式化器，它可以识别并自动修复代码中的风格问题。为了安装，我们需要安装 3 个包——`prettier`本身,`eslint-plugin-prettier`将 Prietter 集成到 ESLint 中，而`eslint-config-prettier`将关闭与 Prettier 冲突的 ESLint 规则。

(不要忘记将这些包添加到`./package.json`的`--save-dev`标志)

```
npm install prettier eslint-plugin-prettier eslint-config-prettier --save-dev
```

现在我们必须给`./.eslintrc.json`添加一些东西来让 ESLint 和 Prettier 一起工作。

1.  将`"prettier"`添加到`plugins`部分。
2.  在`extends`部分增加`"prettier"`和`prettier/react`。
3.  将`"prettier/prettier": "error"`添加到`rules`部分。也可以将“错误”改为“警告”。

现在，如果我们运行`npx eslint 'src/**'`，我们也可以看到来自更漂亮的警告/错误。要运行格式化和简单的修复，我们可以运行:

```
npx eslint 'src/**/*.{js,jsx}' --fix
```

# 更多配置

首先，向`./.eslintrc.json`中的`rules`部分添加条目允许您禁用某些 ESLint 规则。以下是我个人喜欢禁用的一些规则。

*   `"react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }]` -允许 JSX 在`*.js`文件中做出反应。(默认情况下，AirBnB 强制 React 组件具有`*.jsx`扩展名)
*   `"react/state-in-constructor": "off"`——允许你将状态声明为类变量，而不是在 React 组件的构造函数中。

接下来，我们可以通过创建一个`./.prettierrc`文件并更改一些[选项](https://prettier.io/docs/en/options.html)来进行更漂亮的配置。以下是我想改变的一些选项。

*   `"printWidth": 100` -将最大线宽改为 100 个字符(默认为 80)
*   `"singleQuote": true` -对字符串使用单引号，这是 AirBnB 的风格(默认为 false)

到目前为止，您的文件可能看起来像这样。

`package.json`

```
{
  ...
  "devDependencies": {
    "eslint": "6.6.0",
    "eslint-config-airbnb": "^18.2.0",
    "eslint-config-prettier": "^6.11.0",
    "eslint-plugin-import": "^2.22.0",
    "eslint-plugin-jsx-a11y": "^6.3.1",
    "eslint-plugin-prettier": "^3.1.4",
    "eslint-plugin-react": "^7.20.3",
    "eslint-plugin-react-hooks": "^4.0.8",
    "prettier": "^2.0.5"
  }
}
```

`.eslintrc.json`

```
{
    "env": {
        "browser": true,
        "es6": true
    },
    "parser": "babel-eslint",
    "extends": [
        "plugin:react/recommended",
        "airbnb",
        "prettier",
        "prettier/react"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "prettier"
    ],
    "rules": {
        "prettier/prettier": "error",
        "react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],
        "react/state-in-constructor": "off"
    }
}
```

`.prettierrc`

```
{
    "printWidth": 100,
    "singleQuote": true
}
```

# 与 VSCode 集成

为了在代码中显示 ESLint 警告并在 VSCode 中自动运行格式化，我们需要安装 2 个扩展。

# 埃斯林特

*   安装 [ESLint 扩展](https://github.com/Microsoft/vscode-eslint)
*   好了

# 较美丽

*   安装[漂亮的加长件](https://github.com/prettier/prettier-vscode)
*   编辑 VSCode `settings.json`
*   添加
    `"[javascript]": { "editor.defaultFormatter": "esbenp.prettier-vscode", }` 将 JavaScript 文件的格式化程序改为更漂亮的。
*   如果您也使用`*.jsx`文件，对`[javascriptreact]`进行与上述相同的设置
*   现在运行`Format Document`将使用更漂亮
*   有关更多详细信息，请参见扩展自述文件(例如，保存时运行格式化)

*原贴于我的* [*博客*](https://www.andrewmin.info/blog/react-setup/)