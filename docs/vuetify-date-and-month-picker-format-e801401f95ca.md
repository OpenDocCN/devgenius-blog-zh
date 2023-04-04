# Vuetify —月份选择器格式

> 原文：<https://blog.devgenius.io/vuetify-date-and-month-picker-format-e801401f95ca?source=collection_archive---------4----------------------->

![](img/b1147ab05541c13f3a251a852341f39f.png)

马丁·桑切斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 月选取器只读

我们可以用`readonly`属性将月份选择器设置为只读:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" type="month" readonly></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

# 月份选择器当前月份指示器

当前月份指示器可以用`show-current`按钮改变:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" type="month" :show-current="false"></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

设置为`false`的`show-current`道具使选择的月份变亮。

我们也可以将其设置为月份字符串:

```
<template>
  <v-row justify="center">
    <v-date-picker v-model="date" type="month" show-current="2020-08"></v-date-picker>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 7),
  }),
};
</script>
```

选定的月份周围会有边框。

# 对话框和菜单中的日期选择器

我们可以在对话框中放一个日期选择器。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-col cols="12" sm="6" md="4">
      <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        :return-value.sync="date"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="date"
            label="Picker in menu"
            prepend-icon="event"
            readonly
            v-bind="attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="date" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
        </v-date-picker>
      </v-menu>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    menu: false,
  }),
};
</script>
```

我们把我们的`v-date-picker`放在一个`v-menu`中，这样当我们点击菜单图标时，我们可以显示日期选择器。

我们将`v-text-field`放在`activator`槽中，这样我们就可以使用`on`对象来监听日期选择器事件。

我们使用`attrs`对象为`v-text-field`设置道具。

这样，当我们从日期选择器中选择一个日期时，它的值将显示在文本字段中。

`prepend-icon`显示图标。

# 日期选择器-格式化日期

我们可以从日期选择器中格式化日期。

例如，我们可以写:

```
<template>
  <v-row justify="center">
    <v-col cols="12" sm="6" md="4">
      <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        :return-value.sync="date"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="computedDateFormatted"
            label="Picker in menu"
            prepend-icon="event"
            readonly
            v-bind="attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="date" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
        </v-date-picker>
      </v-menu>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "HelloWorld",
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    menu: false,
  }),
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date);
    },
  },
  methods: {
    formatDate(date) {
      if (!date) return null; const [year, month, day] = date.split("-");
      return `${month}/${day}/${year}`;
    },
  },
};
</script>
```

我们将`v-text-field`的`v-model`设置为`computedDateFormatted`计算属性。

它用`formatDate`方法格式化日期字符串，该方法分割原始日期字符串，并用斜杠组合各部分。

然后我们应该看到带文本字段的格式化日期。

# 结论

我们可以将月份选择器设置为只读，并将日期格式化为我们想要的格式。