# 使用 Ionic 和 React 进行移动开发——搜索栏、分段显示和下拉菜单

> 原文：<https://blog.devgenius.io/mobile-development-with-ionic-and-react-search-bar-segment-display-and-dropdowns-1da5230a098?source=collection_archive---------1----------------------->

![](img/7488456845e5677f90ddad6052b90f56.png)

照片由塞尔吉奥·阿尔维斯·桑多斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果我们知道如何创建 React web 应用程序，但希望开发移动应用程序，我们可以使用 Ionic 框架。

在本文中，我们将看看如何使用带有 React 的 Ionic 框架开始移动开发。

# 搜索栏

我们可以添加一个带有`IonSearchbar`组件的搜索栏。

例如，我们可以写:

```
import React, { useState } from 'react';
import { IonContent, IonSearchbar, } from '@ionic/react';
import './Tab1.css';const Tab1: React.FC = () => {
  const [searchText, setSearchText] = useState(''); return (
    <IonContent>
      <IonSearchbar value={searchText} onIonChange={e => setSearchText(e.detail.value!)}></IonSearchbar>
    </IonContent>
  );
};export default Tab1;
```

我们添加了`IonSearchbar`组件来添加搜索栏。

`onIonChange`具有获取输入值的功能，我们可以用它来设置一个状态的值。

`e.detail.value`有输入值。

在我们调用`onIonChange`回调函数中的`setSearchText`后，`searchText`已经有了输入的值。

`value`被设定为输入值。

此外，我们可以显示一个取消按钮来清除搜索文本:

```
import React, { useState } from 'react';
import { IonContent, IonSearchbar, } from '@ionic/react';
import './Tab1.css';const Tab1: React.FC = () => {
  const [searchText, setSearchText] = useState(''); return (
    <IonContent>
      <IonSearchbar
        showCancelButton="focus"
        cancelButtonText="Custom Cancel"
        value={searchText}
        onIonChange={e => setSearchText(e.detail.value!)}
      >
      </IonSearchbar>
    </IonContent>
  );
};export default Tab1;
```

# 段

我们可以使用`IonSegment`组件添加分段显示。

例如，我们可以写:

```
import React from 'react';
import { IonContent, IonLabel, IonSegment, IonSegmentButton } from '@ionic/react';
import './Tab1.css';const Tab1: React.FC = () => {
  return (
    <IonContent>
      <IonSegment onIonChange={e => console.log('Segment selected', e.detail.value)}>
        <IonSegmentButton value="friends">
          <IonLabel>Friends</IonLabel>
        </IonSegmentButton>
        <IonSegmentButton value="enemies">
          <IonLabel>Enemies</IonLabel>
        </IonSegmentButton>
      </IonSegment>
    </IonContent>
  );
};export default Tab1;
```

我们可以添加`IonSegment`来添加段。

`IonSegmentButton`组件将一个按钮添加到段显示中。

然后，按钮将自动调整大小以适合分段显示。

# 选择下拉菜单

我们可以用`IonSelect`组件在我们的应用程序中添加一个选择下拉菜单。

例如，我们可以写:

```
import React, { useState } from 'react';
import { IonContent, IonItem, IonLabel, IonSelect, IonSelectOption } from '@ionic/react';
import './Tab1.css';const Tab1: React.FC = () => {
  const [hairColor, setHairColor] = useState<string>('brown'); return (
    <IonContent>
      <IonItem>
        <IonLabel>Hair Color</IonLabel>
        <IonSelect value={hairColor} okText="Okay" cancelText="Dismiss" onIonChange={e => setHairColor(e.detail.value)}>
          <IonSelectOption value="brown">Brown</IonSelectOption>
          <IonSelectOption value="blonde">Blonde</IonSelectOption>
          <IonSelectOption value="black">Black</IonSelectOption>
          <IonSelectOption value="red">Red</IonSelectOption>
        </IonSelect>
      </IonItem>
    </IonContent>
  );
};export default Tab1;
```

添加一个下拉菜单让我们选择头发的颜色。

我们将它放在`IonItem`组件中，这样我们就可以在左侧获得标签，在右侧获得下拉列表。

`IonSelect`组件有`value`来获取值。

`okText`具有为确定按钮显示的文本。

`cancelText`具有为取消按钮显示的文本。

`IonSelectOption`有选择选项。

当我们选择一个下拉项目时，运行`onIonChange`的回调。

`e.detail.value`有下拉框的选定值。

# 结论

我们可以添加一个搜索栏，分段显示，并用 Ionic React 选择下拉菜单。