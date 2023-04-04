# 用 CamanJS 和 React 进行图像处理

> 原文：<https://blog.devgenius.io/image-manipulation-with-camanjs-and-react-af813c615387?source=collection_archive---------4----------------------->

![](img/041fbca7a2d4c8bca79239b0386ba84d.png)

由 [Hermes Rivera](https://unsplash.com/@hermez777?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

CamanJS 库允许我们在客户端进行图像处理。

我们可以在 React 这样的框架中使用它。

在本文中，我们将了解如何使用 CamanJS 和 React 来操作图像。

# 入门指南

我们通过使用 Create React App 创建一个 React 项目来使用 CamanJS 和 React。

为此，我们运行:

```
npx create-react-app my-project
```

其中`my-project`是项目名称。

然后在`public/index.html`文件中，我们将 CamanJS 的脚本标签添加到 React 项目的`head`标签中:

```
<script
    src="https://cdnjs.cloudflare.com/ajax/libs/camanjs/4.1.2/caman.full.min.js"
    integrity="sha512-JjFeUD2H//RHt+DjVf1BTuy1X5ZPtMl0svQ3RopX641DWoSilJ89LsFGq4Sw/6BSBfULqUW/CfnVopV5CfvRXA=="
    crossorigin="anonymous"></script>
```

然后我们可以将我们想要操作的图像添加到`src`文件夹中。

一旦我们做到了这一点，我们可以添加以下内容到`App.js`:

```
import React, { useEffect } from "react";
import kittenPic from './kitten.jpg'export default function App() {
  const kitten = React.useRef(null); useEffect(() => {
    window.Caman(`#${kitten.current.id}`, function () {
      this.brightness(10);
      this.contrast(30);
      this.sepia(60);
      this.saturation(-30);
      this.render();
    });
  }, [kitten.current]); return (
    <div className="App">
      <img
        src={kittenPic}
        alt=""
        style={{ width: 200, height: 200 }}
      />
      <img
        id="kitten"
        ref={kitten}
        src={kittenPic}
        alt=""
        style={{ width: 200, height: 200 }}
      />
    </div>
  );
}
```

我们导入图像并将其添加为`img`标签的`src`。

CamanJS 只处理存储在本地项目中的图像。

然后在`useEffect`钩子中，我们进行图像操作。

是我们正在操作的图像对象。

我们通过 ID 获取图像。ID 是`Caman`函数的第一个参数。

`this.brightness`让我们改变亮度。

`this.contrast`让我们改变对比度。

`this.sepia`让我们更改棕褐色调。

`this.saturation`让我们改变饱和度。

`this.render`渲染新图像。

现在我们应该看到左边是原图，右边是 CamanJS 编辑的图。

# 层

CamanJS 带有一个分层系统，给了我们更多的编辑能力。

例如，我们可以写:

```
import React, { useEffect } from "react";
import kittenPic from './kitten.jpg'export default function App() {
  const kitten = React.useRef(null); useEffect(() => {
    window.Caman(`#${kitten.current.id}`, function () {
      this.exposure(-10);
      this.newLayer(function () {
        this.setBlendingMode("multiply");
        this.opacity(80);
        this.fillColor('#6899ba');
        this.copyParent();
        this.filter.brightness(10);
        this.filter.contrast(20);
      });
      this.clip(10);
      this.render();
    });
  }, [kitten.current]); return (
    <div className="App">
      <img
        src={kittenPic}
        alt=""
        style={{ width: 200, height: 200 }}
      />
      <img
        id="kitten"
        ref={kitten}
        src={kittenPic}
        alt=""
        style={{ width: 200, height: 200 }}
      />
    </div>
  );
}
```

我们在图层之前用`exposure`方法改变曝光。

然后我们用`newLayer`方法创建图层。

在回调中，我们调用`setBlendingMode`来设置混合模式。

`opacity`改变不透明度。

`fillColor`改变层的填充颜色。

`copyParent`将父层的内容复制到当前层。

一旦我们调用了`copyParent`，我们就可以调用`filter`方法来改变图层，使它看起来像我们想要的。

# 结论

我们可以使用 CamanJS 和 React 来操作 React 应用程序中的图像。