# 强制完成—自动完成

> 原文：<https://blog.devgenius.io/vuetify-autocomplete-de19719dff0d?source=collection_archive---------0----------------------->

![](img/0a7ef3bfbe68b51b898dd4b7ebf10886.png)

坎贝尔·布朗热在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 自动完成

我们使用`v-autocomplete`组件向我们的应用程序添加自动完成输入。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card color="red lighten-2" dark>
          <v-card-text>
            <v-autocomplete
              v-model="model"
              :items="items"
              :loading="isLoading"
              :search-input.sync="search"
              color="white"
              hide-no-data
              hide-selected
              item-text="Description"
              item-value="API"
              label="Public APIs"
              placeholder="Start typing to Search"
              prepend-icon="mdi-database-search"
              return-object
            ></v-autocomplete>
          </v-card-text>
          <v-divider></v-divider>
          <v-expand-transition>
            <v-list v-if="model" class="red lighten-3">
              <v-list-item v-for="(field, i) in fields" :key="i">
                <v-list-item-content>
                  <v-list-item-title v-text="field.value"></v-list-item-title>
                  <v-list-item-subtitle v-text="field.key"></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-expand-transition>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn :disabled="!model" color="grey darken-3" @click="model = null">
              Clear
              <v-icon right>mdi-close-circle</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data: () => ({
    descriptionLimit: 60,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
  }), computed: {
    fields() {
      if (!this.model) return []; return Object.keys(this.model).map((key) => {
        return {
          key,
          value: this.model[key],
        };
      });
    },
    items() {
      return this.entries.map((entry) => {
        const Description = entry.Description.slice(0,  this.descriptionLimit); return Object.assign({}, entry, { Description });
      });
    },
  }, watch: {
    search(val) {
      if (this.items.length > 0) return;
      if (this.isLoading) return;
      this.isLoading = true; fetch("https://api.publicapis.org/entries")
        .then((res) => res.json())
        .then((res) => {
          const { count, entries } = res;
          this.count = count;
          this.entries = entries;
        })
        .catch((err) => {
          console.log(err);
        })
        .finally(() => (this.isLoading = false));
    },
  },
};
</script>
```

我们添加了`v-autocomplete`组件。

`v-model`有典范。

`loading`道具有装载状态。

`color`设置文本颜色。

`hide-no-data`表示我们不隐藏任何数据。

`hide-selected`表示我们隐藏选中的项目。

`item-text`是描述的属性。

`item-value`是物品的属性。

`label`是输入标签。

`placeholder`有输入占位符。

`prepend-icon`是添加到项目的图标名称。

`return-object`将结果作为对象返回。

`v-list`有显示项目和显示结果。

`search`观察器在`search`值改变时获取数据。

`search`我们打字时的变化。

`fields` computed 属性是我们作为搜索结果呈现的内容。

`items`已经选中的物品。

![](img/27ae57cd237ef1551c7df27be887c063.png)

Samuele Errico Piccarini 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用`v-autocomplete`组件添加一个自动完成输入。