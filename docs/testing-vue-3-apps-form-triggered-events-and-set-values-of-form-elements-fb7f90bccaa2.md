# 测试 Vue 3 应用程序—表单触发的事件和设置表单元素的值

> 原文：<https://blog.devgenius.io/testing-vue-3-apps-form-triggered-events-and-set-values-of-form-elements-fb7f90bccaa2?source=collection_archive---------3----------------------->

![](img/6d29289f6e99a7adf2a37f3dd6b9d4d6.png)

照片由 [Battlecreek 咖啡烘焙师](https://unsplash.com/@battlecreekcoffeeroasters?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

随着应用程序变得越来越复杂，自动测试它们变得非常重要。我们可以通过单元测试做到这一点，然后我们就不必手动测试所有的东西了。

在本文中，我们将通过编写一个简单的应用程序并测试它来看看如何测试 Vue 3 应用程序。

# 测试表单触发的事件

我们可以测试提交表单时触发的事件。

例如，我们可以写:

```
import { mount } from '@vue/test-utils'const EmailInput = {
  template: `
    <div>
      <input type="email" v-model="email">
      <button @click="submit">Submit</button>
    </div>
  `,
  data() {
    return {
      email: ''
    }
  },
  methods: {
    submit() {
      this.$emit('submit', this.email)
    }
  }
}test('sets the value', async () => {
  const wrapper = mount(EmailInput)
  const input = wrapper.find('input')
  await input.setValue('abc@mail.com')
  expect(input.element.value).toBe('abc@mail.com')
})test('submit event is triggered', async () => {
  const wrapper = mount(EmailInput)
  await wrapper.find('button').trigger('click')
  expect(wrapper.emitted()).toHaveProperty('submit')
})
```

在第二个测试中，我们得到按钮并触发`click`事件。

然后我们检查是否用`wrapper.emitted`方法发出了`submit`事件。

我们可以用发出事件的参数来检查事件触发了什么。

例如，我们可以写:

```
import { mount } from '@vue/test-utils'const EmailInput = {
  template: `
    <div>
      <input type="email" v-model="email">
      <button @click="submit">Submit</button>
    </div>
  `,
  data() {
    return {
      email: ''
    }
  },
  methods: {
    submit() {
      this.$emit('submit', this.email)
    }
  }
}test('emits the input to its parent', async () => {
  const wrapper = mount(EmailInput)
  await wrapper.find('input').setValue('abc@mail.com')
  await wrapper.find('button').trigger('click')
  expect(wrapper.emitted('submit')[0][0]).toBe('abc@mail.com')
})
```

我们登上`EmailInput`。

然后我们在输入上调用`setValue`来设置它的值。

然后我们触发按钮上的`click`事件。

我们检查是否用输入值发出了`submit`事件。

# 使用各种表单元素

我们可以在测试中操作其他表单元素。

例如，我们可以写:

```
import { mount } from '@vue/test-utils'const FormComponent = {
  template: `
    <div>
      <form @submit.prevent="submit">
        <input type="email" v-model="form.email">
        <textarea v-model="form.description"/> <select v-model="form.city">
          <option value="new-york">New York</option>
          <option value="toronto">Toronto</option>
        </select> <input type="checkbox" v-model="form.subscribe"/>
        <input type="radio" value="weekly" v-model="form.interval"/>
        <input type="radio" value="monthly" v-model="form.interval"/> <button type="submit">Submit</button>
      </form>
    </div>
  `,
  data() {
    return {
      form: {
        email: '',
        description: '',
        city: '',
        subscribe: false,
        interval: ''
      }
    }
  },
  methods: {
    async submit() {
      this.$emit('submit', this.form)
    }
  }
}test('submits a form', async () => {
  const wrapper = mount(FormComponent)
  await wrapper.find('input[type=email]').setValue('name@mail.com')
  await wrapper.find('textarea').setValue('Lorem ipsum')
  await wrapper.find('select').setValue('toronto')
  await wrapper.find('input[type=checkbox]').setValue()
  await wrapper.find('input[type=radio][value=monthly]').setValue()
})
```

我们可以通过`textarea`、`selecrt`、复选框和单选按钮来使用`setValue`方法。

此外，我们可以触发复杂的事件。

例如，我们可以写:

```
import { mount } from '@vue/test-utils'const FormComponent = {
  template: `
    <div>
      <form @submit.prevent="submit">
        <input type="email" v-model="form.email">
        <textarea v-model="form.description"/> <select v-model="form.city">
          <option value="new-york">New York</option>
          <option value="toronto">Toronto</option>
        </select> <input type="checkbox" v-model="form.subscribe"/>
        <input type="radio" value="weekly" v-model="form.interval"/>
        <input type="radio" value="monthly" v-model="form.interval"/> <button type="submit">Submit</button>
      </form>
    </div>
  `,
  data() {
    return {
      form: {
        email: '',
        description: '',
        city: '',
        subscribe: false,
        interval: ''
      }
    }
  },
  methods: {
    async submit() {
      this.$emit('submit', this.form)
    }
  }
}test('submits a form', async () => {
  const wrapper = mount(FormComponent)
  const email = 'name@mail.com'
  const description = 'Lorem ipsum'
  const city = 'toronto'
  await wrapper.find('input[type=email]').setValue(email)
  await wrapper.find('textarea').setValue(description)
  await wrapper.find('select').setValue(city)
  await wrapper.find('input[type=checkbox]').setValue()
  await wrapper.find('input[type=radio][value=monthly]').setValue()
  await wrapper.find('form').trigger('submit.prevent')
  expect(wrapper.emitted('submit')[0][0]).toMatchObject({
    email,
    description,
    city,
    subscribe: true,
    interval: 'monthly',
  })
})
```

我们有相同的成分。

在测试中，我们挂载组件并调用`setValue`来设置每个字段的值。

然后我们检查用`submit`事件发出的对象是否与用`toMatchObject`方法预期的对象匹配。

我们测试了`submit`方法中的`this.form`对象是否与最后一行中的匹配。

# 结论

我们可以测试表单触发的事件，并用 Vue 测试工具在我们的 Vue 3 组件测试中设置各种表单元素的值。