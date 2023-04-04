# 原生反应—状态栏和图像样式

> 原文：<https://blog.devgenius.io/react-native-status-bar-and-image-styles-4f6f31e772b6?source=collection_archive---------5----------------------->

![](img/ad78871bd071e45f78aacfe8c15bc12d.png)

照片由塞尔吉奥·阿尔维斯·桑多斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React Native 是基于 React 的移动开发，我们可以用它来进行移动开发。

在本文中，我们将看看如何使用它来创建一个带有 React Native 的应用程序。

# 状态栏

`StatusBar`让我们用应用程序控制状态栏。

例如，我们可以写:

```
import React, { useState } from 'react';
import { Button, StyleSheet, StatusBar, View } from "react-native";
import Constants from "expo-constants";export default function App() {
  const [visibleStatusBar, setVisibleStatusBar] = useState(false);
  const changeVisibilityStatusBar = () => {
    setVisibleStatusBar(!visibleStatusBar);
  }; return (
    <View style={styles.container}>
      <StatusBar backgroundColor="blue" barStyle='dark-content' />
      <View>
        <StatusBar hidden={visibleStatusBar} />
      </View>
      <View style={styles.buttonContainer}>
        <Button title="Toggle StatusBar" onPress={() => changeVisibilityStatusBar()} />
      </View>
    </View>
  );
}const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: Constants.statusBarHeight,
  },
  scrollView: {
    flex: 1,
    backgroundColor: 'pink',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

我们增加了一个可以用`hidden`道具切换的`StatusBar`。

我们还添加了一个将`backgroundColor`设置为`'blue'`的。

`barStyle`设置状态栏样式，为`'dark-content'`。

也可以设置为`'default'`和`‘light-content’`。

# 形象风格道具

我们可以设置我们的图像显示我们想要的风格。

例如，我们可以写:

```
import React from 'react';
import { View, Image } from "react-native";export default function App() {
  return (
    <View>
      <Image
        style={{
          resizeMode: "cover",
          height: 100,
          width: 200
        }}
        source={{ uri: 'https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo' }}
      />
    </View>
  );
}
```

设置`width`和`height`。

`resizeMode`设置如何调整图像的大小。

我们可以将其设置为`'cover'`、`'contain'`、`'stretch'`、`'repeat'`或`'center'`。

此外，我们可以设置色调。例如，我们可以写:

```
import React from 'react';
import { View, Image } from "react-native";export default function App() {
  return (
    <View>
      <Image
        style={{
          tintColor: "lightgreen",
          resizeMode: "contain",
          height: 100,
          width: 200
        }}
        source={{ uri: 'https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo' }}
      />
    </View>
  );
}
```

然后我们可以看到图像是一个浅绿色的方框，而不是显示原始图像。

此外，我们可以在图像周围添加边框。

例如，我们可以写:

```
import React from 'react';
import { View, Image } from "react-native";export default function App() {
  return (
    <View>
      <Image
        style={{
          borderColor: "red",
          borderWidth: 5,
          height: 100,
          width: 200
        }}
        source={{ uri: 'https://reactnative.dev/docs/assets/p_cat2.png' }}
      />
    </View>
  );
}
```

我们将`borderColor`设置为`'red'`并将`borderWidth`设置为 5 来添加边框。

此外，我们可以通过编写以下内容为图像添加边框半径:

```
import React from 'react';
import { View, Image } from "react-native";export default function App() {
  return (
    <View>
      <Image
        style={{
          borderTopLeftRadius: 10,
          borderTopRightRadius: 10,
          borderBottomLeftRadius: 10,
          borderBottomRightRadius: 10,
          height: 100,
          width: 200
        }}
        source={{ uri: 'https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo' }}
      />
    </View>
  );
}
```

我们用这些属性为每个角的图像设置边界半径。

# 结论

我们可以用 React Native 控制状态栏和操作图像。