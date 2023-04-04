# 顶级反应库—动画、尺寸和降价

> 原文：<https://blog.devgenius.io/top-react-libraries-animation-sizes-and-markdown-4846a476fbfd?source=collection_archive---------6----------------------->

![](img/5a5d55a50114dededd67b2336218d731.png)

照片由[Verena b ttcher](https://unsplash.com/@verenaxxs?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反作用运动

React-Motion 软件包可以让我们以自己的方式制作动画内容。

要安装它，我们可以运行:

```
npm i react-motion
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Motion, spring } from "react-motion";export default function App() {
  return (
    <div className="App">
      <Motion defaultStyle={{ x: 0 }} style={{ x: spring(100) }}>
        {value => <div>{value.x}</div>}
      </Motion>
    </div>
  );
}
```

它附带了`Motion`组件来设置默认样式。

通过渲染`value.x`来显示`x`属性。

然后我们使用`spring`函数来动画显示从 0 到 100 的数字。

它还支持其他类型的运动。

例如，我们可以使用`StaggerMotion`组件来制作两个盒子拉伸的动画:

```
import React from "react";
import { StaggeredMotion, spring } from "react-motion";export default function App() {
  return (
    <div className="App">
      <StaggeredMotion
        defaultStyles={[{ h: 0 }, { h: 0 }]}
        styles={prevInterpolatedStyles =>
          prevInterpolatedStyles.map((_, i) => {
            return i === 0
              ? { h: spring(100) }
              : { h: spring(prevInterpolatedStyles[i - 1].h) };
          })
        }
      >
        {interpolatingStyles => (
          <div>
            {interpolatingStyles.map((style, i) => (
              <div key={i} style={{ border: "1px solid", height: style.h }} />
            ))}
          </div>
        )}
      </StaggeredMotion>
    </div>
  );
}
```

我们使用`StaggeredMotion`组件，而`defaultStyles`具有高度的初始样式。

然后在`styles`道具中，我们有代码用`spring`函数制作高度变化的动画。

我们对高度`prevInterpolatedStyles`进行动画处理，将高度更改为中间高度。

然后在标签之间，我们用给定的高度制作盒子的动画。

# react-sizeme

react-sizeme 让我们获得元素的大小。

要安装它，我们可以运行:

```
npm i react-sizeme
```

然后我们可以通过写来使用它:

```
import React from "react";
import { SizeMe } from "react-sizeme";export default function App() {
  return (
    <div className="App">
      <SizeMe>{({ size }) => <div>width: {size.width}px</div>}</SizeMe>
    </div>
  );
}
```

`SizeMe`组件具有 div 的大小。

我们用`size.width`得到值。

我们也可以使用`withSize`高阶分量来获得尺寸:

```
import React from "react";
import { withSize } from "react-sizeme";function App({ size }) {
  return <div>My width is {size.width}px</div>;
}export default withSize()(App);
```

# 反应-降价

react-markdown 允许我们在 react 应用程序中呈现 markdown 代码。

要安装它，我们运行:

```
npm i react-markdown
```

然后我们可以通过写来使用它:

```
import React from "react";
const ReactMarkdown = require("react-markdown");const input = `# This is a headerAnd this is a paragraphThis is a [link]([http://example.com)`](http://example.com)`);export default function App() {
  return (
    <div className="App">
      <ReactMarkdown source={input} />
    </div>
  );
}
```

我们把降价代码放在一个字符串中。

然后我们将它作为`source`属性的值传递给`ReactMarkdown`组件来显示它。

它还可以解析 HTML:

```
import React from "react";
const ReactMarkdown = require("react-markdown");const input = `# This is a headerAnd this is a paragraph<code>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis lacinia consectetur nunc. Morbi suscipit volutpat urna, in feugiat lorem convallis eget. Ut vel diam imperdiet, ultricies ligula et, tempor nibh. Donec congue turpis ex, in rhoncus sem feugiat eu. Donec condimentum volutpat ligula ac venenatis. Fusce lectus ligula, porttitor eu mollis ac, dictum vitae ligula. Pellentesque suscipit facilisis risus, finibus facilisis velit varius a. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Sed consequat justo in euismod volutpat. Cras blandit, nibh nec ornare tincidunt, libero metus tempor risus, non pulvinar nisl justo vitae arcu. Fusce sed mi et augue convallis volutpat et vitae turpis. Vestibulum efficitur ac est id ultrices. Quisque condimentum augue lectus, vulputate auctor ex ultrices sed. Vivamus facilisis est ac urna vestibulum eleifend.</code>`;export default function App() {
  return (
    <div className="App">
      <ReactMarkdown source={input} escapeHtml={false} />
    </div>
  );
}
```

它将呈现`code`标签中的所有文本，并将它们显示为代码。

![](img/6f44eefd99b917f322122b75ebb0c309.png)

瑞安·康塞普西翁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

反应-运动让我们可以让任何东西有生气。

react-sizeme 包获取元素的大小。

react-markdown 将 markdown 呈现为 HTML。