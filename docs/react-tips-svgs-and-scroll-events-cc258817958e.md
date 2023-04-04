# 反应提示—SVG 和滚动事件

> 原文：<https://blog.devgenius.io/react-tips-svgs-and-scroll-events-cc258817958e?source=collection_archive---------7----------------------->

![](img/3527b1438d16c86a49778d7c00383f27.png)

照片由[陈彦蓉](https://unsplash.com/@spdumb2025?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 将 SVG 嵌入 ReactJS

我们可以将 SVG 代码直接放入 React 组件中

例如，我们可以写:

```
function SvgApple() {
  return (
       <svg id="svg2" viewBox="0 0 624.38 604.71" version="1.0">
      <defs id="defs3">
        <linearGradient
          id="linearGradient2847"
          x1="340.66"
          gradientUnits="userSpaceOnUse"
          y1="322.86"
          gradientTransform="translate(164.44 185.74)"
          x2="398.88"
          y2="322.86"
        >
          <stop id="stop2831" stop-color="#ccc" offset="0" />
          <stop id="stop2833" stop-color="#ccc" stop-opacity="0" offset="1" />
        </linearGradient>
        <radialGradient
          id="radialGradient2849"
          gradientUnits="userSpaceOnUse"
          cy="586.46"
          cx="366.79"
          gradientTransform="matrix(1 0 0 .90909 246.44 239.05)"
          r="258.49"
        >
          <stop id="stop2807" stop-color="#e6e6e6" offset="0" />
          <stop
            id="stop2809"
            stop-color="#e6e6e6"
            stop-opacity="0"
            offset="1"
          />
        </radialGradient>
        <radialGradient
          id="radialGradient2851"
          gradientUnits="userSpaceOnUse"
          cy="274.22"
          cx="490.32"
          gradientTransform="matrix(1 0 0 .61041 168.44 290.57)"
          r="123.39"
        >
          <stop id="stop2821" stop-color="#ccc" offset="0" />
          <stop id="stop2823" stop-color="#ccc" stop-opacity="0" offset="1" />
        </radialGradient>
      </defs>
      <g id="layer1" transform="translate(-89.704 -231.29)">
        <g
          id="g2837"
          transform="matrix(.93776 .34729 -.34729 .93776 206.95 -108.99)"
        >
          <path
            id="path10856"
            stroke="#000"
            transform="translate(731.28 508.81)"
            d="m-320.82 328.99c1.62-1.61-22.63-13.81-20.59-14.85 2.03-1.04-2.35 25.75-0.09 25.39 2.25-0.36-10.2-24.47-7.94-24.12 2.26 0.36-17.03 19.45-15 20.49 2.04 1.04 6.14-25.79 7.76-24.18 1.62 1.62-25.21 5.73-24.17 7.76 1.03 2.04 20.12-17.25 20.48-15 0.36 2.26-23.76-10.19-24.12-7.93-0.35 2.26 26.43-2.13 25.39-0.09-1.03 2.03-13.23-22.21-14.85-20.6-1.61 1.62 22.63 13.81 20.6 14.85-2.04 1.04 2.35-25.75 0.09-25.39s10.19 24.48 7.94 24.12c-2.26-0.36 17.03-19.45 14.99-20.49-2.03-1.03-6.14 25.79-7.75 24.18-1.62-1.62 25.21-5.72 24.17-7.76s-20.13 17.26-20.48 15c-0.36-2.26 23.76 10.19 24.11 7.93 0.36-2.25-26.42 2.13-25.39 0.1 1.04-2.04 13.24 22.21 14.85 20.59z"
          />
          <path
            id="path10853"
            stroke="#000"
            fill="red"
            d="m625.29 586.46c0 138.74-115.81 235-258.5 235-142.68 0-258.49-96.26-258.49-235s115.81-234.99 258.49-234.99c142.69 0 258.5 96.25 258.5 234.99z"
          />
          <path
            id="rect10858"
            stroke="#000"
            stroke-width=".62806"
            fill="#803300"
            d="m381.01 236.85c8.59 0 17.56 0.03 17.56 0.07l-14.31 171.88c0 0.04-6.92 0.07-15.51 0.07s-15.51-0.03-15.51-0.07l-12.26-171.88c0-0.04 31.44-0.07 40.03-0.07z"
          />
          <path
            id="path2827"
            opacity=".6729"
            fill="url(#linearGradient2847)"
            d="m381.01 236.85c8.59 0 17.56 0.03 17.56 0.07l-14.31 171.88c0 0.04-6.92 0.07-15.51 0.07s-15.51-0.03-15.51-0.07l-12.26-171.88c0-0.04 31.44-0.07 40.03-0.07z"
          />
          <path
            id="path10861"
            stroke="#000"
            stroke-width="3.063"
            fill="none"
            d="m337 394.28c2.62 5 12.11 14.08 19.7 14.09 7.77 0.02 27.78 0.58 29.61-1.57 3-3.54 10.81-15.89 12.33-20.76"
          />
          <path
            id="path10868"
            stroke="#000"
            stroke-width=".35974"
            fill="#338000"
            d="m538.62 313.49c-56.1 31.56-164.82 43.23-171.29 31.7-6.46-11.54 64.93-89.87 121.02-121.43 56.1-31.56 117.89-28.15 124.35-16.61 6.46 11.53-17.99 74.78-74.08 106.34z"
          />
          <path
            id="path2803"
            opacity=".40654"
            fill="url(#radialGradient2849)"
            d="m707.29 586.46c0 138.74-115.81 235-258.5 235-142.68 0-258.49-96.26-258.49-235s115.81-234.99 258.49-234.99c142.69 0 258.5 96.25 258.5 234.99z"
          />
          <path
            id="path2817"
            opacity=".78364"
            fill="url(#radialGradient2851)"
            d="m542.62 311.49c-56.1 31.56-164.82 43.23-171.29 31.7-6.46-11.54 64.93-89.87 121.02-121.43 56.1-31.56 117.89-28.15 124.35-16.61 6.46 11.53-17.99 74.78-74.08 106.34z"
          />
        </g>
      </g>
    </svg>
  );
}
```

我们添加了一个来自[https://publicdomainvectors . org/en/free-clip art/Apple-vector-drawing/2168 . html](https://publicdomainvectors.org/en/free-clipart/Apple-vector-drawing/2168.html)的苹果 SVG，删除了原始 SVG 代码中的所有命名空间标签。

# 在 React 中更新滚动组件的样式

我们可以通过在组件中附加一个`scroll`事件监听器来监听滚动事件。

例如，我们可以写:

```
class App extends React.Component {
  constructor(){
    this.state = {}
  } componentDidMount() {
    window.addEventListener('scroll', this.handleScroll);
  }, componentWillUnmount() {
    window.removeEventListener('scroll', this.handleScroll);
  }, handleScroll() {
    const scrollY = window.scrollY; this.setState({
      scrollY
    });
  }

  render(){
    return <div>{this.state.scrollY}</div>;
  }
}
```

我们在`componentDidMount`方法中附加了带有`addEventListener`方法的`scroll`事件监听器，以便在组件挂载时添加事件监听器。

然后我们调用`removeEventListener`来移除监听器。

`handleScroll`是我们的倾听者。

我们用`window.scrollY`属性得到滚动距离。

我们调用`setState`来设置`scrollY`状态，这样我们就可以在组件的任何地方使用它。

在`componentWillUnmount`方法中，当组件卸载时，我们移除`scroll`事件监听器。

如果我们有一个函数组件，我们写:

```
import React, { useState, useEffect } from "react"const ScrollingElement = () => {
  const [scrollY, setScrollY] = useState(0); function handleScroll() {
    setScrollY(window.scrollY);
  } useEffect(() => {
    function watchScroll() {
      window.addEventListener("scroll", handleScroll);
    }
    watchScroll();

    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  }, []); return (
    <div>
      <div>{scrollY}px</div>
    </div>
  );
}
```

该代码相当于类示例。

我们在`useEffect`回调中添加监听来监听滚动事件。

然后我们通过移除回调中的监听器来停止监听，该监听器在`useEffect`回调中返回。

处理程序调用`setScrollY`函数来设置`scrollY`状态。

![](img/f5fc5bf80d7557e7fad55d769e893a3d.png)

由 [Herbert Goetsch](https://unsplash.com/@hg_photo?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以在 React 组件中直接嵌入 SVG，只要我们移除所有的命名空间标签。

为了监听滚动事件，我们在组件中附加了一个滚动事件监听器。