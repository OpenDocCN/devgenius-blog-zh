# Typescript RxJs takeUntil，takeWhile

> 原文：<https://blog.devgenius.io/typescript-rxjs-takeuntil-takewhile-34e7939e4fba?source=collection_archive---------4----------------------->

如何及时停止数据发射？

![](img/76ec196e0f5b5829cda54cd7983d9704.png)

反应式函数编程的魔力。

基于 3“O”原则的反应式方法。

可观察的—操作者—观察者

1.  可观察的—一些输入数组、数据流、异步集合…
2.  操作员—允许对发出的数据进行操作。
3.  观察者—消耗排放数据。在操作员的帮助下，当可观察的物体将被结束时，你将被通知。

可观察的对象只有当你订阅它们时才会起作用。

假设我们有一个可观察的管道。经常需要停止执行流并通知另一个服务。RxJs 提供了 2 个以上的操作员来处理这种情况。

1.  当一些外部事件表明是时候退订时，使用 **takeUntil** 。
2.  当输入值达到退订条件时，使用 **takeWhile** 。

在处理可观察对象的过程中，有一个很好的做法来区分这些变量，例如使用 **$** 符号。所以我指的是结果美元。

```
export class MyComponent implements OnInit, OnDestroy {private resultValue: number;
private readonly destroy$: Subject<void> = new Subject();constructor(private readonly calcService: CalculationService) {
}private checkPriceState(): void {
  this.calcService.getLatestData()
    .pipe(takeUntil(this.destroy$),
      .tap(data => console.log(data) ))
    .subscribe((value: number) => {
      this.resultValue = value;
    });
}ngOnDestroy(): void {
  this.destroy$.next();
  this.destroy$.complete();
}
```

angular life cycle method**NgOnDestroy**允许我们控制何时在组件期间删除订阅。

**TakeUntil** 操作者帮助我们在 **destroy$** 主体发出时停止发出可观察值。它可以防止内存泄漏。

另外， **destroy$** subject 是私有的，所以它在我们的类之外是不可用的。我们不允许任何人将数据放入其中！

他们之间的主要区别是基于这样一个事实，即**直到**接受可观察的值。

**TakeWhile** 只要满足某个条件 stateUpdated 谓词就会发出值。

```
private changeState = false; // used for outer component
private charges: ChargesObject[] = [];
isSaving$: Observable<boolean>;constructor(private readonly store: Store<State>) {
  this.isSaving$ = this.store.pipe(select(s***electors***.isSaving));
}updateWithGlobalState(): void {
  let stateUpdated = true;
  this.isSaving$.pipe(
    takeWhile(() => stateUpdated)).subscribe(hasSaving => {
    if (!hasSaving) {
      const state = this.store.select(s***electors***.getState);
      state.pipe(take(1)).subscribe(result => {
        stateUpdated = false;
        this.charges = deepCopy(result);         this.changeState = true;
      });
    }
  });
}
```

您还可以添加第二个参数 boolean `inclusive`，它将包含第一个为`takeWhile`返回 false 的值。有时需要查看打破条件的值。

```
source$.pipe(takeWhile(predicate, true));
```

举例来说:

```
const breakParam = "Y";
const result = of(["X","Y","Z"])
.pipe(takeWhile(v => v != breakParam, true))
.subscribe(item =>
  console.log(item);
);
```

我们也可以使用运算符`endWith(value)`。它在完成时发出给定值。

```
const result = source$.pipe(
  takeWhile(item => item != 5), endWith(5))
  .subscribe(value => console.log(value),
      err => console.log('error', err),
      () => console.log('complete'));
  }or let's take first 3 valuesinterval(1000).pipe(
      take(3),
      endWith(999)
    ).subscribe(value => console.log(value));
```

# 过滤器与耗时

在 RxJS 中比较这两个相似的操作符也很有趣:`takeWhile` & `filter`

我们试图将该值与某些条件相匹配，并丢弃其他条件。但是，`takeWhile`只有在满足中断条件操作符之前才与可观察对象一起工作。虽然`filter`永远不会停止可观测。

```
const source$ = of(1,1,2,2,1,3,4);source$
  .pipe(takeWhile(it => it === 1))
  .subscribe(val => console.log('takeWhile', val)); //1,1// filter will filtered out all '1' items.
source$
  .pipe(filter(it => it === 1))
  .subscribe(val => console.log('filter', val)); //1,1,1// filter will take only 1 value that more than 2.
source$
  .pipe(filter(x => x > 2), takeLast(1)).subcribe(...); //4
```

**结论**

所有这些操作符看起来都很有逻辑，读起来也很简单，尽管总会出现一些意想不到的行为。

RxJS、RxJava、RxKotlin 中有很多类似的运算符可以让你及时停止订阅:first、single、take、takeLast、takeUntil、takeWhile。运营商将帮助您管理如何订阅以及何时手动或自动退订。Rx 允许最小化该过程的例行程序。

**链接**

 [## RxJS

### 编辑描述

rxjs.dev](https://rxjs.dev/guide/overview)  [## RxJS

### 编辑描述

rxjs-dev.firebaseapp.com](https://rxjs-dev.firebaseapp.com/api/operators/endWith)