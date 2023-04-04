# Typescript 优化了数组的搜索

> 原文：<https://blog.devgenius.io/typescript-optimizes-the-search-of-array-98cdefb1b36?source=collection_archive---------6----------------------->

只是一些关于**在 Typescript 中包含**操作符的想法。

![](img/ed68b80b1c39a3717519fb60eb1f535e.png)

70 年代汽车罕见的设计。蒙特威尔第 375

每天我都要面对不同的任务，这些任务都需要妥善解决。

目标:付款类型在服务中应该是唯一的。但是当它也出现在附加子服务的实现期间时，它应该被移除并且在主阵列中保持唯一。

举例来说，假设我们有一个针对不同服务付费的糟糕的算法。

目标:我们必须检查已经包含在和 **servicePayments 中的两个数组**之间可能的匹配。**如果我们发现付款已经包含在主列表中，则将价格设置为 0。**

```
export interface PriceAmount {
  currentAmount: number;
  originalAmount?: number;
}
export interface ServicePayment {
  paymentType: PaymentType;
  priceAmount: PriceAmount;
}
export enum PaymentType {
  *_Visa* = "Visa",
  *_Mastercard* = "MasterCard",
  *_JSB* = "JSB",
  *_MIR* = "MIR",
  *_UnionPay* = "UnionPay"
}
export interface ChargeIncludedIn {
  code: string;
  paymentType: PaymentType[];
}const alreadyIncludedIn: ChargeIncludedIn =
 { code: "ABC", paymentType: [ "UnionPay" ]}; private checkPaymentTypeIncluded(charge: ChargeGroup): ChargeGroup { if (!charge.alreadyIncludedIn) {
    return charge;
  }
  const servicePayment: ServicePayment[] = 
    charge.servicePayments.**map**(item => {
      const { priceAmount } = item;
      return { ...item, priceAmount: { ...priceAmount, curtAmount: 0 } };
    }); return { ...charge, servicePayment};
}
```

当前地图没有使用已经包含在数组值中的**的迭代。**

让我们添加正确的逻辑。

我们必须检查已经包含在和 **servicePayments** 中的 2 个数组**之间的可能匹配，将 **return** 添加到所有匹配中是不正确的。**

```
 const servicePayment: ServicePayment[] = [];
    for (const item of charge.servicePayments) {
      charge.alreadyIncludedIn.paymentType.**map**(includedIn => {
        if (includedIn.paymentType === item) {
          item.priceAmount.currentAmount = 0;
        }
      });
      servicePayment.push(item);
    }
```

我们还可以优化现有数组的迭代算法，使用 **forEach** 运算符:

```
 const servicePayment: ServicePayment[] = charge.servicePayments;
    for (const item of charge.servicePayments) {
      charge.alreadyIncludedIn.paymentType.**forEach**(includedIn => {
        if (includedIn.paymentType === item) {
          item.priceAmount.currentAmount = 0;
        }
      });
    }
    return { ...charge, servicePayment };
```

将第二个循环和 if 子句结合起来会使代码更容易阅读

在**包含运算符的帮助下，可以像这样进行优化。**它允许将数组迭代和条件检查结合在一起:

```
const servicePayment: ServicePayment[] = charge.servicePayments;
for (const item of charge.servicePayments) {
  if (charge.alreadyIncludedIn.paymentType.**includes**
    (item.paymentType)) {
    item.priceAmount.currentAmount = 0;
  }
}
return { ...charge, servicePayment};
```

**包括**是非常简单和有用的运算符，举例来说:

```
 getDataFromTheService(billiardValue: string): string {
    const billiards = [ "Pool", "Snooker", "Pyramid", "Carom" ];
    if (billiards.**includes**(billiardValue)) {
      return this.resolveService(billiardValue);
    }
    return billiardValue;
  }
```

**箭头方法**

这种符号帮助我们在实现`filter`、`map`和其他数组迭代时不需要编写`return`表达式。

```
const sampleArray: number[] = [100, 200, 300];// use return and parenthesis for big long logic 
const rslt: number[] = sampleArray.map( (num: number) => {
  return num * 2;
})// let's make it compact
const result: number[] = sampleArray.map((num: number) => num * 2 );// output: 200, 400, 600
```

尝试按参数匹配时 if 条件的替代条件:

```
const combiArray: any = {
  a: [1,2,3],
  b: [4,5],
  c: [6]
};
const getItFor = (item: any) => combiArray[item] || [];
const resultMatch = (item: any, variant: any) => getItFor(item).includes(variant);
console.log(resultMatch('a', 1)); // true
console.log(resultMatch('a', 5)); // false
```

**结论**

有许多方法可以实现优化或进行重构来减少代码字符串的数量。