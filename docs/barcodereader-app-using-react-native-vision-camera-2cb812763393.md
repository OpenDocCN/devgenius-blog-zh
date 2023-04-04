# 使用 React 本机视觉摄像头的条形码阅读器应用程序。

> 原文：<https://blog.devgenius.io/barcodereader-app-using-react-native-vision-camera-2cb812763393?source=collection_archive---------0----------------------->

你可以找到许多博客反应原生条码阅读器应用程序，但所有这些都很旧，其中一些表现很差。不久前，我有机会为一家公司设计了一个，在这家公司里，性能是一个关键因素。在做了大量的研究、基准测试和大量的头脑风暴之后，我们可以做出一个性能非常好的应用。让我们开始吧。

![](img/b246347c95cc4504a00471d6c001ed91.png)

介绍图像

首先，我假设您已经准备好了标准的 react 本地模板。我将不涉及这一点，因为这是众所周知的。

现在，让我们继续进行所需的安装。我们将使用一个名为:react-native-vision-camera 的库。它是 react-native-camera 的替代品，现在是社区建议的标准相机。在这里阅读。

要安装:

```
yarn add react-native-vision-cameraornpm install react-native-vision-cameraorexpo install react-native-vision-camera
```

对于 ios 应用程序:

```
cd ios && pod install && cd ../
```

更新清单:

**安卓:**

打开项目的`AndroidManifest.xml`，在`<manifest>`标签中添加以下几行:

```
<uses-permission android:name="android.permission.CAMERA" />
```

**iOS:**

打开项目的`Info.plist`，在最外面的`<dict>`标签中添加以下代码行:

```
<key>NSCameraUsageDescription</key>
<string>$(PRODUCT_NAME) needs access to your Camera.</string><!-- optionally, if you want to record audio: -->
<key>NSMicrophoneUsageDescription</key>
<string>$(PRODUCT_NAME) needs access to your Microphone.</string>
```

**管理博览会:**

将 VisionCamera 插件添加到您的 Expo 配置(`app.json`、`app.config.json`或`app.config.js`):

```
{
  "name": "my app",
  "plugins": [
    [
      "react-native-vision-camera",
      {
        "cameraPermissionText": "$(PRODUCT_NAME) needs access to your Camera."
      }
    ]
  ]
}
```

最后，编译 mods:

```
expo prebuild
```

要应用这些更改，用 EAS 构建一个新的二进制文件:

```
eas build
```

由于我们将使用帧处理器来构建我们的条形码阅读器，我们将需要 react-native-reactive。让我们也安装它。

```
yarn add react-native-reanimatedor npm install react-native-reanimated
```

现在按照入门指南[进行所需的配置。](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/installation/)

为了避免相机由于权限而重新渲染，尽快请求权限总是一个好主意。比如，在主屏幕上询问权限，而不是在扫描屏幕上询问。我希望你明白我想表达的意思。

因此，在应用程序的主屏幕上，添加以下几行。

```
import {Camera} from 'react-native-vision-camera';/* Inside your component */const checkCameraPermission = async () => {
  let status = await Camera.getCameraPermissionStatus();
  if (status !== 'authorized') {
    await Camera.requestCameraPermission();
    status = await Camera.getCameraPermissionStatus();
    if (status === 'denied') {
      showToast(
        'You will not be able to scan if you do not allow camera access',
      );
    }
  }
};// class based components
componentDidMount() {
    .....
    .....
    checkCameraPermission();
  }// functional componentsuseEffect(() => {
   checkCameraPermission();
}, []);
```

现在，让我们转到实际的 ScanScreen，展示一些 UI 和逻辑部分。

我们将使用视觉摄像头代码扫描仪。还有其他可用的，但从我遇到的，这可能是最好的一个。然而，你也可以检查其他人。

为了更好的 UI 外观，我们也将使用 react-native-hole-view。让我们安装它们。

```
yarn add vision-camera-code-scanner react-native-hole-viewornpm install vision-camera-code-scanner react-native-hole-view// For iOS apps:cd ios && pod install && cd ../
```

现在将下面几行代码添加到您的代码中。

你现在都做完了。这既简单又高效。**关注**我获取更多此类内容。谢了。祝你有美好的一天！

干杯。✌🏻