# 本地反应—抽屉和权限

> 原文：<https://blog.devgenius.io/react-native-drawer-and-permissions-28c931850194?source=collection_archive---------6----------------------->

![](img/f32774d1576ee5253195874cb8bd221c.png)

[CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

React Native 是基于 React 的移动开发，我们可以用它来进行移动开发。

在本文中，我们将看看如何使用它来创建一个带有 React Native 的应用程序。

# DrawerLayoutAndroid

我们可以用`DrawerLayoutAndroid`组件给我们的 Android 应用程序添加一个抽屉布局。

例如，我们可以写:

```
import React, { useState } from 'react';
import { Button, DrawerLayoutAndroid, Text, StyleSheet, View } from "react-native";export default function App() {
  const [drawerPosition, setDrawerPosition] = useState("left");
  const changeDrawerPosition = () => {
    if (drawerPosition === "left") {
      setDrawerPosition("right");
    } else {
      setDrawerPosition("left");
    }
  }; const navigationView = (
    <View style={styles.navigationContainer}>
      <Text style={{ margin: 10, fontSize: 15 }}>I'm in the Drawer!</Text>
    </View>
  ); return (
    <DrawerLayoutAndroid
      drawerWidth={300}
      drawerPosition={drawerPosition}
      renderNavigationView={() => navigationView}
    >
      <View style={styles.container}>
        <Text style={{ margin: 10, fontSize: 15 }}>
          DrawerLayoutAndroid example
        </Text>
        <Button
          title="Change Drawer Position"
          onPress={() => changeDrawerPosition()}
        />
        <Text style={{ margin: 10, fontSize: 15 }}>
          Drawer on the {drawerPosition}! Swipe from the side to see!
        </Text>
      </View>
    </DrawerLayoutAndroid>
  );
}const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center",
    paddingTop: 50,
    backgroundColor: "#ecf0f1",
    padding: 8
  },
  navigationContainer: {
    flex: 1,
    paddingTop: 50,
    backgroundColor: "#fff",
    padding: 8
  }
});
```

我们用`drawerPosition`状态来改变抽屉的位置。

`Button`使用`onPress`道具，该道具具有调用`changeDrawerPosition`来切换抽屉位置的功能。

`DrawerLayoutAndroid`组件有抽屉。

`drawerWidth`有抽屉的宽度。

`drawerPosition`设定抽屉的位置。

`renderNavigationView`是渲染抽屉内容的函数。

然后当我们向左或向右拖动时，我们会看到抽屉。

# 许可和类固醇

我们可以添加`PermissionsAndroid`对象，让我们在应用程序中请求权限。

例如，我们可以写:

```
import React from 'react';
import { StyleSheet, Text, View, PermissionsAndroid, Button } from "react-native";
import Constants from "expo-constants";const requestCameraPermission = async () => {
  try {
    const granted = await PermissionsAndroid.request(
      PermissionsAndroid.PERMISSIONS.CAMERA,
      {
        title: "Camera Permission",
        message:
          "Can I use the Camera?",
        buttonNeutral: "Ask Me Later",
        buttonNegative: "Cancel",
        buttonPositive: "OK"
      }
    );
    if (granted === PermissionsAndroid.RESULTS.GRANTED) {
      console.log("You can use the camera");
    } else {
      console.log("Camera permission denied");
    }
  } catch (err) {
    console.warn(err);
  }
};
export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.item}>Try permissions</Text>
      <Button title="request permissions" onPress={requestCameraPermission} />
    </View>
  );
}const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    paddingTop: Constants.statusBarHeight,
    backgroundColor: "#ecf0f1",
    padding: 8
  },
  item: {
    margin: 24,
    fontSize: 28,
    fontWeight: "bold",
    textAlign: "center"
  }
});
```

我们添加了调用`PermissionAndroid.request`方法来请求权限的`requestCameraPermission`函数。

`PermissionsAndroid.PERMISSIONS.CAMERA`参数意味着我们想要获得摄像机的权限。

第二个参数包含一个具有某些属性的对象。

`title`是权限请求对话框的标题。

`message`是权限请求对话框的消息。

`buttonNeutral`是为中性选项显示的文本。

`buttonNegative`是拒绝权限按钮的文本。

`buttonPosition`是接受许可按钮的文本。

它返回一个带有权限选择的承诺。

我们可以通过对照`PermissionsAndroid.RESULTS.GRANTED`来检查它是否被授予。

# 结论

我们可以添加一个抽屉，并向 React 本地库请求权限。