# React 的故事书—全局、插件和故事

> 原文：<https://blog.devgenius.io/storybook-for-react-globals-and-addons-and-stories-36bb00af116f?source=collection_archive---------4----------------------->

![](img/97a57bbad6dc4a3d37ad7aefd8b63be8.png)

[Alekon pictures](https://unsplash.com/@alekonpictures?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Storybook 让我们可以用各种参数轻松地构建组件原型。

在这篇文章中，我们将看看如何使用 Storybook 来处理 globals。

# 在故事中使用全局变量

我们可以在一个故事里消费全局。

为此，我们从 story 函数中获取`globals`属性。

例如，我们可以写:

`.storybook/preview.js`

```
export const globalTypes = {
  locale: {
    name: 'Locale',
    description: 'locale',
    defaultValue: 'en',
    toolbar: {
      icon: 'globe',
      items: [
        { value: 'en', right: '🇺🇸', title: 'English' },
        { value: 'fr', right: '🇫🇷', title: 'Français' },
        { value: 'zh', right: '🇨🇳', title: '中文' },
      ],
    },
  },
};
```

`src/stories/Button.stories.js`

```
import React from 'react';import { Button } from './Button';export default {
  title: 'Example/Button',
  component: Button,
  argTypes: {
    backgroundColor: { control: 'color' },
  },
};const getCaptionForLocale = (locale) => {
  switch (locale) {
    case 'en': return 'Hello';
    case 'fr': return 'Bonjour';
    case 'zh': return '你好';
    default:
      return 'Hello'
  }
}const Template = (args, { globals: { locale } }) => <Button {...{ ...args, label: getCaptionForLocale(locale) }} />;
```

我们用`.storybook/preview.js`文件定义了`locale`全局变量。

然后在`src/stories/Button.stories.js`文件中，我们用参数的`globals.locale`属性获得了`locale`全局属性。

`right`属性是当我们将它连接到装饰器时，显示在工具栏菜单右侧的文本。

# 在插件中使用全局变量

我们可以在一个附加文件中使用全局变量。

例如，我们可以写:

```
import React from 'react';
import { useGlobals } from '@storybook/api';
import { addons, types } from '@storybook/addons';
import { AddonPanel, Spaced, Title } from '@storybook/components';const LocalePanel = props => {
  const [{ locale }] = useGlobals(); return (
    <>
      <style>
        {`
        #panel-tab-content > div > div[hidden] {
          display: flex !important
        }
      `}
      </style>
      <AddonPanel {...props}>
        <Spaced row={3} outer={1}>
          <Title>{locale}</Title>
        </Spaced>
      </AddonPanel>
    </>
  );
};const ADDON_ID = 'locale-panel';
const PANEL_ID = `${ADDON_ID}/panel`;addons.register(ADDON_ID, (api) => {
  addons.add(PANEL_ID, {
    type: types.PANEL,
    title: 'Locale',
    render: ({ active, key }) => {
      return <AddonPanel active={active} key={key}>
        <LocalePanel />
      </AddonPanel>
    },
  });
});
```

添加一个插件，用`useGlobal`钩子获得全局属性。

我们使用`AddonPanel`、`Spaced`和`Title`属性来显示 addon 面板中的内容。

方法注册了附加组件，这样它就会显示在故事书的附加组件面板中。

此外，我们有`AddonPanel`来渲染面板。

当我们显示带有`style`标签的面板时，我们让它显示出来。

# 故事渲染

我们可以改变故事的呈现方式。

为了添加到`head`标签，我们将 HTML 代码添加到`.storybook/preview-head.html`文件中:

```
<link rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css"
    integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh"
    crossorigin="anonymous">
```

然后它们会显示在 iframe 内部的 head 标签中。

# 结论

我们可以用故事书添加我们自己的插件面板。

此外，我们可以在 iframe 的 head 标签中添加标签。