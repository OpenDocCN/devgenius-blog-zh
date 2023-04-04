# Vuetify —自定义自动完成

> 原文：<https://blog.devgenius.io/vuetify-customize-autocomplete-c298033784d2?source=collection_archive---------3----------------------->

![](img/382bc42eda0b4287e27b3fad0e025bd0.png)

瑞安·斯潘塞在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Vuetify 是一个流行的 Vue 应用程序 UI 框架。

在本文中，我们将了解如何使用 Vuetify 框架。

# 自动完成时的自定义筛选

我们可以用`v-autocomplete`组件添加一个自定义过滤器。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="overflow-hidden" color="purple lighten-1" dark>
          <v-card-text>
            <v-text-field color="white" label="Name"></v-text-field>
            <v-autocomplete
              :items="fruits"
              :filter="customFilter"
              color="white"
              item-text="name"
              label="Fruit"
            ></v-autocomplete>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="success" @click="save">Save</v-btn>
          </v-card-actions>
          <v-snackbar
            v-model="hasSaved"
            :timeout="2000"
            absolute
            bottom
            left
          >Your profile has been updated</v-snackbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data() {
    return {
      hasSaved: false,
      model: null,
      fruits: [
        { name: "apple", id: 1 },
        { name: "grape", id: 2 },
        { name: "orange", id: 3 },
      ],
    };
  }, methods: {
    customFilter(item, queryText, itemText) {
      const text = item.name.toLowerCase();
      const searchText = queryText.toLowerCase();
      return text.indexOf(searchText) > -1;
    },
    save() {
      this.isEditing = !this.isEditing;
      this.hasSaved = true;
    },
  },
};
</script>
```

我们添加了带有`items`道具的`v-autocomplete`组件。

`items`属性包含自动完成下拉菜单的项目。

`filter`道具有我们想要用来过滤的过滤方法。

# 密集自动完成

我们可以添加`dense`道具来降低自动完成的高度和最大高度。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card class="overflow-hidden" color="purple lighten-1" dark>
          <v-card-text>
            <v-autocomplete
              dense
              :items="fruits"
              :filter="customFilter"
              color="white"
              item-text="name"
              label="Fruit"
            ></v-autocomplete>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="success" @click="save">Save</v-btn>
          </v-card-actions>
          <v-snackbar
            v-model="hasSaved"
            :timeout="2000"
            absolute
            bottom
            left
          >Your profile has been updated</v-snackbar>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data() {
    return {
      hasSaved: false,
      model: null,
      fruits: [
        { name: "apple", id: 1 },
        { name: "grape", id: 2 },
        { name: "orange", id: 3 },
      ],
    };
  }, methods: {
    customFilter(item, queryText, itemText) {
      const text = item.name.toLowerCase();
      const searchText = queryText.toLowerCase();
      return text.indexOf(searchText) > -1;
    },
    save() {
      this.isEditing = !this.isEditing;
      this.hasSaved = true;
    },
  },
};
</script>
```

我们只是在`v-autocomplete`上加了`dense`道具，让它变短。

# 时间

我们可以用插槽改变 select 的输出。

例如，我们可以写:

```
<template>
  <v-container>
    <v-row>
      <v-col col="12">
        <v-card color="blue-grey darken-1" dark :loading="isUpdating">
          <v-form>
            <v-container>
              <v-row>
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="name"
                    :disabled="isUpdating"
                    filled
                    color="blue-grey lighten-2"
                    label="Name"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-autocomplete
                    v-model="friends"
                    :disabled="isUpdating"
                    :items="people"
                    filled
                    chips
                    color="blue-grey lighten-2"
                    label="Select"
                    item-text="name"
                    item-value="name"
                    multiple
                  >
                    <template v-slot:selection="data">
                      <v-chip
                        v-bind="data.attrs"
                        :input-value="data.selected"
                        close
                        @click="data.select"
                        @click:close="remove(data.item)"
                      >
                        <v-avatar left>
                          <v-img :src="data.item.avatar"></v-img>
                        </v-avatar>
                        {{ data.item.name }}
                      </v-chip>
                    </template>
                    <template v-slot:item="data">
                      <template v-if="typeof data.item !== 'object'">
                        <v-list-item-content v-text="data.item"></v-list-item-content>
                      </template>
                      <template v-else>
                        <v-list-item-avatar>
                          <img :src="data.item.avatar" />
                        </v-list-item-avatar>
                        <v-list-item-content>
                          <v-list-item-title v-html="data.item.name"></v-list-item-title>
                          <v-list-item-subtitle v-html="data.item.group"></v-list-item-subtitle>
                        </v-list-item-content>
                      </template>
                    </template>
                  </v-autocomplete>
                </v-col>
              </v-row>
            </v-container>
          </v-form>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn
              :disabled="autoUpdate"
              :loading="isUpdating"
              color="blue-grey darken-3"
              depressed
              [@click](http://twitter.com/click)="isUpdating = true"
            >
              <v-icon left>mdi-update</v-icon>Update Now
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template><script>
export default {
  name: "HelloWorld",
  data() {
    const src = "https://cdn.vuetifyjs.com/images/lists/1.jpg";
    return {
      autoUpdate: true,
      friends: ["Sandra Adams", "Britta Holt"],
      isUpdating: false,
      name: "",
      people: [
        { name: "Sandra Adams", avatar: src },
        { name: "Mary Smith", avatar: src },
        { name: "James Wong", avatar: src },
      ],
    };
  }, watch: {
    isUpdating(val) {
      if (val) {
        setTimeout(() => (this.isUpdating = false), 3000);
      }
    },
  }, methods: {
    remove(item) {
      const index = this.friends.indexOf(item.name);
      if (index >= 0) this.friends.splice(index, 1);
    },
  },
};
</script>
```

我们有`selection`槽来填充人名条目的姓名文本和头像。

`v-chip`组件是这两个项目的容器。

此代码:

```
<template v-slot:selection="data">
  <v-chip
  v-bind="data.attrs"
  :input-value="data.selected"
  close
  @click="data.select"
  @click:close="remove(data.item)"
  >
  <v-avatar left>
      <v-img :src="data.item.avatar"></v-img>
  </v-avatar>
  {{ data.item.name }}
  </v-chip>
</template>
```

当我们选择一个项目时会显示芯片。

![](img/bea5801c3b63f1ee43aa5a9daede6024.png)

照片由 [Alina Rubo](https://unsplash.com/@lubitglazami?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以使用 Vuetify 为自动完成添加自定义过滤和显示自定义内容。