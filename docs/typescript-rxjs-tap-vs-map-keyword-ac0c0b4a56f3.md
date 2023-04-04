# Typescript RxJS tap vs map 关键字

> 原文：<https://blog.devgenius.io/typescript-rxjs-tap-vs-map-keyword-ac0c0b4a56f3?source=collection_archive---------1----------------------->

何时使用运算符进行调试。

![](img/9657076f9d25abfba955682ce45a2fc2.png)

桑德罗·波提切利的《春华秋实》，对立统一于此文。

前端应用程序为调试提供了特定的操作符:`**debugger**`、`**console.log()**`。但是`**tap**` 关键字允许在可观的地方做这件事。

与可观测量一起工作，我们可以用不同的方式处理发射数据。 **Map** 、 **tap** 是可以在管道内链接在一起的操作符，但是它们的行为是不同的:

*   `map`、flatMap、switchMap 等的目的是**转换**可观测的发射值。从一种类型到另一种类型。
*   `tap` 的目的是**执行某种其他动作**保持可观察值不变。点击将返回原始数据和类型。
*   平面图——当我们有一个数据发射时，**平面图**将以任意顺序获取，而不是保持输入数据顺序的**图**。
*   切换图——是一种获取最新排放值并忘记该排放源的任何先前数据的能力。
*   函数式编程为不同的场景提供了许多其他的操作符，比如 concatMap、mergeMap、exhaustMap

Web 浏览器借助 F-12 键提供开发人员工具。 **tap** 最常见的用法是用 **console.log()** 输出调试一些情况或者创建自定义 **myService.notify(msg)** 。

**也用管道来组合运算符**

管道方法接受由逗号分隔的运算符。每个操作符都应该按照一定的逻辑排序，这取决于应用程序的逻辑。当用户订阅一个可观察对象时，管道按照添加操作符的顺序执行操作符。

**什么时候用水龙头？**

1.  使用 tap 进行调试，并观察控制台中的结果数据，而不对流进行任何转换。

```
 this.myService
      .calculate(param1, param2).pipe(
        take(1),
        tap(object => console.log(object)),
        map(object => object.name),
        filter(name => name.endsWith("ltd")),
        tap(name => console.log(name))
      )
      .subscribe(result => {
        // notify client
      });
```

2.定义数据更改时的副作用。

例如，在没有订阅的情况下，服务返回可观察到的结果，tap 实现了副作用:

```
isQuotationCreated(session: SessionState): Observable<boolean> {

  return this.chargeService.isQuotationCreated(quotation).pipe(
    tap(result => console.log(result))
    map(result => result.data),
    tap(result => {
      if (!result) {
        const messageData: MessageData = {
          messageType: MsgType.WARNING,
          title: "No quotation found",
          description: "Lorem ipsum"
        };
        this.messageService.showMessage(messageData);
      }
    })
  );
}
```

另一个关于 Subject 的例子:管道请求一直工作到 observable 被订阅。

```
private readonly destroy$ = new Subject<boolean>();private readonly requestDate$: Observable<Date>;
isSearchingDate: boolean;constructor(
  private readonly dateService: RequestDateService,
) {
  this.requestDate$ = dateService.requestDate$;
}ngOnDestroy(): void {
  this.destroy$.complete();
  this.destroy$.unsubscribe();
}loadData(): void {
  this.requestDate$
  .pipe(
    takeUntil(this.destroy$),
    tap(() => (this.isSearchingDate = true))
  )
  .subscribe(requestDate => {
    // notify the service
  });
}
```

它可能是这样一种不同的副作用:

```
 let actionDispatched = false;
  constructor(private readonly service: MyService) {} function resolve(): Observable<PriceResponse> {
    return this.service.pipe(
      tap(price => {
        if (!price && !this.actionDispatched) {
          this.actionDispatched = true;
          this.service.dispatch(***price***);
        }
      }),
      skipWhile(price => !price),
      take(1),
      finalize(() => (this.actionDispatched = false))
    );
  }
```

3.观察内在的可观察性，以及可观察的操作将如何完成。

```
import { of } from 'rxjs';
import { tap, map, mergeMap, toArray } from 'rxjs/operators';const ids = [0,1,2,3,4,5,6,7,8,9];
const transformTo = (id: number) => of(id).pipe(
  map((data) => data * 10),
  tap({
    complete: () => console.log(`Inner value ${id}`),
  })
);of(ids).pipe(
  mergeMap(ids => ids),
  mergeMap(id => transformTo(id)),
  tap({
    error: (error) => console.log(`Error with ${ids}: ` + error)
  }),
  toArray()
).subscribe(e => console.log(e))
```

4.使用点击显示错误。点击使用 3 种方法:下一步，错误和完成。让我们模拟一下可观测量的误差。

```
 of("Lorem", "Ipsum", "Vice")
      .pipe(
        map(item => item.length),
        tap(item => { console.log("Before " + item) }),
        map(item => {
          if (item == 4) {
            throw Error;
          }
          return item;
        }),
        tap(
          item => {
            console.log("After " + item);
          },
          err => {
            console.log("Error: " + err);
          },
          () => {
            console.log("Complete");
          }
        )
      )
      .subscribe(item => console.log(item));
```

![](img/ade5090455e2da745bc2a2171aaa3ee1.png)

华金·索罗拉

**结论**

函数式编程提供了以正确和简单的方式操作数据的方法。在前端应用程序中调试不像在后端那么容易。

**链接**

[](https://blog.angular-university.io/rxjs-higher-order-mapping/) [## RxJs 映射:切换映射 vs 合并映射 vs 串联映射 vs 穷举映射

### 我们日常发现的一些最常用的 RxJs 运算符是 RxJs 高阶映射…

blog.angular-university.io](https://blog.angular-university.io/rxjs-higher-order-mapping/)