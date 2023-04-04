# 反应提示—复制到剪贴板，用钩子比较新旧值

> 原文：<https://blog.devgenius.io/react-tips-copy-to-clipboard-comparing-old-and-new-values-with-hooks-a5f22a258a09?source=collection_archive---------4----------------------->

![](img/c5961092d20bc6371a5419f854619d0b.png)

Ramiz deda kovi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 如何将文本复制到剪贴板

我们可以通过使用`navigator.ckipboard.writeText`方法将文本复制到剪贴板。

例如，我们可以写:

```
<button 
  onClick={() =>  navigator.clipboard.writeText('copy this to clipboard')}
>
  copy to clipboard
</button>
```

我们将参数中字符串的文本复制到剪贴板。

此外，我们可以使用 react-copy-to-clipboard 包来简化我们的工作。

例如，我们可以写:

```
import React from 'react';
import ReactDOM from 'react-dom';
import {CopyToClipboard} from 'react-copy-to-clipboard';

class App extends React.Component {
  state = {
    value: '',
    copied: false,
  }; onChange({target: {value}}) {
    this.setState({value, copied: false});
  },

  render() {
    return (
      <div>
        <input value={this.state.value} onChange={this.onChange} />

        <CopyToClipboard text={this.state.value}
          onCopy={() => this.setState({copied: true})}>
          <span>Copy to clipboard with span</span>
        </CopyToClipboard>

      </div>
    );
  }
}
```

该包带有`CopyToClipboard`组件。

它使用了带有我们想要复制到剪贴板的文本的`text`道具。

复制文本时运行`onCopy`道具。

在组件内部，我们有可以点击进行复制的内容。

一旦元素被点击，`text`道具中的内容将被复制到剪贴板。

我们还可以使用`execCommand`方法将选中的 DOM 元素的内容复制到剪贴板。

例如，我们可以写:

```
import React, { useRef, useState } from 'react';export default function CopyExample() {const [copySuccess, setCopySuccess] = useState('');
  const textAreaRef = useRef(null); function copyToClipboard(e) {
    textAreaRef.current.select();
    document.execCommand('copy');
  }; return (
    <div>
      {
       document.queryCommandSupported('copy') &&
        <div>
          <button onClick={copyToClipboard}>Copy</button> 
          {copySuccess}
        </div>
      }
      <form>
        <textarea
          ref={textAreaRef}
          value='text to copy'
        />
      </form>
    </div>
  );
}
```

我们有一个带有`copyToClipboard`的功能组件，用于从我们的文本区域选择文本。

选择通过以下方式完成:

```
textAreaRef.current.select();
```

`textAreaRef`是我们分配给文本区域的引用。

然后我们调用带有`'copy'`参数的`execCommand`，将选中的文本复制到剪贴板。

在我们返回的 JSX 中，我们检查复制命令是否支持:

```
document.queryCommandSupported('copy')
```

并显示一个按钮让我们复制数据。

我们还有文本区，里面有要复制的内容。

# 用一个 onChange 处理程序识别不同的输入

我们可以对多个输入使用一个事件处理程序。

为此，我们可以创建一个事件处理函数，它接受一个参数来标识我们已经更改的输入。

例如，我们可以写:

```
class App extends React.Component {
  constructor() {
    super();
    this.state = { input1: 0, input2: 0 };
    this.handleChange = this.handleChange.bind(this);
  } handleChange(input, value) {
    this.setState({
      [input]: value
    })
  } render() {
    return (
      <div>
        <input type="text" onChange={e => this.handleChange('input1', e.target.value)} />
        <input type="text" onChange={e => this.handleChange('input2', e.target.value)} />
      </div>
    )
  }
}
```

我们传入一个回调函数，该函数用输入文本时想要改变的状态的键调用`handleChange`方法。

这样，我们可以改变我们想要的输入。

`handleChange`中的`setState`有一个计算属性名，而不是固定属性。

# 用 useEffect 钩子比较旧值和新值

我们可以使用`useRef`钩子来获取之前的值。

我们可以从组件本身获得最新的值。

例如，我们可以写:

```
const usePrevious = (value) => {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}const App = (props) => {
  const { amount, balance } = props
  const prevAmount = usePrevious({ amount, balance });
  useEffect(() => {
    if (prevAmount.amount !== amount) {
      //...
    }

    if (prevAmount.balance !== balance) {
      //...
    }
  }, [amount, balance])    //...
}
```

我们创建了一个`usePrevious`钩子来用`useRef`获取之前的值。

通过将值设置为`ref.current`属性，我们将传递给钩子函数的旧值设置为。

然后从钩子返回先前的值。

在`App`组件中，我们从道具中获取最新的值。

我们从`usePrevious`钩子中得到旧的值。

然后我们可以在`useEffect`回调的时候对比一下。

我们传入的数组中有我们想要监视其变化的值。

![](img/178a256dcbcb17f4a9733f375668bbd8.png)

照片由 [Waranya Mooldee](https://unsplash.com/@anyadiary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`useRef`钩子设置以前的值。

有多种方法可以将文本从组件复制到剪贴板。

我们可以通过创建自己的事件处理程序来识别组件中的不同输入。