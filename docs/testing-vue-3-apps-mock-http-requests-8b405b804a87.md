# 测试 Vue 3 应用——模拟 HTTP 请求

> 原文：<https://blog.devgenius.io/testing-vue-3-apps-mock-http-requests-8b405b804a87?source=collection_archive---------2----------------------->

![](img/f92f672d456200d8535b54e7a7b696b2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Mometrix 测试准备](https://unsplash.com/@mometrixtestprep?utm_source=medium&utm_medium=referral)的照片

随着应用程序变得越来越复杂，自动测试它们变得非常重要。我们可以通过单元测试做到这一点，然后我们就不必手动测试所有的东西了。

在本文中，我们将通过编写一个简单的应用程序并测试它来看看如何测试 Vue 3 应用程序。

# 解决其他异步行为

如果我们在组件外部的组件中有异步行为，那么我们就模仿它们。

例如，我们可以写:

`Foo.vue`

```
<template>
  <div>
    <p>{{ answer }}</p>
  </div>
</template><script>
import axios from "axios";export default {
  data() {
    return {
      answer: "",
    };
  },
  beforeMount() {
    this.load();
  },
  methods: {
    async load() {
      const {
        data: { answer },
      } = await axios.get("https://yesno.wtf/api");
      this.answer = answer;
    },
  },
};
</script>
```

`example.spec.js`

```
import { mount } from '[@vue/test-utils](http://twitter.com/vue/test-utils)'
import flushPromises from 'flush-promises'
import axios from 'axios'
import Foo from './Foo'jest.mock('axios', () => ({
  get: jest.fn(() => Promise.resolve({ data: { answer: 'yes' } }))
}))test('uses a mocked axios HTTP client and flush-promises', async () => {
  const wrapper = mount(Foo)
  expect(axios.get).toHaveBeenCalledWith('https://yesno.wtf/api')
  await flushPromises()
  const div = wrapper.find('div')
  expect(div.text()).toContain('yes')
});
```

我们有`Foo`组件，它从 API 获取一些数据并呈现出来。

然后在`example.spec.js`中，我们用`jest.fn`方法模拟`axios.get`方法。

它让我们窥探函数是否被调用，以及用什么调用它。

此外，我们从函数中返回数据。

然后在测试中，我们安装组件。

然后我们调用`expect`来检查`axios.get`是否和`‘https://yesno.wtf/api'`一起被调用，就像我们在`Foo`中做的那样。

然后我们调用`flushPromises`等待 DOM 更新。

最后，我们使用`text`方法获取 div 并检查文本。

# 断言加载状态

我们还可以通过测试来检查装载状态。

例如，我们可以写:

`Foo.vue`

```
<template>
  <div>
    <p v-if="loading" role="alert">Loading...</p>
    <p v-else>{{ answer }}</p>
  </div>
</template><script>
import axios from "axios";export default {
  data() {
    return {
      loading: false,
      answer: "",
    };
  },
  beforeMount() {
    this.load();
  },
  methods: {
    async load() {
      this.loading = true;
      const {
        data: { answer },
      } = await axios.get("https://yesno.wtf/api");
      this.answer = answer;
      this.loading = false;
    },
  },
};
</script>
```

`example.spec.js`

```
import { mount } from '@vue/test-utils'
import flushPromises from 'flush-promises'
import axios from 'axios'
import Foo from './Foo'jest.mock('axios', () => ({
  get: jest.fn(() => Promise.resolve({ data: { answer: 'yes' } }))
}))test('uses a mocked axios HTTP client and flush-promises', async () => {
  const wrapper = mount(Foo)
  expect(wrapper.find('[role="alert"]').exists()).toBe(true)
  expect(wrapper.find('[role="alert"]').text()).toBe('Loading...')
  expect(axios.get).toHaveBeenCalledWith('https://yesno.wtf/api')
  await flushPromises()
  const div = wrapper.find('div')
  expect(div.text()).toContain('yes')
  expect(wrapper.find('[role="alert"]').exists()).toBe(false)
});
```

我们有包含加载消息的`Foo`组件。

它由`loading`反应属性控制。

然后在测试中，我们检查是否呈现了将`role`设置为`alert`的元素。

然后我们调用`flushPromises`并检查将`role`设置为`alert`的元素是否不再被渲染。

我们还检查是否得到了与上一个例子一样的响应。

# 结论

我们检查模拟 HTTP 请求，用 Vue 3 组件和 Vue 测试工具测试我们的组件。