# 有角度的深拷贝与浅拷贝

> 原文：<https://blog.devgenius.io/angular-deep-copy-vs-shallow-copy-685511e43b42?source=collection_archive---------0----------------------->

只是克隆的实际方面，不会对 TypeScript 应用程序的性能产生影响。

![](img/5694db73e0d94b097740d181a572b94c.png)

莫斯科“西南剧院”上演“拿破仑戏剧”

就像上图中的单元格一样，Angular 应用程序有很多对象和属性。根据 1 组件中 UI 变化的逻辑，我们经常需要更新嵌套组件并应用新值。但是在使用 deepcopy()方法的情况下，这可能会花费时间并降低性能。深度副本会以 100%的概率更新相似的对象，但这和*孙子*说的“向麻雀开炮”是一样的。

**一、理论部分。**

因此，我们在进行深层复制、浅层复制或对象赋值之间进行选择。

1.  任务。A = B。它只是对对象进行引用，而不是复制。
2.  浅拷贝意味着某些子值仍然连接到原始变量。扩展运算符表示的符号。

```
private rack: BilliardRack[];get BilliardRack(): BilliardRack[] {
  return this.rack.map(ball => ({ ...ball }) );
}
```

3.深度复制意味着新变量的所有值都被复制并与原始变量断开连接。可能有不同的深度拷贝库，我更喜欢使用来自`/node_modules/deep-copy-ts`的标准

```
export declare function deepCopy<T>(target: T): T;
```

复制对象的不同和最广泛的变体:

```
const billiard = {
  type1: "Snooker",
  type2: "Russian Pyramid",
  type3: "American Pool",
  players: ["Ronnie O'Sullivan", "Aleksandr Palamar", "etc"],
};// use shallow copy with spread operator of 3 dots
const billiardGame = { ...billiard };
// the most fast method of copy
const billiardGame = Object.assign({}, billiard);
const billiardGame = Object.assign(obj, { type3: "Finnish Kaisa"} );// or make deep copy
const billiardGame = JSON.parse(JSON.stringify(billiard));
const billiardGame = deepCopy(billiard); billiardGame.type3 = "French caromball";
console.log(billiard.type3);
console.log(billiardGame.type3);        //new value will be applied // You can also use shallow copy to merge many objects together:
const billiardTotal = { ...billiard, ...billiardGame};
console.log(billiardTotal);
```

但是如果您直接赋值，它就不起作用:

```
const obj = {
   item1: 1,
   item2: 2
};

**const newObj = obj;         // assignment**const newObj.newItem = 3;
console.log(newObj);        // {item1: 1, item2: 2, newItem: 3}// and incorrect behavior:
// we expect the old variable to have the original values, not the changed ones, but it doesn't.console.log(obj);           // {item1: 1, item2: 2, newItem: 3}
```

数组也是对象，所以复制方法的行为相同:

```
const gamers = ["Ronnie O'Sullivan", "Aleksandr Palamar"];
let players = gamers.map(el => el);
players[2] = "John Higgins";console.log(players[2]); // shows new value
console.log(gamers[2]);  // keep the old original
```

**二。实用部分。**

![](img/8368bb587b5f45dda2a1d311f6b6ea33.png)

像国际象棋中的细胞一样的对象属性

还是实际一点吧。angular 应用程序可能很复杂，一个 HTML 页面由许多相互绑定的组件组成。

假设我们在`updateCharges()`方法的子组件中获得了新的值，需要更新正确的父组件对象。**输出**注释帮助我们将数据发送出去。

```
@Output()
chargeTermsEvent = new ***EventEmitter***<ChargeTerms>();private updateCharges(charges: ChargeTerms) {
  console.log("get new charges, send to parent"); 
  this.chargeTermsEvent.emit(charges);
}
```

在另一个组件中，在 HTML 层，我们从方法`chargeTermsEvent` 获取数据，并将新数据发送给方法`handleChargeTerms`

```
<charges-tariff class="sub-row"
               [tariffs]="***item***"
               (chargeTermsEvent)="handleChargeTerms(***$event***)"
               />
```

