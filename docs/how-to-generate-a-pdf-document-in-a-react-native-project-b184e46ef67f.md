# 如何在 React 原生项目中生成 PDF 文档？

> 原文：<https://blog.devgenius.io/how-to-generate-a-pdf-document-in-a-react-native-project-b184e46ef67f?source=collection_archive---------1----------------------->

*不久前，UppLabs 正在进行一个* [***项目***](https://upplabs.com/upplabs-created-healthcare-app-for-preventing-hair-loss/) *该项目的关键功能包括在手机上生成 PDF。在这个由*[*Pavlo Ihnatiev*](https://medium.com/u/396872d0cc2f?source=post_page-----b184e46ef67f--------------------------------)*编写的材料中，我们将分享一些关于如何使用 React Native 生成 PDF 文档的技巧。*

![](img/37babe0977dc598e76c9a1d9de91a26f.png)

最初这份材料发表在 [UppLabs 的博客](https://upplabs.com/blog/how-to-generate-a-pdf-document-in-a-react-native-project/)上。

# 基础

在 React 原生项目中创建 PDF 文档的最简单方法包括使用 [**Expo Print**](https://docs.expo.io/versions/latest/sdk/print) 插件。

第一步是用应该在我们的 PDF 中的内容样本编写 HTML。您可以在标记中插入 CSS 样式、自定义字体、图像、链接等。

```
const htmlContent = `
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Pdf Content</title>
        <style>
            body {
                font-size: 16px;
                color: rgb(255, 196, 0);
            } h1 {
                text-align: center;
            }
        </style>
    </head>
    <body>
        <h1>Hello, UppLabs!</h1>
    </body>
    </html>
`;
```

然后，您应该编写函数，使用定义的 HTML 标记创建 PDF 文档:

```
import * as Print from 'expo-print';const htmlContent = `
....
`;const createPDF = async (html) => {
    try {
        const { uri } = await Print.printToFileAsync({ html });
        return uri;
    } catch (err) {
        console.error(err);
    }
};
```

**注意事项**:

此功能将创建一个 PDF 文件，并将其保存到应用程序缓存文件夹中。所以如果想替换或者复制这个文件，可以使用 [**Expo 文件系统**](https://docs.expo.io/versions/latest/sdk/filesystem) 。如果想保存到设备上的图库，可以使用 Android 平台的 [**Expo 媒体库**](https://docs.expo.io/versions/latest/sdk/media-library) 和 IOS 平台的 [**Expo 分享**](https://docs.expo.io/versions/latest/sdk/sharing) 。

允许您创建 PDF 并将其保存到图库的函数示例:

```
import * as Print from "expo-print";
import * as MediaLibrary from "expo-media-library";
import * as Sharing from "expo-sharing"; ....const createAndSavePDF = async (html) => {
  try {
    const { uri } = await Print.printToFileAsync({ html });
    if (Platform.OS === "ios") {
      await Sharing.shareAsync(uri);
    } else {
      const permission = await MediaLibrary.requestPermissionsAsync(); if (permission.granted) {
        await MediaLibrary.createAssetAsync(uri);
      }
    } } catch (error) {
    console.error(error);
  }
};
```

您可以在 React 本地组件中使用该功能，如果您需要创建一个包含简单内容的快速 PDF 文档，这就足够了。

# 高级案例

## PDF 页面边距

如果您使用函数**print . printtofilesync**，生成的 PDF 文档可能包含页边距(取决于 WebView 引擎)。它们由 **@page** 样式块设置，您可以在您的 HTML 内容代码中覆盖它们:

```
<style> 
   @page { margin: 20px; } 
</style>
```

## 避免破坏内容

如果您在 HTML 标记中插入几个内容部分，它们可能无法放在同一个文档页面上。在这种情况下，第二节的内容会中断，其中一部分内容会放到文档的下一页。但是当您希望避免破坏内容部分时，有时这种方法是不可接受的。

为了控制一些 HTML 元素的中断，你可以在你的 HTML 切片中使用[**【break-inside】**](https://css-tricks.com/almanac/properties/b/break-inside/)CSS 属性。

```
section {
  break-inside: avoid;
}
```

因此，如果两个部分不能放在同一个页面上，第二个部分将被放置在文档的下一页上而不会断开。

## 在 PDF 中使用字体、图像和其他资源

如果它被远程放置，您可以在 HTML 中使用自定义字体、图像或其他资源，没有任何麻烦。但如果你想使用 React 原生应用资产中的图像，这可能是一个问题，因为当你的 React 原生应用为生产而构建时，这样的图像不能被加载到你的文档中。在这种情况下，您应该首先将文件从 assets 复制到应用程序的缓存目录中。不幸的是，在 IOS 平台上，本地文件也不能加载到 PDF，所以你应该将本地文件转换成 **base64** 字符串。之后，你可以在你的 HTML 标记中使用复制文件的 URL(Android 平台上)或者 **base64** 字符串(IOS 平台上)。

在这种情况下，可以使用 [**Expo Asset**](https://docs.expo.io/versions/latest/sdk/asset/) 和[**Expo image manipulator**](https://docs.expo.io/versions/latest/sdk/imagemanipulator)。

在 PDF 文档中使用之前，从资产中复制图像并将其转换为 IOS 平台的 **base64** 的代码示例:

```
import { Platform } from "react-native";
import { Asset } from "expo-asset";
import * as ImageManipulator from "expo-image-manipulator"; ....const copyFromAssets = async (asset) => {
  try {
    await Asset.loadAsync(asset);
    const { localUri } = Asset.fromModule(asset); return localUri;
  } catch (error) {
    console.log(error);
    throw err;
  }
}; const processLocalImageIOS = async (imageUri) => {
  try {
    const uriParts = imageUri.split(".");
    const formatPart = uriParts[uriParts.length - 1];
    let format; if (formatPart.includes("png")) {
      format = "png";
    } else if (formatPart.includes("jpg") || formatPart.includes("jpeg")) {
      format = "jpeg";
    } const { base64 } = await ImageManipulator.manipulateAsync(
      imageUri,
      [],
      { format: format || "png", base64: true }
    ); return `data:image/${format};base64,${base64}`;
  } catch (error) {
    console.log(error);
    throw error
  }
};....const htmlContent = async () => {
    try {
        const asset = require('./assets/logo.png');
        let src = await copyFromAssets(asset);
        if(Platform.OS === 'ios') {
            src = await processLocalImageIOS(src);
        } return `<img src="${src}" alt="Logo" />`
    } catch (error) {
        console.log(error);
    }
}
```

因此，调用此代码后，您可以在 PDF 文档中使用资产图像的返回 URL。

## 大型图像的优化

如果您想要在 PDF 文档中放置大图像，您应该先对其进行优化。它可以大大减小您的 PDF 文档的大小。当您想要插入使用设备摄像头拍摄的图像时，优化图像非常有用。要优化图像，您可以使用[**Expo image manipulator**](https://docs.expo.io/versions/latest/sdk/imagemanipulator)。

**注意**:这个插件只能处理本地设备上的图片。你需要先下载图像。

优化大型图像的函数示例:

```
import * as ImageManipulator from 'expo-image-manipulator'; ....const optimizeImage = async (imageUri) => {
  try {
      const { uri } = await ImageManipulator.manipulateAsync(
        imageUri,
          [
              {
                  resize: {
                      width: 600,
                  },
              },
          ],
          { compress: 0.1 },
      ); return uri;
  } catch (error) {
      console.log(error);
      return imageUri;
  }
};
```

## 查例子！

[](https://expo.io/@upplabs/upplabs-pdf-example) [## 博览会上的 Upplabs PDF

### 描述扫描使用 Android 手机打开，您可以使用 Expo 移动应用程序扫描此二维码以加载此…

世博会](https://expo.io/@upplabs/upplabs-pdf-example) 

查看案例研究:

[](https://upplabs.com/portfolio/hairmax/) [## 毛发学家防止脱发的手机应用程序。UppLabs

### HairMax 是为毛发学家开发的移动应用程序，用于防止客户脱发。当客户…

upplabs.com](https://upplabs.com/portfolio/hairmax/) 

我们希望这篇文章是有用的！你可以在我们的网站上阅读更多关于 [**移动开发**](https://upplabs.com/expertise/mobile-development/) 和 [**向 UppLabs 寻求帮助**](https://upplabs.com/contacts/) 开发你的移动应用。