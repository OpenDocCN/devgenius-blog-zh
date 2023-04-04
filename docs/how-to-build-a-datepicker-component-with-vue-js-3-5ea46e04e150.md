# 如何用 Vue.js 3 构建一个 Datepicker 组件？

> 原文：<https://blog.devgenius.io/how-to-build-a-datepicker-component-with-vue-js-3-5ea46e04e150?source=collection_archive---------2----------------------->

![](img/7373071694110edc1d99ba77bb5f2e29.png)

照片由[杰森·理查德](https://unsplash.com/@jasonthedesigner?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时，我们想在 Vue 3 应用程序中构建自己的日期选择器组件，让用户选择日期。

在本文中，我们将看看如何用 Vue 3 构建一个日期选择器组件。

# 用 Vue.js 3 构建一个 Datepicker 组件

我们可以通过创建一个接受`modelValue`属性并发出`update:modelValue`事件的组件来构建我们自己的输入组件。

这两件事合在一起就和`v-model`指令一样。

因此，我们可以编写以下代码来创建一个日期选择器并使用它。

`App.vue`

```
<template>
  <DatePicker v-model="date" />
  <p>{{ date }}</p>
</template><script>
import DatePicker from "./components/DatePicker.vue";export default {
  name: "App",
  components: {
    DatePicker,
  },
  data() {
    return {
      date: new Date(2020, 1, 1),
    };
  },
};
</script>
```

`components/DatePicker.vue`

```
<template>
  <div id="date-picker">
    <div>
      <label>year</label>
      <br />
      <select v-model="year">
        <option v-for="y in years" :key="y">
          {{ y }}
        </option>
      </select>
    </div>
    <div>
      <label>month</label>
      <br />
      <select v-model="month">
        <option v-for="m in 12" :key="m" :value="m - 1">
          {{ m }}
        </option>
      </select>
    </div>
    <div>
      <label>day</label>
      <br />
      <select v-model="day">
        <option v-for="d in maxDate" :key="d">
          {{ d }}
        </option>
      </select>
    </div>
  </div>
</template><style scoped>
#date-picker {
  display: flex;
}#date-picker div {
  margin-right: 10px;
}
</style><script>
import moment from "moment";export default {
  name: "DatePicker",
  props: {
    modelValue: Date,
  },
  data() {
    return {
      years: [],
      year: new Date().getFullYear(),
      month: 0,
      day: 1,
    };
  },
  methods: {
    emitDate() {
      const { year, month, day } = this;
      this.$emit("update:modelValue", new Date(year, month, day).toString());
    },
  },
  watch: {
    year() {
      this.emitDate();
    },
    month() {
      this.emitDate();
    },
    day() {
      this.emitDate();
    },
  },
  computed: {
    maxDate() {
      const { month } = this;
      if ([0, 2, 4, 6, 7, 9, 11].includes(month)) {
        return 31;
      } else if ([3, 5, 8, 10].includes(month)) {
        return 30;
      }
      return 28;
    },
  },
  beforeMount() {
    const currentYear = new Date().getFullYear();
    for (let i = -100; i <= 100; i++) {
      this.years.push(currentYear + i);
    }
    const d = moment(this.modelValue);
    this.year = +d.format("YYYY");
    this.month = +d.format("MM") - 1;
    this.day = +d.format("DD");
  },
};
</script>
```

在`App.vue`中，我们添加了`DatePicker`组件，并用`v-model`将其绑定到一个日期字符串。

我们用`Date`构造函数创建日期字符串。

当我们从`modelValue`道具中得到它时，它会自动转换成一个字符串。

下面，我们展示了`date`值。

然后在`DatePicker.vue`中，我们添加 3 个选择元素，让我们选择年、月和日。

我们用`v-model`将`select`元素值绑定到反应属性。

`month`的`value`以 0 开始，以 11 结束，以匹配`Date`构造器接受的内容。

我们使用`v-for`来呈现选项值的数字。

接下来，我们向`style`标签添加一些样式，使下拉列表并排显示，并在它们之间添加边距。

然后我们有了组件对象。

该组件采用字符串形式的`modelValue`属性。

`data`返回我们用于模板的反应属性。

`years`是我们稍后将填充的 year 下拉列表的年份数组。

`year`、`month`和`day`设置为默认值。

接下来，我们用`emitDate`方法发出带有日期字符串的`update:modelValue`事件。

我们在`year`、`month`和`day`观察器中调用它，这样当这些值发生变化时它就会运行。

然后我们添加`maxDate` computed 属性来返回我们可以根据选择的月份设置的最大天数。

因为 JavaScript `Date`构造函数使用从 0 开始的月份，我们检查从 0 到 11 的月份值，其中 0 是一月，11 是十二月。

最后，在`beforeMount`钩子中，我们用当前年份前后的 100 年来填充`years`数组。

我们得到`modelValue`属性的值并解析年、月和日的值

我们用 moment 对象自带的`format`方法进行解析。

现在，当我们更改下拉列表中的选项时，我们应该看到选项反映在下面的文本中。

# 结论

我们可以用 Vue 3 轻松创建一个下拉组件。