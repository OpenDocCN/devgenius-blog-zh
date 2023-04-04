# 构造函数中的角度依赖注入

> 原文：<https://blog.devgenius.io/angular-dependency-injection-de5ce8ec0961?source=collection_archive---------5----------------------->

重构总比不重构好。

![](img/7b1639fd3f1d92f59ffce01fb84802ca.png)

华金·索罗拉，印象派，1908 年，“巴伦西亚海滩的下午”

有时这是一个选择什么的问题:每次我们调用方法时创建一个类的实例。或者在构造函数中声明一次。

**一些实用的理论迪。**

如果我们将类的实例传递给构造函数，就可以减少代码字符串的数量和维护工作。**构造函数**是类被实例化时执行的类的默认方法。构造函数基本上确保所有的类变量都被正确初始化。它也用于**依赖注入**，以我的`CurrencyChangeService` 为例如下:

```
private readonly destroy$: Subject<void> = new Subject();
private currencyResult: CalculationResult;constructor(private readonly currencyServ: CurrencyChangeService) {}ngOnInit() { // lifecycle first hook called right after constructor //and use it in the code just like this:
  this.currencyServ.convert()
    .takeUntil(this.destroy$)
    .subscribe(response => this.currencyResult = response);
}
```

依赖注入(DI)是根据依赖关系自动创建对象。这是坚实原则的支柱之一。

```
private result: MyModelResult;
constructor(myService: MyService) {
   this.result = myService.calculateResult(); 
}
```

每次调用当前类时，Angular injector 都会注入一个 MyService 实例。Angular 观察构造函数参数，它通过调用 new MyService()创建一个新的实例。他寻找与构造函数参数的类型相匹配的提供者，解析它们并将它们传递给构造函数，如 new MyService(param)。

![](img/aee45b15018983ff8d00c88dd6b1b315.png)

**现在是时候观察一些实际的重构任务了。**

假设我们有一个方法 calculateMyCharges，它通过每次调用函数来创建自己的 ChargeCalculation 类实例。

这不是一个最佳的解决方案:我们不应该在每次调用方法时都创建一个新的服务实例。让我们调整一下，calculateMyCharges 函数将汇率作为参数。以此类推其他参数。只使用 DI 来注入服务和类似的类。

重构前的代码:

```
export interface CalculationResult {
  calculatedCharges: CalcOutput[];
}
@Injectable({
  providedIn: "root"
})
export class ChargeService {
  constructor(private readonly logger: LoggerService) {} calculateMyCharges(myCharge: Charge,
    exchangeRates: ExchangeRate[]): CalculationResult { **// created each time
    const calculator = new ChargeCalculator(
      new CurrencyChangeService(this.logger));** return calculator.calculate(myCharge, exchangeRates);
  }
}
```

解决方案是移走类的实例创建:

```
const calculator = new ChargeCalculator(
  new CurrencyChangeService(this.logger));
```

重构后，我们会得到这样的结果:

```
@Injectable({
  providedIn: "root"
})
export class ChargeService {
  constructor(private readonly logger: LoggerService,
    private readonly chargeCalculation: ChargeCalculator) {} calculateMyCharges(myCharge: Charge,
      exchangeRates: ExchangeRate[]): CalculationResult {

    return this.chargeCalculator.calculate(myCharge, exchangeRates);
  }
}
```

我们的类将从映射器中这样调用:

```
export class MainMapperAssembler {
  private readonly chargeService =
    new ChargeService(this.logger, this.chargeCalculator); constructor(private readonly chargeCalculator: ChargeCalculator) {} callMyCalculateMethod() {  
    this.chargeService.calculateMyCharges(
      selectedCharge, exchangeRates); ...
  }
}
```

为了避免"`NullInjectorError: No provider for ChargeCalculator!`"的问题，我们必须在 ChargeCalculator 类的顶部添加@ Injectable:

```
@Injectable({
  providedIn: "root"
})
```

Angular 不是向导:我们必须手动将值写入“providedIn”属性。因此，用@ Injectable 修饰的 ChargeCalculator 的一个简单变体意味着我们在应用程序中注册服务。

```
@Injectable({
  providedIn: "root"
})
export class ChargeCalculator {
  exchangeRates: ExchangeRate[];

  constructor(private readonly currencyChangeService: CurrencyChangeService) {}

  calculate(exchangeRate: ExchangeRate[]): CalculationResult { const chargePrice = this.currencyChangeService
      .convertPrice(this.exchangeRates); ...
  }
}
```

Angular 中所有注册的依赖项都可以被限定范围。他们分成三组:

1.  根级别的范围是整个项目。实例在所有模块和组件之间共享。向您计划提供或注入的任何东西添加一个@ Injectable 装饰器。

```
@Injectable({
  providedIn: 'root'
})
```

2.模块——延迟加载模块中提供的实例在模块的组件之间共享。这里的“Logger”是一个注入令牌，注入器用它来标识类。

```
@NgModule({
  declarations: [AppComponent],
  imports: [ApiModule],
  providers: [{ provide: Logger, useClass: Logger }]
})
```

3.组件-注入到组件中的实例在组件的生命周期中存在。UseValue 属性用于设置特定值:

```
@Component({
  providers: [
    { provide: MyService, useValue: { request: 'test' } },
    { provide: MyService2, useValue: () => { **return** 'test' } },
    { provide: 'array', useValue: [10, 20] } }
  ]
})
```

**结论**

Angular 中内置的依赖注入是一种非常简单明了的方法。本文只是描述了一些实践经验，并不是整个 DI 概念。

**链接**

[](https://indepth.dev/posts/1148/how-to-avoid-angular-injectable-instances-duplication) [## 如何避免角注射实例重复-角深度

### 这个主题在其他文章和文档中被反复描述过，但是当我面对这个问题时，我仍然…

深度开发](https://indepth.dev/posts/1148/how-to-avoid-angular-injectable-instances-duplication)  [## 有角的

### Angular 是一个构建移动和桌面 web 应用程序的平台。加入数百万开发者的社区…

angular.io](https://angular.io/guide/dependency-injection-providers)