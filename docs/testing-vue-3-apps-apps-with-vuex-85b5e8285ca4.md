# 测试 Vue 3 应用——使用 Vuex 的应用

> 原文：<https://blog.devgenius.io/testing-vue-3-apps-apps-with-vuex-85b5e8285ca4?source=collection_archive---------4----------------------->

![](img/2ef2ccc1761f1cb15f1da46ae224510c.png)

照片由 [Ani Kolleshi](https://unsplash.com/@anikolleshi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

随着应用程序变得越来越复杂，自动测试它们变得非常重要。我们可以通过单元测试做到这一点，然后我们就不必手动测试所有的东西了。

在本文中，我们将通过编写一个简单的应用程序并测试它来看看如何测试 Vue 3 应用程序。

# 测试 Vuex

我们可以通过 Vuex 商店测试应用程序。

例如，如果我们有以下应用程序:

```
import { mount } from '@vue/test-utils'
import { createStore } from 'vuex'const store = createStore({
  state() {
    return {
      count: 0
    }
  },
  mutations: {
    increment(state) {
      state.count += 1
    }
  }
})const App = {
  template: `
    <div>
      <button @click="increment" />
      Count: {{ count }}
    </div>
  `,
  computed: {
    count() {
      return this.$store.state.count
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment')
    }
  }
}const app = createApp(App)
app.use(store)
```

我们可以用真正的 Vuex 商店来测试。

例如，我们可以通过编写以下内容来添加测试:

```
import { mount } from '@vue/test-utils'
import { createApp } from 'vue'
import { createStore } from 'vuex'const store = createStore({
  state() {
    return {
      count: 0
    }
  },
  mutations: {
    increment(state) {
      state.count += 1
    }
  }
})const App = {
  template: `
    <div>
      <button @click="increment" />
      Count: {{ count }}
    </div>
  `,
  computed: {
    count() {
      return this.$store.state.count
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment')
    }
  }
}const app = createApp(App)
app.use(store)test('vuex', async () => {
  const wrapper = mount(App, {
    global: {
      plugins: [store]
    }
  })
  await wrapper.find('button').trigger('click')
  expect(wrapper.html()).toContain('Count: 1')
})
```

我们将我们的`store`传递到为`plugins`属性设置的数组中。

然后当我们点击`App`中的按钮时，真实商店的状态被更新。

# 使用模拟商店进行测试

我们也可以用模拟商店来测试应用程序。

例如，我们可以写:

```
import { mount } from '@vue/test-utils'
import { createApp } from 'vue'
import { createStore } from 'vuex'const store = createStore({
  state() {
    return {
      count: 0
    }
  },
  mutations: {
    increment(state) {
      state.count += 1
    }
  }
})const App = {
  template: `
    <div>
      <button [@click](http://twitter.com/click)="increment" />
      Count: {{ count }}
    </div>
  `,
  computed: {
    count() {
      return this.$store.state.count
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment')
    }
  }
}const app = createApp(App)
app.use(store)test('vuex using a mock store', async () => {
  const $store = {
    state: {
      count: 25
    },
    commit: jest.fn()
  }
  const wrapper = mount(App, {
    global: {
      mocks: {
        $store
      }
    }
  })
  expect(wrapper.html()).toContain('Count: 25')
  await wrapper.find('button').trigger('click')
  expect($store.commit).toHaveBeenCalled()
})
```

我们用作为模拟函数的`commit`方法创建我们的`$store`对象。

然后我们得到渲染状态。

我们点击按钮，检查是否调用了`$store.commit`。

这很方便，因为我们在挂载的组件中没有任何外部依赖。

然而，如果我们打破了 Vuex 商店，那么我们没有任何警告。

# 结论

我们可以用 Vue 3 的 Vue 测试工具测试连接到 Vuex 商店的组件。