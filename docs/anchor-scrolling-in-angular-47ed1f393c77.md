# 锚滚动角度

> 原文：<https://blog.devgenius.io/anchor-scrolling-in-angular-47ed1f393c77?source=collection_archive---------2----------------------->

## 为什么它不起作用？

![](img/785e4e133abea25d3e81bd7497dc797f.png)

如果当 URL 中包含一个片段时，您曾经遇到过将正文滚动到正确位置的问题，那么请不要离开。

# 水疗中心的主播卡嗒声。

片段是 URL 的`hash`属性，通常指向页面上的一个锚点。锚点击在网络上工作需要以下基本知识:

`<a href="#anchor">Click me</a>`

在另一个地方

`<element id="anchor"></element>`

为了在页面加载时工作，不需要任何其他东西。在 Angular 中，值得一提的是，链接应该是具有`fragment`属性的路由器链接，就像这样:

`<a routerLink="." fragment="anchor">Click me</a>`

根据[角度文档](https://angular.io/api/router/InMemoryScrollingOptions#anchorScrolling)，我们需要将根路由器的`anchorScrolling`属性设置为`enabled`来进行滚动。但这并不准确。

那么这个属性到底是做什么的呢？当用`anchor`捕捉到滚动事件时，调用[函数](https://github.com/angular/angular/blob/b653ee340e3361b484837b028bdd7c4f91a46f85/packages/common/src/viewport_scroller.ts#L119) `scrollToAnchor`。让我们深入研究一下这个函数。该函数进行一些检查，找到元素，然后[得到坐标](https://github.com/angular/angular/blob/b653ee340e3361b484837b028bdd7c4f91a46f85/packages/common/src/viewport_scroller.ts#L156)，如下所示:

> 看，Medium 终于采用了代码着色！

```
// Angular internal function common library
scrollToElement(el) {
  const rect = el.getBoundingClientRect();
  const left = rect.left + this.window.pageXOffset;
  const top = rect.top + this.window.pageYOffset;
  const offset = this.offset();
  this.window.scrollTo(left - offset[0], top - offset[1]);
}
```

**那么，为什么——即使它找到了元素——返回值都是零呢？**

如果元素在主组件之外(因此比任何子组件加载得都快),它可能会被找到，但是它会出现在最上面。所以零是正确的读数。然而，如果它在子组件中，返回的滚动对象可能是`null`，如果它还没有被加载的话。

我们必须等待客户端加载。但是需要多久呢？

让我分享一下`RouterModule` [角度代码](https://github.com/angular/angular/blob/main/packages/router/src/router_scroller.ts#L67)中的滚动事件“消费者”函数，以供参考，因为我们将使用我们自己的订阅者覆盖它:

```
// Angular internal code in router module library
private consumeScrollEvents() {
  return this.router.events.subscribe(e => {
    if (!(e instanceof Scroll)) return;
    // a popstate event. The pop state event will always ignore anchor scrollin
    if (e.position) {
      if (this.options.scrollPositionRestoration === 'top') {
        this.viewportScroller.scrollToPosition([0, 0]);
      } else if (this.options.scrollPositionRestoration === 'enabled') {
        this.viewportScroller.scrollToPosition(e.position);
      }
      // imperative navigation "forward"
    } else {
      if (e.anchor && this.options.anchorScrolling === 'enabled') {
      /*********** here is where things happen ************/
        this.viewportScroller.scrollToAnchor(e.anchor);
      } else if (this.options.scrollPositionRestoration !== 'disabled') {
        this.viewportScroller.scrollToPosition([0, 0]);
      }
    }
  });
}
```

# 计时器

解决这个问题的第一个也是最明显的方法是在滚动到元素之前使用一个`timeout`。如果我们在根路由模块中设置`anchorScrolling`为`enabled`，那么`Scroll`事件会在`NavigationEnd`之后立即触发。然而，在**版本 15 中，**事件被超时触发([见发布修复](https://github.com/angular/angular/commit/79e9e8ab779d230f6a1df25c4ccff94b13129305)):

```
// Angular internal code
private scheduleScrollEvent(routerEvent: NavigationEnd, anchor: string|null): void {
  this.zone.runOutsideAngular(() => {
    setTimeout(() => {
      this.zone.run(() => {
        this.router.triggerEvent(new Scroll(
            routerEvent, this.lastSource === 'popstate' ? this.store[this.restoredId] : null,
            anchor));
      });
    }, 0);
  });
}
```

**但是事情是这样的**，定时器几乎立刻触发事件(0 毫秒)。这仍然不是我们想要的，因为有时内容在加载之前需要一点时间(想想 headless CSM，预渲染并不能解决这个问题，因为 Angular 在 JavaScript 准备好的时候就开始了)。因此，让我们编写自己的事件订阅者，至少有 1 秒钟的延迟。

> 最终代码在[堆栈上](https://stackblitz.com/edit/angular-anchor-scrolling)

在我们的**根路由模块**中，我们将设置一个事件监听器，超时后调用`scrollToAnchor`函数。我们还应该禁用任何`anchorScrolling`,这样它就不会运行内置代码。

```
// root routing module
@NgModule({
  imports: [
    RouterModule.forRoot(routes, {
      // you may still do this
      scrollPositionRestoration: 'top',
      // but don't do this, the default is disabled
      // anchorScrolling: 'enabled'
    })
  ],
  exports: [RouterModule]
})
export class AppRoutingModule {
  constructor(
    router: Router,
    viewportScroller: ViewportScroller,
  ) {
    // listen to events
    router.events.pipe(
      // catch the Scroll event
      filter(event => event instanceof Scroll)
    ).subscribe({
      next: (e: Scroll) => {
         if (e.anchor) {
           // timeout for 1 second before scrolling
           // or timer(1000) RxJS
           zone.runOutsideAngular(() => {
             setTimeout(() => {
               zone.run(() => {
                 viewportScroller.scrollToAnchor(e.anchor);
               });
             }, 1000);
           });
         }
      }
    });
  }
}
```

我们可以用有棱角的男人用的区外法，让自己看起来很酷。😎

您可以微调计时器以适合您的项目，并且记住您需要与页面加载保持足够的距离，但要足够快，以免混淆用户。

这并不理想。我们能做得更好吗？

# 滚动特定事件

如果我们需要的只是一个带有标题的简单页面，那么等待 1 秒钟应该没问题。但是，如果我们有一个复杂的设置**无头 CSM 与我们想要链接的副标题**，那么我们希望有点创意。以下是我的建议:

**触发滚动事件，不影响历史状态**。我仍然想使用 Angular 库的内部机制，我不希望显式地计算位置，也不希望在没有必要时超时。有两个地方我们可以这样做:

*   每次 HTTP 调用结束后
*   选择性地在返回的 HTML 绑定完成后

因为只有通过触发导航事件才能触发滚动事件，所以我们的解决方案是使用`navigate`方法。

让我们假设一个 HTTP 调用，它在几毫秒内结束:

```
// example call to an http function
this.post$ = this.postService.GetPost('someparams').pipe(
  // on finish, try to navigate
  finalize(() => {
    // router = Router
    // url has the hash with it
    // skipLocationChange so that it does not build up in history
    // for this to work, the root router module needs to be set to
    // onSameUrlNavigation: 'reload'
    this.router.navigateByUrl(this.router.url, {skipLocationChange: true});
  })
);
```

这也加上了`anchorScrolling`为`enabled`，`onSameUrlNavigation`为`reload`。我们也可以考虑将`scrollOffset`设置为远离边缘的值，比如`200px`:

```
// root routing module, for navigate solution to work
RouterModule.forRoot(routes, {
   onSameUrlNavigation: 'reload',
   anchorScrolling: 'enabled',
   scrollOffset: [0, 200],
})
```

如果我们有一个服务和一个 HTTP **拦截器**，这可以设计得更好。但是我不会用我的设置来迷惑你，只要记住，在一个好的应用程序中，在完成一个 HTTP 拦截器时，通常会发生另一个事件:**隐藏页面加载器**。你可能想把这条线和它放在一起。

别担心，`navigatgeByUrl`不重装组件，其实重装组件是一个几乎永远不会实现的梦想。

为了找出 URL 是否有片段，我们可以使用`ActivatedRoute` snapshot 属性:

`this.activatedRoute.snapshot.fragment`

或者简单地在你的`router.url`中找到`#`

`this.router.url.indexOf('#') > -1`

所以我们需要设计的几条基本线是:

```
// we need to run this after the event we think creates and populates our HTML
if (this.router.url.indexOf('#') > -1) {
 this.router.navigateByUrl(this.router.url, {skipLocationChange: true});
}
```

就我个人而言，我使用的是无头 CSM，在我渲染 HTML 之后，我调用一个函数来处理超出原始格式的 HTML。我用区外超时来看起来很酷。😎

注意:关于 SSR，生成的锚被 SEO 爬虫检测到就可以了。

谢谢你对我的容忍。你抓住那只熊了吗？

# 资源

*   [锚点滚动角度文档](https://angular.io/api/router/InMemoryScrollingOptions#anchorScrolling)
*   [GitHub Angular 源代码](https://github.com/angular/angular/blob/b653ee340e3361b484837b028bdd7c4f91a46f85/packages/common/src/viewport_scroller.ts#L119)
*   [角度 15°释放固定](https://github.com/angular/angular/commit/79e9e8ab779d230f6a1df25c4ccff94b13129305)
*   [斯塔克布里兹](https://stackblitz.com/edit/angular-anchor-scrolling)