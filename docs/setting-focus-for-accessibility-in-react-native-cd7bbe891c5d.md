# 在 React Native 中为可访问性设置焦点

> 原文：<https://blog.devgenius.io/setting-focus-for-accessibility-in-react-native-cd7bbe891c5d?source=collection_archive---------4----------------------->

有时候你需要将焦点放在一个元素上。在我的例子中，我遇到了一个问题，一个模态打开时给了用户更多的信息，但是当模态关闭时，焦点回到了屏幕的顶部，而不是停留在元素上。这是一个可访问性问题。

![](img/82a21613e5e555074b3b0604188aa8c7.png)

我将首先介绍您*可能*尝试实现的解决方案(在 React 中什么会起作用)，然后提供 React 本机键来关注元素的可访问性。

# 尝试 1: useRef()

如果你有一个功能组件，你可能会像在 React 中那样使用 useRef……然而，你可能会惊讶于你的尝试没有成功。

```
**import** React, { useRef } from 'react';

**const** EditableField = () => { **const** myElement = useRef(); **const** onModalClose = () => {   
      myElement.current.focus();
  }; **return (**     <View>
      <TouchableOpacity
        accessible
        ref={myElement}
        **onPress**{() => {
          Alert.alert(
            'I am an alert',
            'Which tells you more information',
            [{ text: 'Okay', onPress: **onModalClose** }],
            { cancelable: true },
          );
        }}
      >
      Click Me!
      </TouchableOpacity>
   </View>
  )
};
};
```

# 尝试 2: createRef

在我的例子中，我有一个大的类组件，所以我使用了 **createRef** 。我像往常一样为 React 组件编写代码。然而，这种方法不起作用。

```
**import** React, { createRef } from 'react';
**import** {  Alert, TouchableOpacity,  View } from 'react-native';renderElement() { **const** myElement = createRef(); **const** onModalClose = () => {
    if(myElement && myElement.current) {
        myElement.current.focus();
    }
  } **return** (
    <View>
      <TouchableOpacity
        accessible
        ref={myElement}
        **onPress**{() => {
          Alert.alert(
            'I am an alert',
            'Which tells you more information',
            [{ text: 'Okay', onPress: **onModalClose** }],
            { cancelable: true },
          );
        }}
      >
      Click Me!
      </TouchableOpacity>
   </View>
  )
};
```

# React Native 解决方案的关键

React 本地解决方案的关键是两个要素:

**findNodeHandle** :获取组件的原生节点句柄

**AccessibilityInfo** :识别设备当前是否有活动的屏幕阅读器。

使用这两个 react-native 元素，您可以像这样获取元素并设置焦点:

```
 **const** reactTag = findNodeHandle(myElement.current);
    if (reactTag) {
       AccessibilityInfo.setAccessibilityFocus(reactTag);
    }
```

# 功能组件解决方案:

```
**import** React, { **useRef** } from 'react';
**import** {  Alert, TouchableOpacity,  View,  AccessibilityInfo,  findNodeHandle } from 'react-native';**const** EditableField = () => { **const** myElement = **useRef**(); **const** onModalClose = () => {   
    if(myElement && myElement.current) {
        **const** reactTag = findNodeHandle(myElement.current);
        if (reactTag) {
          AccessibilityInfo.setAccessibilityFocus(reactTag);
        }
     }  
  };**return (
**     <View>
      <TouchableOpacity
        accessible
        ref={myElement}
        **onPress**{() => {
          Alert.alert(
            'I am an alert',
            'Which tells you more information',
            [{ text: 'Okay', onPress: **onModalClose** }],
            { cancelable: true },
          );
        }}
      >
      Click Me!
      </TouchableOpacity>
   </View>
  )
};
```

# 类组件的解决方案:

虽然这种方法可能在 web 中有效，但对于 React Native，有一种不同的方法。

```
**import** React, { **createRef** } from 'react';
**import** {  Alert, TouchableOpacity,  View,  AccessibilityInfo,  findNodeHandle } from 'react-native';renderElement() { **const** myElement = **createRef**(); **const** onModalClose = () => {
    if(myElement && myElement.current){ // null check to satisfy TS
        **const** reactTag = findNodeHandle(myElement.current);
        if (reactTag) {
          AccessibilityInfo.setAccessibilityFocus(reactTag);
        }
     }
    } **return** (
    <View>
      <TouchableOpacity
        accessible
        ref={ myElement }
        **onPress**{() => {
          Alert.alert(
            'I am an alert',
            'Which tells you more information',
            [{ text: 'Okay', onPress: **onModalClose** }],
            { cancelable: true },
          );
        }}
      >
      Click Me!
      </TouchableOpacity>
   </View>
  )
};
```