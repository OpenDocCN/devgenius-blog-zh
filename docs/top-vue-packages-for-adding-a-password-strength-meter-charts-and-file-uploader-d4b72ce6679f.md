# 用于添加密码强度计、图表和文件上传器的顶级 Vue 包

> 原文：<https://blog.devgenius.io/top-vue-packages-for-adding-a-password-strength-meter-charts-and-file-uploader-d4b72ce6679f?source=collection_archive---------16----------------------->

![](img/0e2d3d5b01274dd02e30a7610494be51.png)

乔希·牛顿在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Vue.js 是一个易于使用的 web 应用框架，我们可以用它来开发交互式前端应用。

在本文中，我们将看看添加密码强度表、图表和文件上传器的最佳包。

# 密码强度计

vue-password-strength-meter 是一个为我们的 vue 应用程序添加密码强度的简单工具。

为了使用它，我们运行:

```
npm i vue-password-strength-meter zxcvbn
```

来安装它。

`zxcvbn`是测量密码强度所必需的。

然后我们可以通过写来使用它:

```
<template>
  <password v-model="password"/>
</template>

<script>
import Password from "vue-password-strength-meter";
export default {
  components: { Password },
  data() {
    return {
      password: null
    };
  }
};
</script>
```

我们使用`password`组件来添加密码输入。

我们用`v-model`绑定到`password`状态。

它可以用一些选项来定制。

我们可以监听在 score 和 feedback 事件发出时发出的事件。

例如，我们可以写:

```
<template>
  <password v-model="password" @score="showScore" @feedback="showFeedback"/>
</template>

<script>
import Password from "vue-password-strength-meter";
export default {
  components: { Password },
  data() {
    return {
      password: null
    };
  },
  methods: {
    showFeedback({ suggestions, warning }) {
      console.log(suggestions);
      console.log(warning);
    },
    showScore(score) {
      console.log(score);
    }
  }
};
</script>
```

它发出带有密码强度分数的`score`事件。

发出`feedback`事件，并反馈我们的密码强度。

如果我们愿意，我们可以得到这些东西并显示给用户。

我们可以为奇怪的仪表、成功和错误类、标签等定制样式。

# vue-简单上传程序

vue-simple-uploader 是一个用于 vue 应用程序的文件上传组件。

为了使用它，我们运行:

```
npm i vue-simple-uploader
```

来安装它。

然后我们写道:

`main.js`

```
import Vue from "vue";
import App from "./App.vue";
import uploader from "vue-simple-uploader";Vue.use(uploader);
Vue.config.productionTip = false;new Vue({
  render: h => h(App)
}).$mount("#app");
```

`App.vue`

```
<template>
  <uploader :options="options" class="uploader-example">
    <uploader-unsupport></uploader-unsupport>
    <uploader-drop>
      <p>Drop files here to upload or</p>
      <uploader-btn>select files</uploader-btn>
      <uploader-btn :attrs="attrs">select images</uploader-btn>
      <uploader-btn :directory="true">select folder</uploader-btn>
    </uploader-drop>
    <uploader-list></uploader-list>
  </uploader>
</template>

<script>
export default {
  data() {
    return {
      options: {
        target: "//localhost:3000/upload",
        testChunks: false
      },
      attrs: {
        accept: "image/*"
      }
    };
  }
};
</script>
```

向我们的应用程序添加上传组件。

当浏览器不支持所需功能时，显示`uploader-unsupport`。

`upload-btn`是上传按钮。

`uploader-drop`是用于文件上传的 dropzone。

`uploader-list`显示上传的文件列表。

`options`有选项，包括上传网址。

`attres`有我们接受的物品。

我们可以填充一些插槽来定制我们的应用程序。

# vue-高图表

我们可以使用 vue-highcharts 包通过我们的应用程序轻松添加图表。

一个特殊的功能是我们可以为点添加自定义标记。

为了使用它，我们运行:

```
npm i vue2-highcharts highcharts
```

安装它及其依赖项。

然后我们可以通过写来使用它:

```
<template>
  <div>
    <vue-highcharts :options="options" ref="lineCharts"></vue-highcharts>
    <button @click="load">load</button>
  </div>
</template>

<script>
import VueHighcharts from "vue2-highcharts";
const data = {
  name: "New York",
  marker: {
    symbol: "square"
  },
  data: [
    7.0,
    6.9,
    {
      y: 26.5,
      marker: {
        symbol: "url(http://www.highcharts.com/demo/gfx/sun.png)"
      }
    }
  ]
};
export default {
  components: {
    VueHighcharts
  },
  data() {
    return {
      options: {
        chart: {
          type: "spline"
        },
        title: {
          text: "Monthly Average Temperature"
        },
        subtitle: {
          text: "temperature chart"
        },
        xAxis: {
          categories: ["Jan", "Feb", "Mar"]
        },
        yAxis: {
          title: {
            text: "Temperature"
          },
          labels: {
            formatter() {
              return this.value + "°";
            }
          }
        },
        tooltip: {
          crosshairs: true,
          shared: true
        },
        credits: {
          enabled: false
        },
        plotOptions: {
          spline: {
            marker: {
              radius: 4,
              lineColor: "#666666",
              lineWidth: 1
            }
          }
        },
        series: []
      }
    };
  },
  methods: {
    load() {
      const lineCharts = this.$refs.lineCharts;
      lineCharts.delegateMethod("showLoading", "Loading...");
      lineCharts.addSeries(data);
      lineCharts.hideLoading();
    }
  }
};
</script>
```

我们添加`vue-highcharts`组件来添加图表。

`options`拥有所有的图表选项。

`chart`有图表类型、

`title`有标题。

`subtitle`有副标题。

`xAxis`有 x 轴标签。

`yAxis`有 y 轴标签。

`labels`有标签。

`formatter`具有格式化标签的功能。

`tooltios`启用工具提示。`crosshairs`启用线上的标签。

`shared`启用其他标签。

`plotOptions`有记号笔。

让我们用图表做各种事情。

在我们的例子中，我们使用`'showLoading'`来显示加载文本。

我们获取图表组件的 ref，然后使用`addSeries`方法添加图表数据。

`hideLoading`隐藏加载文本。

![](img/2707de69efc11c28ee226fc799992944.png)

由 [KirstenMarie](https://unsplash.com/@24k?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

vue-password-strength-meter 为我们提供了一个有用的密码强度测量仪。

vue-simple-uploader 为我们提供了一个文件上传组件。

vue-highcharts 是个图表库。