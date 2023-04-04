# 如何在 React 原生应用中滚动特定部分

> 原文：<https://blog.devgenius.io/how-to-scroll-specific-section-in-your-react-native-app-25d0af30879c?source=collection_archive---------3----------------------->

![](img/02dd794048f88134054764890c5b1379.png)

对于前端开发人员来说，每当用户做一个动作，其中之一是滚动到屏幕上的特定视图时，设置视图的任何部分都是一件痛苦的事情，开发人员开始寻找解决方案，最终发现一些没有很好解释的例子，这只会使事情变得更糟，今天我们将通过三个步骤来解决这个问题，并将清楚地解释所有代码，即使没有任何经验的人也能理解代码。所以让我们开始吧。

```
*Main.tsx File*const [dataSourceCords, setDataSourceCords] = useState([] as number[]);const [scrollToIndex, setScrollToIndex] = useState(0);const [ref, setRef] = useState<ScrollView>(); *// create ref*<ScrollView
  ref={ref => {setRef(ref as any); *//set the ref*
}}>
<View
key={1} //keys will be needed for function
onLayout={event => {
const layout = event.nativeEvent.layout;
dataSourceCords[1] = layout.y; *// we store this offset values in an array*
}}
//You can render any component here
</View>

<View
key={2} //keys will be needed for function 
onLayout={event => {  
const layout = event.nativeEvent.layout;
dataSourceCords[2] = layout.y; *// we store this offset values in an          array*
}}*//You can render any component here*</View></ScrollView>
```

我们创建了一个 Main.tsx 文件，它有两个视图，你可以在这个视图中渲染任何你想渲染的东西，我们给了每个视图重要的键码，这些键码将在函数中使用。onLayout 函数，它给我们这个视图从顶部的偏移值，并且是我们将要使用的滚动函数所需要的。

在我们存储了偏移值并将 ref 设置为 scrollview 之后，我们就可以设置滚动函数来使用它了。

```
*ScrollHandler.ts*
export const scrollHandler = (key: number) => { 
if (dataSourceCords.length > scrollToIndex) {ref?.scrollTo({
x: 0,
y: dataSourceCords[key], *//we get the offset value from array based on key*
animated: true,});
}};
```

上面的函数会处理剩下的，我们唯一要做的就是用任何有 onPress 函数的元素调用这个函数。下面我们只需对 Main.tsx 文件做一些修改，就可以开始了。

```
*Main.tsx File* ***Changed***import scrollHandler from './utils' *// import the function*const sectionNames = [  *// create dummy data so you can render two button*
{ value: 1, text: 'Section 1' },  
{ value: 2, text: 'Section 2' },

]; const [dataSourceCords, setDataSourceCords] = useState([] as number[]);const [scrollToIndex, setScrollToIndex] = useState(0);const [ref, setRef] = useState<ScrollView>(); *// create ref*<ScrollView
  ref={ref => {setRef(ref as any); *//set the ref*
}}>
<View>   // just add an view 
{sectionNames.map(item => { *//render the dummy data* 
    return (
    <TouchableOpacity       // return an button
        key={item.value}
        onPress={() => scrollHandler(item.value)} *//call the function* >
          <Text style={{color: constant.colorPrimary, 
           fontSize:15,}}>{item.text}</Text>
    </TouchableOpacity>);})}
 </View><View
key={1} //keys will be needed for function
onLayout={event => {
const layout = event.nativeEvent.layout;
dataSourceCords[1] = layout.y; *// we store this offset values in an array*
}}
//You can render any component here
</View>

<View
key={2} //keys will be needed for function 
onLayout={event => {  
const layout = event.nativeEvent.layout;
dataSourceCords[2] = layout.y; *// we store this offset values in an          array*
}}*//You can render any component here*</View></ScrollView>
```

就这样，我们有两个视图，我们可以在里面渲染任何东西，我们在顶部有两个按钮，可以滚动到这两个部分。请检查代码中的注释，我会尽可能多地解释代码。

希望对你有帮助。如果是这样的话，给我一个关注和喜欢:)

检查我的其他文章。

[1-)如何在 React JS 中隐藏网站源代码？](https://medium.com/@akinkarayun/how-to-hide-website-source-code-in-react-js-77164d474324)

[2-)常见的反应本国问题及其解决方法](https://javascript.plainenglish.io/common-react-native-problems-and-solutions-22a1076e4589)

[3-)如何在你的 react 原生应用中渲染图像数组长度的点。](/how-to-render-dots-based-on-that-image-array-length-has-in-your-react-native-app-76dac14763e4)

我的链接在下面

[AKIN KARAYUN | LinkedIn](https://www.linkedin.com/in/akin-karayun-ab3239bb/)

[Akin Kara yun(devankarayun . web . app)](https://devakinkarayun.web.app/)