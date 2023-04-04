# 了解 Magento 2 基于模块的架构

> 原文：<https://blog.devgenius.io/understanding-magento-2-module-based-architecture-f0dac539c367?source=collection_archive---------6----------------------->

![](img/de5a966baba00db68c4206ddc2972c74.png)

先说什么是模块，模块怎么工作，怎么注册。

# 模块概述

Magento 2 文档解释了关于模块的以下内容:

> 模块是一个逻辑组，即包含块、控制器、助手、模型的目录，它们与特定的业务特性相关。为了与 Magento 对最佳模块化的承诺保持一致，一个模块封装了一个特性，对其他模块的依赖性最小。— [Magento DevDocs —模块概述](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_intro.html)
> 
> 模块和主题是 Magento 中的定制单元。模块提供支持逻辑的业务特性，而主题强烈影响用户体验和店面外观。这两个组件都有一个生命周期，允许安装、删除和禁用它们。从商家和扩展开发者的角度来看，模块是 Magento 组织的中心单元。— [Magento DevDocs —模块概述](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_intro.html)
> 
> Magento 框架提供了一组核心逻辑:PHP 代码、库以及由模块和其他组件继承的基本功能。— [Magento DevDocs —模块概述](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_intro.html)

简而言之:模块是一个逻辑组(文件夹),它包含了特定特性的业务逻辑。一个模块可以安装在两个地方。

1.  在 vendor(安装 composer 时):*vendor/[vendor]/[type]/[type]-[module-name]/[module-name]*。
2.  在应用程序中/(无编写器):

*   as 模块: *app/code/【供应商名称】/【模块名称】*
*   as 主题: *app/design/【类型】/【供应商名称】/【主题名称】*
*   as 语言包: *app/i18n/en_EN.csv*

当我们谈论“类型”时，我们谈论的是一个模块、主题(管理或前端主题)或语言包。

如果一个模块包含一个只与特定项目相关的特定定制，我们可以把它放在 *app/code/【厂商】/【类型】-【模块名】*。

通常，首先在 app/中开发我们的模块，当第一个“稳定”版本准备好时，将它转移到供应商中。

# 模块注册

Magento 组件，包括模块、主题和语言包，必须通过 Magento ComponentRegistrar 类在 Magento 系统中注册。— [Magento DevDocs —注册您的组件](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/build/component-registration.html)

当我们构建一个模块时，它必须被注册才能被 Magento 识别。这是通过 registration.php 的*中的 Magento [ComponentRegistrar 类](https://github.com/magento/magento2/blob/2.4-develop/lib/internal/Magento/Framework/Component/ComponentRegistrar.php)来完成的。您可以在模块的根目录下创建它。*

注册模块:

```
<?php
use \Magento\Framework\Component\ComponentRegistrar;
ComponentRegistrar::register(ComponentRegistrar::MODULE, '[Vendor_ModuleName]', __DIR__);
```

注册语言包:

```
<?php
use \Magento\Framework\Component\ComponentRegistrar;
ComponentRegistrar::register(ComponentRegistrar::LANGUAGE, '[Vendor_PackageName], __DIR__);
```

注册一个前端或管理主题，其中*【区域】*为前端或 adminhtml:

```
<?php
use \Magento\Framework\Component\ComponentRegistrar;
ComponentRegistrar::register(ComponentRegistrar::THEME, '[area]/[vendor]/[theme-name], __DIR__);
```

然后我们必须在/composer.json(模块的根目录)中自动加载我们的 registration.php。

```
{
    "name": [vendor]/[module-name]
    "autoload" {
        "psr-4": { "[Vendor]\\[ModuleName]\\": "" },
        "files": ["registration.php"]
    }
}
```

在我们创建了 registration.php 并将其添加到我们的 composer autoload 之后，我们需要创建一个 */etc/module.xml* 。它基本上告诉 Magento 它存在。

序列节点描述了模块必须加载的顺序。当你使用插件、偏好或其他依赖项(如布局)时，这可能很有用。

```
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="YourCompany_YourModule" setup_version="1.0.0">
        <sequence>
            <module name="TheirCompany_TheirModule" />
            <module name="AnotherCompany_AnotherModule" />
        </sequence>
    </module>
</config>
```

# 模块交互

Magento 2 中模块之间的交互方式是依赖注入、服务契约或数据服务契约。交互的一个重要方面是组件所在的范围。换句话说，一个 Magento 区域。

使用服务契约的一些好处是，这些契约确保了其他模块和第三方扩展可以实现定义良好的、持久的 API。此外，这些契约使得将服务配置为 web APIs 变得容易。

Magento docs: [服务合同剖析](https://devdocs.magento.com/guides/v2.2/architecture/archi_perspectives/service_layer.html#service-contract-anatomy)

# 模块限制和副作用

然而，当你在 Magento 2 中使用模块时，有一些限制应该被考虑。你应该使用依赖来表示一个模块依赖于另一个模块— [模块依赖](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_depend.html)

1.  一个模块负责一个特性。
2.  依赖于其他模块的模块必须使用 depends 在 *etc/modules.xml* 中明确声明。
3.  当一个模块被安装或删除时，它不应该影响其他模块。

一些常见的错误有:

*   [缺少类反射异常](https://laracasts.com/discuss/channels/laravel/reflectionexception-class-not-found-on-repository)
*   [类缺失类不存在](https://stackoverflow.com/questions/31364289/error-class-does-not-exist)
*   [PHP 警告:未捕获的错误:类丢失找不到类](https://dev.to/dechamp/php---how-to-fix-class--not-found-error-1gp9)

# Magento 地区

Magento 区域仅加载与该区域相关的组件。当加载一个组件时，您只需要查看请求发生的区域。这确保了更优化的过程。此外，它确保一个组件不会影响不同的区域。

> *您可以启用或禁用模块内的某个区域。如果启用了这个模块，它会将一个区域的路由器注入到一般应用程序的路由过程中。如果此模块被禁用，Magento 将不会加载一个区域的路由器，因此，一个区域的资源和特定功能将不可用。—* [*Magento DevDocs —模块和区域*](https://devdocs.magento.com/guides/v2.4/architecture/archi_perspectives/components/modules/mod_and_areas.html)

# Magento 区域类型

1.  **Adminhtml** —你在管理面板中看到的任何代码
2.  **前端** —店面(或前端)有你在前面看到的所有模板和布局文件。
3.  **Base** —这是没有 adminhtml 或前端区域时的回退。
4.  **Crontab** —位于 cron.php，加载了*\ Magento \ Framework \ App \ Cron*。
5.  **webapi_rest** —用于 REST APIs
6.  **webapi_soap** —用于 SOAP APIs

要了解区域是如何工作的，你可以找到代码[Magento/Framework/App/area . PHP](https://github.com/magento/magento2/blob/2.0/lib/internal/Magento/Framework/App/Area.php)