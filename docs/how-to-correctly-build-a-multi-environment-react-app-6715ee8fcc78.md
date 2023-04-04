# 如何正确搭建多环境 React App？

> 原文：<https://blog.devgenius.io/how-to-correctly-build-a-multi-environment-react-app-6715ee8fcc78?source=collection_archive---------0----------------------->

![](img/5bb7a1e8798abe9f17410af4cdbfc535.png)

如果您正在使用 Create react App 构建 React 应用程序，则用于修改环境变量的内置支持取决于 NODE_ENV 值。环境文件。

当您有一个简单的部署配置时，这是非常理想的。但是，很多时候在企业级系统中，需要支持两种以上的环境；通常，还需要支持三到四种环境。您的企业需要一款多环境 React 应用来满足需求。据你所知，可以利用内部有经验的资源，或者有[顶级 Reactjs 开发公司](https://medium.com/top-software-companies/top-reactjs-development-companies-in-india-9cd8bf3f363d)准备抓住机会。

根据传统观点，您可以使用内置系统通过创建额外的。env 文件并将 NODE_ENV 变量设置为所需的值。但是，CRA 不允许通过执行弹出来实现这一点，弹出会从 React 应用程序中移除任何默认约定，并留给您自己定制。

# 目录:

1.  [简介](#3ed5)
2.  [环境变量](#23c5)
3.  [建立数值](#35af)
4.  [环境的配置](#3ff1)
5.  [环境特定。环境文件](#a99e)
6.  [结论](#b54b)

# 环境变量

React 接受位于项目根目录的. env 文件中定义的环境变量。如果您已经熟悉了细节，请跳到环境配置，但是请记住。env 作为默认值。

## 概观

*   变量可以在您的 shell 或项目的根目录中定义。环境文件。
*   导入的变量以前缀 REACT_APP_ 开头。
*   Process.env 用于存储导入的值，例如 process . env . REACT _ APP _ SECRET _ CODE。
*   为了查看新添加/更新的变量，必须重新启动开发服务器。
*   shell 中定义的变量优先于. env 文件中定义的变量。
*   NODE_ENV 自动设置为开发(使用 npm 启动时)、测试(使用 npm 测试时)或生产(使用 npm 构建时)。因此，从创建-反应-应用的角度来看，只有三种环境。

# 建立价值观

变量可以在执行 npm 启动或 npm 构建之前或期间定义:

`$ REACT_APP_SECRET_CODE=dev123 npm start`

`$ REACT_APP_SECRET_CODE=prod456 npm build`

值可以在 package.json 中指定，因此可以通过修订控制进行跟踪:

`"scripts": {`

`"start": "REACT_APP_SECRET_CODE=123 react-scripts start",`

`...`

`}`

虽然在 package.json 中设置设置是一种与团队共享配置的方便方法，但是这种方法不能很好地扩展。考虑增加配置变量或环境数量的后果。幸运的是，变量可以在一个名为. env 的单独文件中定义。

开发服务器和构建过程会自动从. env 文件中导入值。格式与 shell 脚本(name=value)中使用的格式相同，每行一个变量:

`$ cat .env`

`REACT_APP_SECRET_CODE=123`

`REACT_APP_HOST_ENV=staging`

`$`

在中存储定义的优势。env 文件包括以下内容:

*   更容易自动化。
*   文件的设置作为默认设置，可以使用上面列出的技术之一进行修改。因此，其他人可以手动调整变量，而无需修改。环境文件。这在下面描述的多环境安排中得到了利用。
*   Dotenv 用于在 create-react 应用程序中执行配置。请查阅 dotenv 的文档，了解更多关于它的语法和语义。

## 利用价值

自动将导入的值添加到 process.env。它们不需要手动导入。导入的值与 JS 文件中指定的值行为相同:

`if (process.env.NODE_ENV === 'production') { ... }`

`app.listen(process.env.REACT_APP_PORT);`

请记住，将只导入那些以 REACT _ APP _ 开头的值，并且必须重新启动开发服务器或构建以获取新值。

# 环境配置

现在，我们将了解如何使用 reactor-apps 可变支持为许多环境创建一个设置。目标是提供一个简单的方法来标准化和自动化在构建过程中使用环境特定数据的过程，而不管你的应用程序是如何构建的。

## 概观

1.  在。env 文件，定义变量(有或没有默认值)。
2.  在目标环境的生成目录的. env.local 文件中添加特定于环境的值/覆盖。
3.  创建和使用特定于环境的。如果需要，环境文件(例如，环境分段)。
4.  在。环境文件，定义变量。

虽然变量可以在 create-react-app 之外设置(例如，在 shell 启动脚本中)，但是为了准确性、可靠性和有效性，并使事情不那么怪异，我们应该选择一种在所有环境中普遍适用的方法——从本地开发到生产。这使得更广泛的团队子集能够在需要时有效地提供支持。换句话说，我们希望采用最有利于建立流程的策略，在该流程中:

*   配置值的来源要么是已知的，要么是容易辨别的。
*   配置和构建都是可跟踪和可复制的。
*   目前，这是通过 create-react-app 完成的。环境文件。

第一步是为您的项目创建一个. env 文件(如果它还没有),在其中保存您的设置，并更改对代码设置的引用。目前，我们假设这些是您的发展价值，尽管这将在以后进一步讨论。

此外，值得一提的是，通过利用。env 文件，变量不是环境的全局变量，在执行 start 命令后不会保留。在我看来，这是一个积极的发展。但是，由于 create-react-app 还不支持 dotenv-expand，这在某些情况下可能是个缺点(更新:从 1.1.0 开始，确实支持扩展)。

特定于环境的值存储在. env.local 中。

dotenv(react-scripts 导入环境变量的机制)还检查名为. env.local 的文件是否存在，如果找到，则导入其中声明的变量。当两个文件声明相同的值时，以下优先顺序适用(最高优先):

*   壳
*   env.local
*   。包封/包围（动词 envelop 的简写）
*   因此,. env.local 中的值会覆盖. env 中的值。

假设您使用中间服务器或子目录来构建每个环境，第 2 步是生成一个. env.local 文件。包含特定于环境的覆盖的 env 文件。

如果您在中使用通用默认值。对于本地开发，该文件可能为空。在其他情况下(例如，生产)，您通常希望手动指定每个值，因为修改必须更加小心地处理。

最后，应在修订控制忽略列表中添加一个条目(例如 gitignore ),因为它不应该被签入。如果模块是用. npmignore 文件发布的，它也应该包含在该文件中。

# 特定环境。环境文件

. env.local 方法非常有效，因为它确保所有环境都使用相同的方法来设置和使用环境配置。然而，它继续依赖于未跟踪的文件，这违背了再现性的目的。

解决这个问题的一个方法是创建。每个环境的 env 文件(例如. env.staging 而不是. env.local ),并在源代码控制中跟踪它们。在这种情况下，dotenv 不会自动导入文件；但是，可以更新 package.json 中的构建脚本来实现这一点(更新:从 1.0.0 开始，对于三个预定义的环境，这一步不再是必需的——文件是自动导入的):

`"scripts": {`

`"build": "sh -ac '. .env.build; react-scripts build'",`

`...`

`}`

该命令与源命令相同。这表明该文件将在当前上下文中执行。因此，其中包含的环境变量将在运行以下命令之前定义:react-scripts build。这与使用. env.local 时的优先级相同。

前面的方法可以概括为适应各种构建环境:

`"scripts": {`

`"build": "sh -ac '. .env.${REACT_APP_ENV}; react-scripts build'",`

`...`

`}`

现在，任何有权访问相关配置文件的人都可以通过在构建过程中设置 REACT APP ENV 来针对该环境进行开发:

$ REACT _ APP _ ENV =暂存 npm 运行构建

可以为每个环境添加其他快捷方式:

`"scripts": {`

`"build": "sh -ac '. .env.${REACT_APP_ENV}; react-scripts build'",`

`"build:staging": "REACT_APP_ENV=staging npm run build",`

`"build:production": "REACT_APP_ENV=production npm run build",`

`...`

`}`

将构建命令缩短为:

`$ npm run build: staging`

作为开发依赖项，按如下方式安装它:

$ yarn add-dev react-app-env(或 NPM install-save-dev)

并切换启动和构建脚本:

`"scripts": {`

`"start": "react-app-env start'",`

`"build": "react-app-env build'",`

`"test": "react-app-env test'",`

…

}

最后，至少包括两个环境变量:development.env 和 production.env。与前面的阶段不同，react-app-env 会自动在变量前面加上 REACT_APP_。因此，上例中变量的定义将更新为:

`SECRET_CODE=123`

`HOST_ENV=staging`

但是，在应用程序中，仍然必须使用变量的完整名称，包括 REACT_APP_。考虑到这一点，我希望库不要自动前置以避免误解，但我看不出有什么办法可以绕过它。

默认情况下，npm start 和 npm test 导入 development.env，而 npm build 导入 production . env。—env-file 选项可用于为其他环境提供支持:

`"scripts": {`

`"build:staging": "react-app-env --env-file=staging.env",`

`...`

`}`

的内容。env 仍然可以用作默认值，但是该文件中的变量不会自动加上 REACT_APP_。

最后，该模块假定默认环境文件位于项目的根目录中。要改变这一点(例如，支持单独的配置存储库)并添加其他环境，可以使用类似于上面描述的配置:

`"scripts": {`

`"build": "react-app-env --env-file=config/${BUILD_ENV}.env build",`

`"build:staging": "BUILD_ENV=staging npm run build",`

`"build:production": "BUILD_ENV=production npm run build",`

`...`

`}`

这种策略与上述策略的主要区别在于 REACT_APP_ 的自动前置，以及改进的跨平台支持。

内置支持 Create-react-app 1.0.0 包含了对三种预定义环境的自动配置文件读取:开发、测试和生产。

如前所述，默认(最低优先级)值可以在. env 中定义，NODE_ENV 的值由使用的脚本自动确定:

*   开始→发展
*   测试→测试
*   构建→生产

1.0.0 版包含对以下配置文件的内置支持:. env.development、. env.test 和. env.production。这些文件中的值优先于. env 中的值。每个文件(包括默认文件)都通过包含扩展名. local 来启用额外的覆盖层。以下是完整的优先顺序(最高优先):

1.  壳
2.  . env. {环境}。当地的
3.  . env. {环境}
4.  env.local
5.  。包封/包围（动词 envelop 的简写）

在 1.1.0 中，实现了对 dotenv-expand 的兼容性，允许在。env 文件(有关示例，请参见 create-react-app 说明):

`DOMAIN=[www.example.com](http://www.example.com/)`

`REACT_APP_FOO=$DOMAIN/foo`

`REACT_APP_BAR=$DOMAIN/bar`

变量扩展支持在本地(在同一个文件中)或 shell 上开发的模型，但不支持跨文件开发。例如，在中声明的值。不能在. env.development 中放大 env。

# 简单地

从一个有经验的 ReactJS 开发公司的角度来看，这种方法肯定是卓有成效的。这可以通过使用. env、. env.production 和. env.development 文件集合来实现。现在，react 开发人员可以很容易地知道您何时运行/构建您的应用程序。CRA 会将 NODE_ENV 环境变量设置为开发或生产，并将相应的。将基于这些值使用 env 文件。

如果你想提高 React 应用程序的性能，这里有一些你必须尝试的技巧。另外，请在评论中与我们分享你的想法，我们希望听到你的反馈。