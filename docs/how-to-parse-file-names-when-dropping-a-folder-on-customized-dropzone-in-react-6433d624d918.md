# 在 React 中将文件夹拖放到自定义的 dropzone 上时如何解析文件名

> 原文：<https://blog.devgenius.io/how-to-parse-file-names-when-dropping-a-folder-on-customized-dropzone-in-react-6433d624d918?source=collection_archive---------16----------------------->

![](img/358efc32d820e3e990c43c5a66b6aa36.png)

卡米尔·皮特扎克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

好吧，我们不是在谈论从天上掉下来。我们正在谈论…

Dropzone！

在开始阅读本文之前，我假设正在阅读本文的人对 react 和 javascript 有一定的了解。

如果您正在处理拖放文件和文件夹，很可能您已经使用了 react-dropzone。但是如果你想定制你的 dropzone 呢？

在我定制的 dropzone 版本中，它由三个组件组成:

*   DropTarget.js
*   DropList.js
*   dropEffects.js

这些文件是什么？

DropTarget 是一个文件，它实际上完成了计算文件夹中有哪些文件等所有处理工作。下面是 DropTarget.js 的实现:

```
import React from "react";
import PropTypes from "prop-types";
import * as dropEffects from "./dropEffects";const insideStyle = {
 backgroundColor: "#cccccc",
 opacity: 0.5,};const DropTarget = (props) => {
  const [isOver, setIsOver] = React.useState(false);

  const dragOver = (ev) => {
    ev.preventDefault(); ev.dataTransfer.dropEffect = props.dropEffect;
};const handleFiles = (file) => {
  console.log(file);
};const drop = (ev) => {
  const droppedItem = ev.dataTransfer.getData("drag-item");
  var entry = ev.dataTransfer.items[0].webkitGetAsEntry();

  if (entry.isFile) {
    entry.file(handleFiles);
  } else if (entry.isDirectory) {
    var reader = entry.createReader();
    reader.readEntries(function (entries) {
      entries.forEach(function (dir, key) {
        dir.file(handleFiles);
      });
    });
  }

  setIsOver(false);
};const dragEnter = (ev) => {
  ev.dataTransfer.dropEffect = props.dropEffect;
  setIsOver(true);
};const dragLeave = () => setIsOver(false); return (
    <div
      onDragOver={dragOver}
      onDrop={drop}
      onDragEnter={dragEnter}
      onDragLeave={dragLeave}
      style={{ width: "100%", height: "100%", ...(isOver ? insideStyle : {}) }}
    >
      Drop here!
    </div>
 );
}; DropTarget.propTypes = {
  onItemDropped: PropTypes.func.isRequired, 
  dropEffect: PropTypes.string,
  children: PropTypes.oneOfType([
    PropTypes.arrayOf(PropTypes.node),
    PropTypes.node,
  ]).isRequired,
};DropTarget.defaultProps = {
  dropEffect: dropEffects.All,
};export default DropTarget;
```

有大量的工作要做，所以让我们只关注一些细节。DropTarget 返回一个 div，它处理所有类型的拖放功能(onDrop、onDropOver 等)。在这种情况下，我们唯一真正关心的是 onDrop。

当用户将一个装满文件的文件夹放到 div 上时，它被识别为一个事件。在这个事件中(在这个例子中称为 ev ),在 drop 函数中我们得到了被拖动的项。假设我们只处理一个文件夹，我们引用“ev.dataTransfer.items[0]”。webkitGetAsEntry()”，它获取第一个被拖动的项目。

然后，我们检查 entry.isFile()和 entry.isDirectory()是否匹配。如果它是一个目录，那么我们创建一个读取器来解析该目录中的其余条目。

下面是完成这项工作所需的另外两个文件:

//DropList.js

```
import React from "react";
import DropTarget from "./DropTarget";export default () => {
  const [items, setItems] = React.useState([]);
  const itemDropped = (item) => setItems([...items, item]);
  console.log("drop list");
  console.log("items: ", items); return (
    <DropTarget onItemDropped={itemDropped} dropEffect="link">
     <div className="drag-drop-container">
       {items.map((item) => {
         return (
           <div key={item} className="item">
             {item}
           </div>
         );
       })}
     </div>
    </DropTarget>
  );
 };
```

DropTarget.js 引用的另一个文件:

//dropEffects.js

```
export const All = "all";export const Move = "move";export const Copy = "copy";export const Link = "link";export const CopyOrMove = "copyMove";export const CopyOrLink = "copyLink";export const LinkOrMove = "linkMove";export const None = "none";
```

一旦在文件夹中包含了所有这三个文件，就可以像这样简单地引用 droplist:

```
import React, { Component } from "react";
import DropList from "./components/DropList";
import logo from "./logo.svg";
import "./App.css";class App extends Component { render() {
    return (
     <div className="App">
      <DropList />
     </div>
   );
  }
}export default App;
```

现在你知道了！