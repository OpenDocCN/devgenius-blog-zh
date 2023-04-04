# 理解 Magento 2 中的文件系统

> 原文：<https://blog.devgenius.io/understanding-the-file-system-in-magento-2-b1858826a0c7?source=collection_archive---------3----------------------->

![](img/c4697e6b8aee41102a218032678a6100.png)

> 开始组件开发的第一件事就是理解和设置文件系统。尽管所有组件都需要特定的文件，但每种类型的组件都有不同的文件结构。另外，可以选择组件根目录开始开发。以下部分提供了更多信息。— [Magento DevDocs —关于组件文件结构](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_intro.html)

# 在哪里可以找到 Javascript 和 PHTML 文件？

*   视图/前端/web/js
*   查看/前端/requirejs-config.js
*   视图/前端/布局
*   视图/前端/模板

# 组件根目录

一个组件根目录可以出现在两个地方，/app 和/vendor，它是那个组件的**顶层目录**。

**/app**

*   模块— *应用/代码*。
*   店面主题—*app/设计/前端*。
*   管理主题— *应用程序/设计/管理 html* 。
*   语言包— *app/i18n* 。

**/供应商**

*   模块— *供应商/【供应商名称】/【模块—模块名称】*
*   店面主题— *供应商/[供应商]/主题—[区域]-[主题名称]*
*   管理主题-*供应商/[供应商名称]/主题-管理 html-[主题名称]*
*   语言包— *供应商/[供应商名称]/language-en_us*

> 任何第三方组件(以及 Magento 应用程序本身)都被下载并存储在供应商目录下。如果您使用 Git 来管理项目，这个目录通常会添加到。gitignore 文件。因此，我们建议您在应用程序/代码中进行定制工作，而不是在供应商中。— [Magento DevDocs —创建您的组件文件结构](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/build/module-file-structure.html)

# 组件文件结构

组件需要 registration.php、etc/module.xml 和 composer.json 文件。这足以让 Magento 2 知道一个组件存在。参见我之前的博客[了解 Magento 2 基于模块的架构——模块注册](https://rickdaalhuizen.com/posts/understanding-magento-2-module-based-architecture/)

以下文件和文件夹是 Magento 2 模块(组件)中常见的。

*   **Api** —任何暴露于 Api 的 PHP 类(服务和数据服务契约)。
*   **块** — PHP 视图类作为模型视图控制器(MVC)模块逻辑垂直实现的一部分。
*   **控制台** —控制台命令
*   **控制器** — PHP 控制器类，作为模块逻辑的 MVC 垂直实现的一部分。
*   **控制器/管理 html** —管理控制器
*   **Cron** — Cron 作业类
*   **等** —配置文件；尤其是 module.xml，这是必需的。
*   **助手** —助手
*   **i18n**—CSV 格式的本地化文件
*   **模型** — PHP 模型类，作为模块逻辑的 MVC 垂直实现的一部分。
*   **模型/资源模型** —数据库交互
*   **观察者** —事件监听器
*   **插件** —包含任何需要的插件。
*   **Setup** —安装或升级时调用的模块数据库结构和数据设置类。
*   测试 —单元测试
*   **Ui** — UI 组件类
*   **查看** —查看文件，包括静态视图文件、设计模板、电子邮件模板和布局文件。
*   查看/{ area }/电子邮件
*   视图/{ area }/布局
*   视图/{ area }/模板
*   view/{area}/ui_component
*   view/{ area }/ui _ component/templates
*   查看/{area}/web
*   视图/{ area }/网站/模板
*   view/{ area }/require js-config . js

# 主题文件结构

Magento 主题通常位于*app/design/frontend/【Vendor】/*中，或者位于*Vendor/【Vendor】/theme-[area]-【theme-name】*中，但是从技术上来说可以放在任何地方。但是，主题必须放在单独的文件夹中。

典型的 magento 主题文件夹结构如下所示:

```
[theme_dir]/
├── [Vendor]_[Module]/
│   ├── web/
│   │   ├── css/
│   │   │   ├── source/
│   ├── layout/
│   │   ├── override/
│   ├── templates/
├── etc/
├── i18n/
├── media/
├── web/
│   ├── css/
│   │   ├── source/
│   ├── fonts/
│   ├── images/
│   ├── js/
├── composer.json
├── registration.php
├── theme.xml
```

要创建一个主题，你基本上只需要下面这些，这些是创建一个主题所需要的。

*   **composer.json** —描述主题依赖和元数据。如果您的主题是 composer 包，则需要。
*   registration.php 需要注册你的主题
*   **theme.xml** —它包含基本的元信息，比如主题标题和父主题名称

以下文件夹是可选的:

*   **布局/** —扩展默认模块或父主题布局的布局文件。
*   **布局/覆盖/基础** —覆盖默认模块布局的布局。
*   **布局/覆盖/ <父主题>** —覆盖模块父主题布局的布局。
*   **模板** —该目录包含主题模板，这些模板覆盖该模块的默认模块模板或父主题模板。自定义模板也存储在该目录中。
*   **etc/view.xml** —该文件包含所有店面产品图像和缩略图的配置。
*   **i18n**——。带翻译的 csv 文件。
*   **媒体** —这个目录包含一个主题预览(你的主题的截图)。
*   **web** —可以从前端直接加载的静态文件。
*   **网页/字体** —主题字体。
*   **网页/图片** —该主题中使用的图片。
*   **web/js** —主题 JavaScript 文件。
*   **web/css** —无主题文件
*   **web/css/source** —这个目录包含了调用 Magento UI 库中全局元素的 mixins 的无主题配置文件，以及覆盖默认变量值的无主题文件。
*   **web/css/source/lib** —查看覆盖存储在 lib/web/css/source/lib 中的 UI 库文件的文件

# 觉得这个帖子有用吗？请点击👏下面的按钮！:)