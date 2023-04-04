# 顶级 React 库—日期选择器、富文本编辑器和分页

> 原文：<https://blog.devgenius.io/top-react-libraries-date-picker-rich-text-editor-and-pagination-4a887317e72d?source=collection_archive---------13----------------------->

![](img/c9bbda482f4f23f46f94c41b5309a2f6.png)

照片由[金青铜](https://unsplash.com/@26yashm12?utm_source=medium&utm_medium=referral)放在[的挡泥板上](https://unsplash.com?utm_source=medium&utm_medium=referral)拍摄

为了让开发 React 应用更容易，我们可以添加一些库，让我们的生活更轻松。

在本文中，我们将看看一些流行的 React 应用程序库。

# rc 日历

日历是一个日期选择器库。

要安装它，我们运行:

```
npm i rc-calendar
```

然后我们可以通过写来使用它:

```
import React from "react";
import Calendar from "rc-calendar";
import "rc-calendar/assets/index.css";export default class App extends React.Component {
  state = {
    mode: "date",
    rangeStartMode: "date",
    rangeEndMode: "date"
  }; handlePanelChange = (...args) => {
    console.log("on panel change", ...args);
  }; render() {
    return (
      <div style={{ zIndex: 1000, position: "relative" }}>
        <Calendar
          mode={this.state.mode}
          onPanelChange={this.handlePanelChange}
        />
      </div>
    );
  }
}
```

我们使用`Calendar`组件添加一个日期选择器。

`mode`也可以是`'time'`、`'decade'`、`'month'`或`'year'`，让我们选择月份或年份。

此外，我们可以使用`RangeCalendar`组件来选择日期范围:

```
import React from "react";
import RangeCalendar from "rc-calendar/lib/RangeCalendar";
import "rc-calendar/assets/index.css";export default class App extends React.Component {
  state = {
    mode: "date",
    rangeStartMode: "date",
    rangeEndMode: "date"
  }; handleRangePanelChange = (...args) => {
    console.log("on panel change", ...args);
  }; render() {
    return (
      <div style={{ zIndex: 1000, position: "relative" }}>
        <RangeCalendar
          mode={[this.state.rangeStartMode, this.state.rangeEndMode]}
          onPanelChange={this.handleRangePanelChange}
        />
      </div>
    );
  }
}
```

我们切换到`RangeCalendar`组件来获得日期范围选择器。

`mode`是一个数组，用于选择开始和结束范围的模式。

这些选项与`Calendar`相同。

配置日历还有许多其他选择。

我们可以改变样式，设置地区，以我们的方式呈现日期，等等。

# 反应-分页

我们可以使用 react-paginate 包在应用程序中添加分页链接容器。

要安装它，我们可以运行:

```
npm i react-paginate
```

然后我们可以通过写来使用它:

```
import React from "react";
import ReactPaginate from "react-paginate";export default class App extends React.Component {
  constructor(props) {
    super(props); this.state = {
      data: [],
      offset: 0
    };
  } componentDidMount() {} handlePageClick = data => {
    let selected = data.selected;
    let offset = Math.ceil(selected * this.props.perPage); this.setState({ offset });
  }; render() {
    return (
      <div className="commentBox">
        <ReactPaginate
          previousLabel={"previous"}
          nextLabel={"next"}
          breakLabel={"..."}
          breakClassName={"break"}
          pageCount={this.state.pageCount}
          marginPagesDisplayed={2}
          pageRangeDisplayed={5}
          onPageChange={this.handlePageClick}
          containerClassName={"pagination"}
          subContainerClassName={"pages pagination"}
          activeClassName={"active"}
        />
      </div>
    );
  }
}
```

我们有`ReactPagination`组件来显示带有一些道具的分页栏。

`previousLabel`是上一页链接的文本。

`nextLabel`是下一页链接的文本。

`breakLabel`是文本的分页符，用来跳过页码。

`breakClassName`是课间休息的类名。

`pageCount`是页数的道具。

`marginPagesDisplayed`为页边距显示的页数。

`pageRangeDisplayed`是要显示的页面范围。

`onPageChange`是我们点击页面链接时运行的功能。

`containerClassName`是容器的类名。

`subContainerClassName`是子容器的类名。

`activeClassName`是活动链接的类。

它没有样式，所以我们可以添加我们想要的样式。

# 反应-羽毛笔

React-Quill 是 React 应用程序的一个易于使用的文本编辑器包。

要安装它，我们运行:

```
npm i react-quill
```

然后我们可以通过写来使用它:

```
import React from "react";
import ReactQuill from "react-quill";
import "react-quill/dist/quill.snow.css";export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { text: "" };
    this.handleChange = this.handleChange.bind(this);
  } handleChange(value) {
    this.setState({ text: value });
  } render() {
    return <ReactQuill value={this.state.text} onChange={this.handleChange} />;
  }
}
```

`ReactQuill`组件用于添加富文本编辑器。

它需要一个设置为`text`状态的`value`道具。

`onChange`用`handleChange`方法用输入值更新状态。

我们还需要 CSS 来正确显示文本编辑器。

它支持普通文本编辑器的所有功能，包括粗体、斜体、下划线、列表、链接等等。

工具栏和编辑区域可以自定义。

![](img/f72855512d760df805cfae7913413d6e.png)

照片由[海伦娜·洛佩斯](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

rc-calendar 可用于添加日期或时间选择器。

react-paginate 允许我们在应用程序中添加分页链接。

和 React-Quill 是一个丰富的文本编辑器，可以对各种定制选择做出反应。