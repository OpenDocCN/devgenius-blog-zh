# 深度嵌套且可能未定义—类型脚本索引类型

> 原文：<https://blog.devgenius.io/deeply-nested-and-possibly-undefined-typescript-index-types-baa7d6802028?source=collection_archive---------3----------------------->

大家好！感谢您的加入！

今天，我想分享一个在 TypeScript 中快速处理索引/查找类型的小家伙——特别是在处理查找深度嵌套且可能未定义的类型时。

所以让我们进入未知的世界吧！

![](img/0b73f0cd285c1a4e9af5b71e5dd69ebe.png)

# 创建类型

## 界面

有时，您可能会处理具有嵌套属性的接口，甚至可能处理数组和对象。对我来说，我在处理由`graphql-codegen`生成的类型并将部分属性传递给 React 组件时遇到过这种情况。

让我们看看这个啤酒厂的示例界面，因为今天是周六。

```
interface Brewery {
  name: string;
  location: {
    address: string;
    addressLineTwo: string;
    city: string;
    state: string;
    zip: number;
  };
  products?: {
    name: string;
    type: "IPA" | "Stout";
    attributes?: {
      hops: { name: string; dryHopped?: boolean }[];
      malt: string;
      ibu: number;
      abv: number;
    };
  }[];
}
```

注意上面，这个接口的一些属性可能是未定义的，包括`products`和`products.attributes`。

# 示例函数

因此，让我们定义一些接受`brewery`类型的*部分*的函数，作为使用它的半实际例子。这样，我们就不必在不需要的时候将整个 brewery 对象从一个函数传递到另一个函数。

## 根据所需属性定义-格式名称

下面的 format name 函数将只接受*啤酒厂`name`的属性，并对其进行一些简单的格式化。为此，我们将使用上面的接口和查找类型键入参数`breweryName`。*

```
const formatBreweryName = (breweryName: Brewery["name"]) => {
  const formatted = breweryName.toUpperCase(); return formatted;
};
```

属性`name`在类型`Brewery`中是必需的，这使得这个参数有点容易定义。

## 从所需的对象定义—格式地址

同样，`address`属性是必需的，这使得定义下面的`breweryAddress`参数相当容易。

```
const formatAddress = (breweryLocation: Brewery["location"]) => {
  const formatted = `${breweryLocation.address} ${breweryLocation?.addressLineTwo}, ${breweryLocation.city}, ${breweryLocation.state} ${breweryLocation.zip}`; return formatted;
};
```

## 从嵌套和未定义的数组中定义—列出产品名称

根据上面的类型，`products`属性是一个数组，可能没有定义，但是我们不需要做任何改变来定义`breweryProducts`。

```
const listProductNames = (breweryProducts: Brewery["products"]) => {
  const productNames = breweryProducts?.map((product) => product.name); return productNames;
};
```

## 从潜在未定义的对象定义-提取名称和类型

当访问数组中潜在的未定义对象的类型时，我们需要做一些调整。下面我们将访问`products`数组中的对象类型来定义`breweryProduct`参数。

使用 TypeScript 提供的`NonNullable`实用程序来定义该类型。

```
const extractNameAndType = (
  breweryProduct: NonNullable<Brewery["products"]>[0]
) => {
  const name = breweryProduct.name.toUpperCase();
  const type = breweryProduct.type.toUpperCase(); return { name, type };
};
```

## 从潜在未定义的嵌套对象定义—格式跃点名称

让我们更深入，更不明确。下面的`hops`参数是潜在未定义的`attributes`的一部分，而`attributes`是潜在未定义的`products`的一部分。

嵌套`NonNullable`实用程序类型来访问这个类型。

```
const formatHopsName = (
  hops: NonNullable<
    NonNullable<Brewery["products"]>[0]["attributes"]
  >["hops"][0]
) => {
  const formatted = hops.name.toUpperCase(); return formatted;
};
```

# 就是这样！

今天是一个快速的问题，有一些非常基本的例子。希望对你有点用！

具体来说，我用它来传递一个嵌套的、可能未定义的对象的属性，作为其他 React 组件的道具。真的，这是另一个故事的对话——如果你想进一步了解细节，请告诉我！

如果你今天读到这篇文章，祝你周六愉快…如果不是，那就享受一个寒冷的周六吧(只要你成年了！！).

嘿你们！感谢您花几分钟时间阅读上面的提示和技巧。我希望它在某种程度上有所帮助！

就文章而言，本周将会很有趣！当我还在从两周前的前交叉韧带手术中恢复的时候，我已经为你计划了一些。我将发布构建 Apollo 联合 API 教程和 Starter Repo 的第三部分——这将是账户服务！

此外，我可能有一个快速反应项目给你，建立一个文件管理器模块！

下次见，

尼克·M