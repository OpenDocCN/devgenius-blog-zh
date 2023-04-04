# 顶级反应库—选项卡、通知、数字输入和文件上传

> 原文：<https://blog.devgenius.io/top-react-libraries-tabs-notifications-numeric-inputs-and-file-upload-cbcca925b95c?source=collection_archive---------6----------------------->

![](img/3a7928f944679bab01f0eac451491d56.png)

照片由[拉维·辛格](https://unsplash.com/@ravishekharsingh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# 反应-标签

react-tabs 包允许我们向 react 应用程序添加选项卡。

要安装它，我们运行:

```
npm i react-tabs
```

然后我们可以通过写来使用它:

```
import React from "react";
import { Tab, Tabs, TabList, TabPanel } from "react-tabs";
import "react-tabs/style/react-tabs.css";export default function App() {
  return (
    <div>
      <Tabs>
        <TabList>
          <Tab>tab 1</Tab>
          <Tab>tab 2</Tab>
        </TabList> <TabPanel>
          <h2>some content</h2>
        </TabPanel>
        <TabPanel>
          <h2>more content</h2>
        </TabPanel>
      </Tabs>
    </div>
  );
}
```

我们导入包中的 CSS 和组件。

然后我们使用带有`TabList`的`Tabs`组件来添加选项卡链接。

`TabList`里面有标签页链接。

`Tab`是我们可以点击切换标签的按钮。

`TabPanel`有每个标签的内容。

现在，我们无需做太多工作就能获得包含内容的标签。

# RC-上传

rc-tree 是一个包，它为我们提供了一个文件输入组件。

要使用它，我们通过运行以下命令来安装它:

```
npm i rc-upload
```

然后我们可以通过写来使用它:

```
import React from "react";
import Upload from "rc-upload";const uploaderProps = {
  action: "/upload",
  data: { foo: 1, bar: 2 },
  headers: {
    Authorization: "token"
  },
  multiple: true,
  beforeUpload(file) {
    console.log("beforeUpload", file.name);
  },
  onStart: file => {
    console.log("onStart", file.name);
  },
  onSuccess(file) {
    console.log("onSuccess", file);
  },
  onProgress(step, file) {
    console.log("onProgress", Math.round(step.percent), file.name);
  },
  onError(err) {
    console.log("onError", err);
  }
};export default function App() {
  return (
    <div>
      <div
        style={{
          margin: 100
        }}
      >
        <div>
          <Upload {...uploaderProps}>upload file</Upload>
        </div> <div
          style={{
            height: 200,
            overflow: "auto",
            border: "1px solid red"
          }}
        >
          <div
            style={{
              height: 500
            }}
          >
            <Upload
              {...this.uploaderProps}
              id="test"
              component="div"
              style={{ display: "inline-block" }}
            >
              another uploader
            </Upload>
          </div> 
          <label>Label for Upload</label>
        </div>
      </div>
    </div>
  );
}
```

我们有`Uploader`组件，它有文件上传输入。

`uploaderProps`拥有一个包含上传 URL、标题、数据等内容的对象。

`data`已经请求数据。

`action`有网址。

我们还传入了在各种情况下运行的各种事件处理程序。

上传前、上传开始时、成功、错误和进度都需要处理程序。

# rc 输入号码

rc-input-number 为我们提供了数字输入。

要安装它，我们运行:

```
npm i rc-input-number
```

然后我们使用库附带的`InputNumber`组件:

```
import React from "react";
import InputNumber from "rc-input-number";export default function App() {
  return (
    <div>
      <InputNumber defaultValue={100} />
    </div>
  );
}
```

`defaultValue`具有输入值。

我们可以使用`value`和`onChange`道具使其成为受控输入:

```
import React from "react";
import InputNumber from "rc-input-number";export default function App() {
  const [value, setValue] = React.useState(100);
  const onChange = value => {
    setValue(value);
  }; return (
    <div>
      <InputNumber value={value} onChange={onChange} />
    </div>
  );
}
```

我们有一个函数来设置`value`状态，获取`value`并将其传递给`value` prop，就像任何其他输入一样。

此外，我们可以使它只读或禁用它。

数字的格式也可以改变。

# RC-通知

rc-notification 为我们提供了在 React 应用程序中添加通知的 UI 组件。

要安装它，我们运行:

```
npm i rc-notification
```

然后我们可以通过写来使用它:

```
import React, { useEffect } from "react";
import Notification from "rc-notification";export default function App() {
  useEffect(() => {
    Notification.newInstance({}, notification => {
      notification.notice({
        content: "content"
      });
    });
  }, []); return <div />;
}
```

我们导入`Notification`对象并调用`newInstance`方法在回调中显示通知。

`notice`方法显示通知内容。

`content`有通知内容。

默认情况下，它会显示 1.5 秒。

我们可以改变它显示的持续时间、样式等等。

我们还可以通过编程方式删除通知。

![](img/8d9b388340540df192311492f526dd34.png)

由[阿丽娜·车尔尼雪娃](https://unsplash.com/@achera?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

让我们添加标签。

rc-upload 是一个文件上传组件。

rc-input-number 是 React 应用程序的数字输入。

rc-notification 是一个简单的通知组件。