# Typescript:接口与类型

> 原文：<https://blog.devgenius.io/typescript-interface-vs-type-cb334add97fa?source=collection_archive---------7----------------------->

选择什么不容易。

![](img/40fa909541040daf595442038647d841.png)

华金·索罗拉，1910 年。两种相似的方法。

类型和接口有什么区别？寻找相似之处:

```
interface CruiseMissile {
    distance: number;
    modelName: string;
}type CruiseMissile = {
    distance: number;
    modelName: string;
};
```

# 功能类型

函数可以由关键字`type`和`interface`键入:

```
type Sum = (x: number, y: number) => number;interface Sum {
  (x: number, y: number): number;
}
```

主要的核心区别是接口可以重新声明。类或其他接口可以扩展源接口。另一方面，类型永远不能改变，我们不能给类型添加新的属性。

举例来说，我们有 1 个接口，并希望使用继承:

```
interface CruiseMissile {
  distance: number;
  modelName: string;
}
interface CruiseMissileTask extends CruiseMissile
 { lat: number; lng: number; }const getMissile = (item: CruiseMissileTask): string => {
  return `${item.lat} ${item.lng}`;
};
```

现在，我们可以创建具有所有属性的对象实例:

```
const missile: CruiseMissileTask = {
  distance: 4000,
  modelName: 'XYZ99',
  lat: 0.00,
  lng: 0.00
};
```

一个类型也可以扩展另一个类型。类似这样的东西:

```
type CruiseMissile = {
  distance: number,
  modelName: string
};
type CruiseMissileTask = CruiseMissile & { targetName: string; };const getMissile = (item: CruiseMissileTask): string => {
  return `${item.targetName}`;
};const missile: CruiseMissileTask = {
  distance: 4000,
  modelName: 'XYZ99',
  targetName: 'Black Rock'
};
console.log(missile);
```

或者在转换成数字实际方面使用这个逻辑:

```
private readonly mapToNumber = (value: unknown):
  undefined | number => value && Number(value);this.mapToNumber(this.dataControl.value);
```

# 不同结构的优势

**类型好处:什么类型能做，接口不能做:**

**元组类型** `|`

```
type FlyingSaucer = string | { name: string };
const mySaucer: FlyingSaucer = { name: 'UFO' }
console.log(mySaucer.name);
```

另一个例子:

```
export type AircraftType= 'PLANE' | 'HELICOPTER' | 'SAUCER' | 'UFO';

export const AirType = {
    PLANE: 'PLANE' as AircraftType,
    HELICOPTER: 'HELICOPTER' as AircraftType,
    SAUCER: 'SAUCER' as AircraftType,
    UFO: 'UFO' as AircraftType
};
```

**映射对象类型键/值** —类型可用选项

```
type Aircraft = 'discoVolante' | 'saucer' | 'ufo';// works with types, not with necessary
type AircraftCount = {
  [key in Aircraft]: number;
}const aircrafts: AircraftCount = {
  discoVolante: 10,
  saucer: 1,
  ufo: 2
};
console.log(aircrafts);
```

使用枚举用法作为关键字:

```
enum Engine { petrol = 'PETROL', electric = 'ELECTRIC' }
type TEngine = {
  [k in Engine]: boolean;
}
const engine: TEngine = { PETROL: true, ELECTRIC: false};
console.log(engine);
```

# 什么时候用`interface`？

**多态性**

让我们将接口命名为契约，以实现如何使用该对象。

一个类可以实现一个接口或类型别名，就像任何其他 OOP 一样，两者都以完全相同的方式实现。

```
interface Engine {
  power: number;
  torque: number;
}class EngineParams implements Engine {
  power = 250;
  torque = 488;
}type Engine = {
  power: number;
  torque: number;
};class EngineParams2 implements AnotherEngine {
  power = 100;
  torque = 130;
}
```

接口无法实现/扩展命名联合类型的类型别名。

```
type EngineAsUnionType = { power: number; } | { torque: number; };
class test implements EngineAsUnionType {
  power = 1001;
  torque = 500;
}
// error with union type
```

# 5.声明合并

接口可以多次定义，并将合并为一个。这样使用类型将会产生错误，因为类型不能重新声明。

```
interface User {
  name: string;
}interface User {
  phone: string;
}// no error
const user: User = {
  name: 'alex',
  phone: 'XXX-XXXX-XXX'
};
console.log(`${user.name} , ${user.phone}`);
```

# 什么时候用`type`？

**类型混叠**

```
type Primitive = boolean | null | undefined | number | string;
```

**类型捕捉** —使用 typeof 识别对象的类型。

```
const engine = { capacity: 2200, type: "diesel" }
type Engine = typeof engine;
console.log(typeof engine);// use type to assign new values
const customEngine: Engine = { capacity: 2500, type: 'petrol'};
console.log(customEngine);
```

这里，让我们从对象中提取属性的类型:

```
type ObjectInfer<O> = O extends {message: infer A} ? A: never
const object = {message: 'test'};
type testResult1 = ObjectInfer<typeof object>; // string
type testResult2 = ObjectInfer<string>; // neverconst msg: testResult1 = "result string";
console.log(msg);
```

**通用转换**

当您将多个类型转换成一个通用类型时，使用`type`。

```
type Nullable<T> = T | null | undefined
type NonNull<T> = T extends (null | undefined) ? never : T
type EngineType = { power: number; torque: number; };
const engine : NonNull<EngineType> = { power: 1001, torque: 800 }
console.log(engine.power);
```

**应该避免什么。**

如果是联合类型，那么我们就不能使用实现。

```
type EngineFuel= Petrol | Diesel | Electric | Methane gas | Hydrogen | Ethanol;
class CustomCar implements EngineFuel {
} 
```

不可能多次使用同一类型的名称。

```
type CustomType = {a: string}
type CustomType = {b: number}
```

![](img/d0a18a421d0daf7b305c26bd67663d1e.png)

每个细节中的神秘和象征

**结论**

原则上，使用类型或接口大多是个人选择。看情况。