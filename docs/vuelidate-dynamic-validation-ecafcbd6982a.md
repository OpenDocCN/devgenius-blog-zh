# Vuelidate —动态验证

> 原文：<https://blog.devgenius.io/vuelidate-dynamic-validation-ecafcbd6982a?source=collection_archive---------1----------------------->

![](img/9760aa31a21451385bef1bae6a667908.png)

照片由[希亚姆](https://unsplash.com/@thezenoeffect?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

默认情况下，Vue.js 没有任何表单验证功能。

因此，我们需要添加自己的表单验证库或自己的代码。

在本文中，我们将了解如何使用 Vuelidate 动态验证表单。

# 动态验证模式

我们可以用 Vuelidate 动态验证表单。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>Name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name.required">Name is required.</div>
    </div><div v-if="hasDescription">
      <label>Description</label>
      <input v-model.trim="$v.description.$model">
      <div v-if="!$v.description.required">Description is required.</div>
    </div><div>
      <label>Has Description</label>
      <input v-model="hasDescription" type="checkbox">
    </div>
  </div>
</template>
<script>
import { required } from "vuelidate/lib/validators";export default {
  name: "App",
  data() {
    return {
      hasDescription: false,
      name: "",
      description: ""
    };
  },
  validations() {
    if (!this.hasDescription) {
      return {
        name: {
          required
        }
      };
    } else {
      return {
        name: {
          required
        },
        description: {
          required
        }
      };
    }
  }
};
</script>
```

我们使用了`validations`方法来返回一个对象，该对象包含要根据`this.hasDescription`属性进行验证的字段。

在模板中，我们需要检查`hasDescription`的值，并且只在`hasDescription`为`true`时渲染`description`字段。

`hasDescription`的值由复选框控制。

# 动态参数

我们还可以动态设置字段的名称。

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>Name</label>
      <input v-model.trim="$v.name.$model">
      <div v-if="!$v.name[valName]">Name is invalid.</div>
    </div>
  </div>
</template>
<script>
import { minLength } from "vuelidate/lib/validators";export default {
  name: "App",
  data() {
    return {
      name: "",
      minLength: 3,
      valName: "validatorName"
    };
  },
  validations() {
    return {
      name: {
        [this.valName]: minLength(this.minLength)
      }
    };
  }
};
</script>
```

我们为`name`字段的验证器取了一个动态名称。

这可以在模板中用来检查名称。

# 内置验证器

Vuelidate 附带了各种类型的验证器。

它们包括:

*   `required` —检查字段是否为必填字段。
*   `requiredIf` —如果谓词为`true`，则需要一个字段
*   `requiredUnless` —如果谓词为`false`，则需要一个字段
*   `minLength` —需要输入的最小长度
*   `maxLength` —所需输入的最大长度
*   `minValue` —最小数值
*   `maxValue` —最大数值
*   `between` —数值范围
*   `alpha` —字母字符
*   `alphaNum` —字母数字字符
*   `numeric` —数字
*   `integer` —整数
*   `decimal` —十进制数字
*   `email` —电子邮件地址
*   `ipAddress` — IP 地址
*   `macAddress` — MAC 地址
*   `sameAs` —检查一个字段是否与另一个字段具有相同的值
*   `url` —网址
*   `or` —当至少一个验证器通过时通过
*   `and` —当所有验证器通过时通过
*   `not` —当提供的验证器未通过时通过
*   `withParams` —用于创建自定义验证器的验证器修改器

例如，我们可以写:

```
<template>
  <div id="app">
    <div>
      <label>field</label>
      <input v-model.trim="$v.field.$model">
      <div v-if="!$v.field.required">field is invalid.</div>
    </div> <div>
      <label>is optional</label>
      <input v-model="isOptional" type="checkbox">
    </div>
  </div>
</template>
<script>
import { requiredUnless } from "vuelidate/lib/validators";export default {
  name: "App",
  data() {
    return {
      field: "foo",
      isOptional: true
    };
  },
  validations: {
    field: {
      required: requiredUnless("isOptional")
    }
  }
};
</script>
```

我们将`isOptional`字段的`required`属性设置为`requiredUnless`验证器。

`requiredUnless(“isOptional”)`表示当`isOptional`为`true`时，该字段为必填项。

# 结论

我们可以用 Vuelidate 动态验证字段。

它还内置了许多验证器。