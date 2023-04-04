# 在 React 应用程序的各个部分之间切换

> 原文：<https://blog.devgenius.io/features-big-e-commerce-companies-i-worked-for-use-for-scaling-the-business-easily-f7ccce5600fa?source=collection_archive---------2----------------------->

## 一些特性切换有益的用例

![](img/ba470382c81cdd3905d38377884d7d43.png)

伊戈尔·米斯克在 [Unsplash](https://unsplash.com/s/photos/ecommerce-website?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 功能切换非常强大

也被称为*特征标志*，这是我工作过的每一个堆栈的重要部分，对于许多用例来说，它非常方便和有用。它们基本上是设置为在两个或更多体验、行为甚至小 UI 组件之间有条件切换的 *cookies* 。让我们列出几种类型的功能切换及其潜在的用例:

1- **服务器端特性切换:**主要用于服务器端渲染代码，比如将 API 端点指向不同数据源的集成。我们可以依赖来自客户端的 cookie，或者根据期望的行为在 http 请求中启动 cookie。

看看下面的[*NextJs getServerSideProps*](https://nextjs.org/docs/basic-features/data-fetching#getserversideprops-server-side-rendering)*例子，这段代码在服务器上执行，它正在访问来自客户端的 cookies，*除非它被代理拦截)*利用 [*一个 npm 包*](https://www.npmjs.com/package/cookies) :*

```
*import Cookies from "cookies";
....export async function getServerSideProps(ctx) {const {req, res, ...rest} = ctx;
 const cookies = new Cookies(req, res);
 const shouldLoadDraftContent = cookies.get('draftContentEnabled'); if(shouldLoadDraftContent) {
   // fetch draft content }else {
   // fetch published content 
 } return { props: { ... some props } };}*
```

*在上面的示例中，我们在决定加载草稿内容之前检查了 cookie*draftContentEnabled*值，这对于在发布之前测试内容的营销团队来说非常方便，尤其是当我们没有足够的环境来指向不同的端点时。*

*2- **客户端功能切换:**通常用于不同行为或体验之间的切换。假设我们有一个新页面，您想只向特定类型的用户或特定地区的用户显示，假设您有一个支持所有地区/用户的源代码。以下示例将向您展示如何在路由器级别的 React 应用程序中实现这一点。这个例子假设您有一个 [cookie 管理访问器(getters 和 setter)](https://www.w3schools.com/js/js_cookies.asp)。*

```
*import {getCookie} from './path/to/your/cookie/manager';
const userRegion = getCookies('userRegion'); return(<Router>
 <Route> ...route for a page</Route>
 <Route> ...route for a page</Route>
 <Route> ...route for a page</Route>
 {userRegion === 'us" && <Route></Route>}
</Router>);*
```

*现在，设置“用户区域”可以基于 url 或某个服务器位置检测器来进行，该检测器向客户端提供该值。这也可以在一个 [A/B 测试脚本](https://vwo.com/ab-testing/)中设置，因此它是 100%自动地服务于您的客户端需求。*

*3- [**应用网关**](https://www.techopedia.com/definition/4189/application-gateway) **特性切换**:这是我最喜欢的部分，因为它真正地分离了关注点——我将在下一节中对此进行更多的讨论。解释这一点的一个好方法是使用一个例子。让我们假设您有一个 [Shopify 商店](https://www.shopify.com/)，其中也包含一些静态页面。你决定将你的静态页面从 shopify 转移到另一个服务提供商或你的 S3 木桶中。*

*如何从不同的来源提供不同的页面？有很多方法可以实现这一点，但是快速使用像[和](https://www.fastly.com/)这样的应用网关是其中之一。*

*现在的问题是，您如何构建这些页面并增量发布新构建的页面？！*

*也就是说，在发布的第一天，你想要显示的新页面(来自你的 AWS)只占你流量的 *10%* ！！！其余的人可以看到遗留页面(shopify 页面)。你逐渐增加这个百分比。这样你就不会冒险让你的新服务器过载，甚至在新代码测试好之前就阻止你的大部分流量使用新代码！。*

*您可以在您的应用程序网关中添加一个功能开关，它可以执行以下操作:*

**这里的语法是一个旧的 [Terraform](https://learn.hashicorp.com/terraform) 语法，主要用于基础设施应用。*

```
*# check if a previously decision was made - for returning customers to see consistent behaviour -if (!req.http.Cookie:enableNewStaticPages) {
  # Select the backend based on variable%
  if (randombool(10,100)) {
   # add logic to point to AWS 
   set req.Cookie.enableNewStaticPages=true
  } else {
   # add logic to point to shopify
   set req.Cookie.enableNewStaticPages=false
  }
} else {
 # keep the same behaviour
 if(req.http.Cookie:enableNewStaticPages === 'true) {
  # add logic to point to AWS
 }else {
  # add logic to point to shopify
 }
}*
```

# ***为什么这是我最喜欢的功能切换类型？***

*基本上是因为它更干净*在大多数情况下*你可以将这些静态页面的代码放在一个单独的存储库中，并让你的服务器指向这些代码，而不是 Shopify。这使得你的代码更整洁，因为在前端不需要特性切换来控制行为。因为代码不会被执行，除非“enableNewStaticPages===true”。*

# *在下一篇文章中:*

*![](img/2fbee1c660c386ea2ccbf7a319f0c7c3.png)*

*由[法伊祖尔·拉赫曼](https://unsplash.com/@fazurrehman?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/ui?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄*

**购物车/结账流程管理*:我将讨论这是一个非常重要的部分，该领域使用了哪些工具和功能来轻松扩展购物车集成，以及在构建购物车或将购物车集成到您的商店时需要考虑的一些事情。*

*不要忘记关注[我的账户](https://medium.com/@abdalla.ahmed.ksa/about)并添加您的电子邮件，以便在新帖子发布时得到通知。*