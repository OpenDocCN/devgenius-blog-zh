# React Native —部分列表和后退按钮处理

> 原文：<https://blog.devgenius.io/react-native-section-list-and-back-button-handling-83c44dba713f?source=collection_archive---------6----------------------->

![](img/a411c0117881c41b414b0b25836d3c58.png)

Brandon Hoogenboom 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Native 是基于 React 的移动开发，我们可以用它来进行移动开发。

在本文中，我们将看看如何使用它来创建一个带有 React Native 的应用程序。

# 章节列表

我们可以添加`SectionList`组件来显示部分列表视图。

它是跨平台的，支持水平模式。

此外，它还支持页眉、页脚和分隔符。

我们可以拉出来刷新数据。

我们可以一边滚动一边加载数据。

它还支持多列。

例如，我们可以写:

```
import React from 'react';
import { SafeAreaView, StyleSheet, Text, View, SectionList } from 'react-native';
const DATA = [
  {
    title: "Main dishes",
    data: ["Pizza", "Burger", "Risotto"]
  },
  {
    title: "Sides",
    data: ["French Fries", "Onion Rings", "Fried Shrimps"]
  },
  {
    title: "Drinks",
    data: ["Water", "Coke", "Beer"]
  },
  {
    title: "Desserts",
    data: ["Cheese Cake", "Ice Cream"]
  }
];
const Item = ({ title }) => (
  <View style={styles.item}>
    <Text style={styles.title}>{title}</Text>
  </View>
);
export default function App() {
  return (
    <SafeAreaView style={styles.container}>
      <SectionList
        sections={DATA}
        keyExtractor={(item, index) => item + index}
        renderItem={({ item }) => <Item title={item} />}
        renderSectionHeader={({ section: { title } }) => (
          <Text style={styles.header}>{title}</Text>
        )}
      />
    </SafeAreaView>
  );
}const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: 0
  },
  item: {
    backgroundColor: 'pink',
    padding: 20,
    marginVertical: 8,
    marginHorizontal: 16,
  },
  title: {
    fontSize: 32,
  },
});
```

我们添加了`SafeAreaView`和`SectionList`来显示一个列表，该列表带有部分标题，每个部分都有自己的内容。

`sections`道具有数据。

`keyExtractor`是一个返回项目唯一索引的函数。

`renderItem`是渲染物品的函数。

`renderSectionHeader`属性用一个函数来呈现标题。

# 后台处理程序

我们可以使用 BackHandler API 来处理后退按钮按压。

例如，我们可以写:

```
import React, { useEffect } from 'react';
import { Text, View, StyleSheet, BackHandler, Alert } from "react-native";export default function App() {
  useEffect(() => {
    const backAction = () => {
      Alert.alert("Are you sure?", "Are you sure you want to go back?", [
        {
          text: "Cancel",
          onPress: () => null,
          style: "cancel"
        },
        { text: "YES", onPress: () => BackHandler.exitApp() }
      ]);
      return true;
    }; const backHandler = BackHandler.addEventListener(
      "hardwareBackPress",
      backAction
    ); return () => backHandler.remove();
  }, []); return (
    <View style={styles.container}>
      <Text style={styles.text}>Click Back button!</Text>
    </View>
  );
}const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center"
  },
  text: {
    fontSize: 18,
    fontWeight: "bold"
  }
});
```

我们为带有`BackHandler.addEventListener`的`hardwareBackPress`事件附加一个事件处理程序。

在`backAction`事件处理函数中，我们用`Alert.alert`方法显示了一个警告。

它将警报的标题和内容作为参数。

第三个参数是一个数组，其中的对象包含 cancel 按钮的内容。

`text`属性是按钮的文本。

`onPress`是我们按下按钮时运行的功能。

`style`有按钮的样式。

当组件卸载时，我们调用`backHandler.remove()`来移除`hardwareBackPress`监听器。

# 结论

我们可以添加`SectionList`组件，为每个部分添加一个带有部分标题和列表项的列表。

此外，我们可以为 back 按钮添加一个处理程序。