并在类型脚本级别使用该方法。现在我们可以处理新收到的数据。在浅拷贝的情况下，我们使用基于 3 个点的扩展操作符。新值将应用于该组件中声明的局部对象。

角度变化检测机制几乎立即检测到变化，并提前发送数据。

```
private chargeTerms: ChargeTerms;handleChargeTerms($event: ChargeTerms) {
  this.chargeTerms = $event;
  this.chargeTerms = { ...this.chargeTerms}; // use shallow copy
  this.hasLeastChargesAdded();
}

isChargesAdded(): boolean {
  return this.chargeTerms !== undefined;
}
```

但是我们也可以使用**深度拷贝**来明确地重新分配对象的每个属性。我使用 Angular 的标准库。

```
import { deepCopy } from "deep-copy-ts";this.chargeTerms = deepCopy(this.chargeTerms);
```

所以，最后，我们的父组件用新数据更新，你可以处理它——将数据发送到另一个子组件的方法`chargeTermsDisabled` ,并在 HTML 端显示它。方法`isChargesAdded()` 将根据`chargeTerms`返回布尔值。

```
<charges-tariff-request #t***ariffComponent***
  [chargeTermsDisabled]="isChargesAdded"
  />
```

使用复制方法的最实际的方面——确保所有新的对象属性将被重新分配给一个局部变量。

在父/子组件之间传输新数据，并更新将在当前页面的 HTML 侧呈现值的某些对象。因此，对象的每个单元格、每个属性都会更新。

**增补 1。**

Spread 运算符从`obj` 获取所有字段，并将其分布在`result`上。

```
const obj = { param: "value", param2: "abrakadabra" };
const result = { ...obj, param2: "new real value" }; // add new value to existing param2
console.log(result);
```

**增补 2。**

让我们从对象中删除一个属性。析构将帮助我们创建一个浅层拷贝。

```
type CruiseMissile = {
  color: string
  technicalData: {
    distance: number;
    ageOfManufactured: number;
  }
}

const missile: CruiseMissile = {
  color: 'white',
  technicalData: {
    distance: 4000,
    ageOfManufactured: 1979
  }
}

const { distance, ...technicalData} = missile.technicalData;
const result = {
    ...missile, technicalData // 'distance' property will be missed.
}
console.log(result);
```

![](img/df58e55b6ce97c48f7f8923a52199c10.png)

我喜欢象棋细胞

**结论**

对于应用程序性能而言，使用浅层拷贝是比深层拷贝更便宜的解决方案，但使用情况取决于某些情况。更新嵌套组件可能有不同的方法。最大的错误是从标准角度生命周期的 ngOnChanges()方法调用 deepCopy()。

**链接**

[](https://frontbackend.com/javascript/what-is-the-the-fastest-way-to-deep-clone-an-object-in-javascript) [## 【已解决】在 JavaScript 中深度克隆一个对象最好最高效的方法是什么？

### 在每种编程语言中，有时都需要一个对象的精确副本。忠实的复制…

frontbackend.com](https://frontbackend.com/javascript/what-is-the-the-fastest-way-to-deep-clone-an-object-in-javascript) [](https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/) [## 如何在 JavaScript 中区分深层和浅层拷贝

### 如何在 JavaScriptPhoto 中区分深层和浅层拷贝？

www.freecodecamp.org](https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/) [](https://bobbyhadz.com/blog/javascript-cannot-assign-to-read-only-property-of-object) [## 无法在 JavaScript | bobbyhadz 中为对象的只读属性赋值

### 当我们试图改变一个具有…的对象的属性时，出现错误“不能分配给对象的只读属性”

bobbyhadz.com](https://bobbyhadz.com/blog/javascript-cannot-assign-to-read-only-property-of-object) 

**奖励链接**。我在莫斯科“西南剧院”最喜欢的戏。

[](https://teatr-uz.ru/igra-v-napoleona) [## Игра в Наполеона

### Название: Игра в Наполеона Автор: Стефан Брюлотт, перевод Ларисы Овадис Жанр: Шахматный детектив Количество действий: 2…

teatr-uz.ru](https://teatr-uz.ru/igra-v-napoleona)