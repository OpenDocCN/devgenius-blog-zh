# Typescript 泛型非常实用

> 原文：<https://blog.devgenius.io/typescript-generics-is-so-pragmatic-bf3028401742?source=collection_archive---------11----------------------->

只是 Angular app 现实生活中的一些实用编程瞬间。

![](img/be16b4e22663bbcea5a15a2fb6995c9f.png)

安德烈·埃西奥诺夫绘画

泛型方法它是一种特殊的函数，在成员之间提供有意义的类型约束。

```
function treatMethod<T>(args): TypeResult
```

1.  特殊的 T char 是 type 变量，我们可以使用 type 作为方法的参数。我们的方法将返回一个字符串类型的结果。

```
function trestMethod<T>(args): TtreatMethod<string>("Test");
```

2.我们还可以将类型变量`T`用于函数的任何参数。

```
function treatMethod<T>(arg: T): TtreatMethod("Test");
```

这是一种论证推理。编译器会将 T 的值设置为我们传入的参数。

3.我们也可以使用 extends 操作符来表示我们的`T`类型变量的约束。参数变成编译器的字符串。

```
interface ComplexName{
  name: string;
}
function treatMethod<T extends ComplexName>(arg: T): T {
  arg.name = treatAndParse(arg.name);
  return arg;
}
```

![](img/80dd2b373f3918abf28cb417ebcae934.png)

安德烈·埃西奥诺夫，水彩画

**一些练习的例子:**

假设我们有一个包含两种汽车引擎的大量逻辑的界面:电动引擎和汽油引擎。

```
export interface Car {}export interface PetrolCar extends Car {
  engineIgnition: boolean;
  isCharged: boolean;
  dataInfo: PetrolCarInfo;
  type: LCLQuotationTs.TypeEnum;
}export namespace PetrolCar{
    export type TypeEnum = 'PetrolCar';
    export const ***TypeEnum*** = {
        PetrolCar: 'PetrolCar' as TypeEnum
    };
}export interface ElectricCar extends Car {
  engineIgnition: boolean;
  isCharged: boolean;
  dataInfo: ElectricCarInfo;
  type: LCLQuotationTs.TypeEnum;
}export namespace ElectricCar{
    export type TypeEnum = 'ElectricCar';
    export const ***TypeEnum*** = {
        ElectricCar: 'ElectricCar' as TypeEnum
    };
}
```

我们不想用两种类型得到一堆乱七八糟的代码，所以将借助一个变量“car”来实现处理不同类型的泛型方法。

```
export abstract class AbstractRequestComponent<T extends Car> implements OnInit {
  car: T; ngOnInit() {
    if (this.car as unknown as PetrolCar) { this.engineIgnition(this.car); } else if (this.car as unknown as ElectricCar) { if ( this.isCharged(this.car) ) {
        this.engineIgnition(this.car);
      }
    }
  } abstract engineIgnition(car: T): Ignition; abstract isCharged(car: T): boolean;
}
```

为了使代码可读，我们将方法实现分成两个不同的类。

此外，我们将业务逻辑转移到服务中，在没有 UI 的情况下只处理数据。

```
@Component({
  templateUrl: "./petrolcar-request.component.html"
})
export class PetrolCarRequestComponent extends
  AbstractRequestComponent<PetrolCar> { constructor(
    readonly ignitionService: IgnitionService
  ) {
    super(ignitionService);
  } engineIgnition(car: PetrolCar): boolean {
    return ignitionService.startIgnition(car);
  } isCharged(car: PetrolCar) {
    throw new Error("Couldn't check charge in petrol engine!");
  }
}
```

和处理电机数据的第二个类似类:

```
@Component({
  templateUrl: "./electriccar-request.component.html"
})
export class ElectricCarRequestComponent extends
  AbstractRequestComponent<ElectricCar> { constructor(
    readonly chargeService: chargeService,
    readonly ignitionService: IgnitionService
  ) {
    super(chargeService, ignitionService);
  } engineIgnition(car: ElectricCar): boolean {
    return ignitionService.startIgnition(car);
  } isCharged(car: ElectricCar): boolean {
    return chargeService.Charged(car);
  }
}
```

服务方法将动态类型作为参数:

```
@Injectable({
  providedIn: "root"
})
export class IgnitionService { startIgnition<T>(car: T): boolean {
    let result: boolean;
    if (isPetrolCar(car)) {
      ...
    } else if (isElectricCar(car)) {
      ...
    }
    return result;
  }
}export function isPetrolCar(car: Car): car is PetrolCar{
  return car !== null && (car as PetrolCar).type ===
    PetrolCar.***TypeEnum***.PetrolCar;
}
export function isElectricCar(car: Car): car is ElectricCar{
  return car !== null
    && (car as ElectricCar).type=== ElectricCar.***TypeEnum***.ElectricCar
    && !!(car as ElectricCar).dataInfo;
}
```

**结论**

在这个特殊的例子中，我使用了 MVC 方法(模型-视图-组件)。但是使用泛型函数也有很多好处，可以将相似的代码逻辑分成不同的子类。