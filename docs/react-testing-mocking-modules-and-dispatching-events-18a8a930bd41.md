# 反应测试——模拟模块和分派事件

> 原文：<https://blog.devgenius.io/react-testing-mocking-modules-and-dispatching-events-18a8a930bd41?source=collection_archive---------9----------------------->

![](img/62ac97a573cd4be7fee6c617bc78e1b9.png)

雅各布·达尔比约恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自动化测试对大多数应用程序来说都很重要。

在本文中，我们将看看如何为 React 组件编写测试。

# 模拟模块

我们可以模拟在测试环境中工作不太好的模块。

例如，如果我们有以下组件:

`Map.js`

```
import React from "react";import { LoadScript, GoogleMap } from "react-google-maps";
export default function Map(props) {
  return (
    <LoadScript id="script-loader" googleMapsApiKey="YOUR_API_KEY">
      <GoogleMap id="example-map" center={props.center} />
    </LoadScript>
  );
}
```

`Contact.js`

```
import Map from "./map";export default function Contact({ name, email }) {
  return (
    <div>
      <address>
        {name} {email}
      </address>
      <Map center={props.center} />
    </div>
  );
}
```

然后我们可以用一个被模仿的`Map`组件来测试`Contact`组件:

`Contact.test.js`

```
import React from "react";
import { render, unmountComponentAtNode } from "react-dom";
import { act } from "react-dom/test-utils";import Contact from "./contact";jest.mock("./map", () => {
  return function DummyMap(props) {
    return (
      <div data-testid="map">
        {props.center.lat}:{props.center.long}
      </div>
    );
  };
});let container = null;
beforeEach(() => {
  container = document.createElement("div");
  document.body.appendChild(container);
});afterEach(() => {
  unmountComponentAtNode(container);
  container.remove();
  container = null;
});it("should render contact information", () => {
  const center = { lat: 0, long: 0 };
  act(() => {
    render(
      <Contact
        name="james"
        email="test@example.com"
        center={center}
      />,
      container
    );
  }); expect(
    container.querySelector("address").textContent
  )
    .toContain("james test@example.com"); expect(container.querySelector('[data-testid="map"]').textContent).toEqual(
    "0:0"
  );
});
```

我们有:

```
jest.mock("./map", () => {
  return function DummyMap(props) {
    return (
      <div data-testid="map">
        {props.center.lat}:{props.center.long}
      </div>
    );
  };
});
```

来嘲笑`Map`组件。

然后当我们调用`render`时，我们用`DummyMap`而不是实际的`Map`组件进行渲染。

# 事件

为了测试事件，我们可以在 DOM 元素上调度真实的 DOM 事件。

例如，如果我们想测试`Toggle`组件:

```
import React, { useState } from "react";export default function Toggle(props) {
  const [state, setState] = useState(false);
  return (
    <button
      onClick={() => {
        setState(previousState => !previousState);
        props.onChange(!state);
      }}
      data-testid="toggle"
    >
      {state ? "off" : "on"}
    </button>
  );
}
```

然后我们可以为它添加一个测试文件:

`Toggle.test.js`

```
import React from "react";
import { render, unmountComponentAtNode } from "react-dom";
import { act } from "react-dom/test-utils";import Toggle from "./toggle";let container = null;
beforeEach(() => {
  container = document.createElement("div");
  document.body.appendChild(container);
});afterEach(() => {
  unmountComponentAtNode(container);
  container.remove();
  container = null;
});it("changes value when clicked", () => {
  const onChange = jest.fn();
  act(() => {
    render(<Toggle onChange={onChange} />, container);
  });
  const button = document.querySelector("[data-testid=toggle]");
  expect(button.innerHTML).toBe("on");
  act(() => {
    button.dispatchEvent(new MouseEvent("click", { bubbles: true }));
  });
  expect(onChange).toHaveBeenCalledTimes(1);
  expect(button.innerHTML).toBe("off");
  act(() => {
    button.dispatchEvent(new MouseEvent("click", { bubbles: true }));
  });
  expect(onChange).toHaveBeenCalledTimes(2);
  expect(button.innerHTML).toBe("on");
});
```

我们模仿作为`onChange`属性的值传入的`onChange`方法。

然后我们从`Toggle`组件中得到带有选择器`[data-testid=toggle]`的按钮。

然后我们可以得到按钮的内容，以及在我们调用`dispatchEvent`调度一次点击`MouseEvent`之后`onChange`被调用了多少次。

我们需要传入`{ bubbles: true }`以便 React 将事件委托给文档。

# 结论

我们可以模拟在测试中不能方便使用的模块。

此外，我们可以在元素上触发事件，然后检查结果。