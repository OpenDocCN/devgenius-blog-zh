# 领域驱动设计中的贫血模型与丰富模型

> 原文：<https://blog.devgenius.io/anemic-models-vs-rich-models-in-domain-driven-design-c5409b286f63?source=collection_archive---------6----------------------->

![](img/eaaf8f5c7c71314d996569f34db6138c.png)

我的商业逻辑应该放在哪里？这是几乎每个开发人员在处理复杂的业务规则时都面临的一个普遍问题，这些规则有很多条件和不明显的需求。在面向对象编程中，有两种主要的代码组织方式:贫血模型和丰富模型。

# 有什么区别？

贫血模型方法就是将数据从业务逻辑中分离出来。通常，看起来您有几个只包含数据字段和一组简单的 getter/setter 方法的模型类。同时，所有相关的业务逻辑都放在服务层中。每个服务通常通过存储库加载一个模型，对其进行一些更改，并更新其在数据库中的状态。一般来说，它与应用程序用例之一相关。

与这种方法相反，Rich Model 将模型数据和相关的业务逻辑组合在一个类中。服务层应该只包含几个不同聚合的逻辑，所有与具体聚合行为和一致性相关的内容都由聚合本身管理。

# 例子

让我们考虑一个取消订单的简单例子:

**贫血模型**

```
class OrderCanceller
  def call
    order = order_repository.find_order

    if order.current_state != :paid
      raise “invalid state”
    end new_state = OrderState.new(state: :cancelled)
    order.states << new_state
    order.current_state = new_state
    order.updated_at = DateTime.now
    # maybe some other logic
    order_repository.update_order(order)
  end
endclass Order
  attr_accessor :id, :states, :current_state, :updated_at
end
```

**富裕模式**

```
class OrderCanceller
  def call
    order = order_repository.find_order
    order.cancel!(DateTime.now)
    order_repository.update_order(order)
  end
endclass Order
  attr_reader :id, :states, :current_state, :updated_at def cancel!(timestamp)
    if @current_state != :paid
      raise “invalid state”
    end new_state = OrderState.new(state: :cancelled)
    @states << new_state
    @current_state = new_state
    @updated_at = timestamp
    # maybe some other logic
  end
end
```

也许这太简单了，对于这么少的代码来说，现在没有太大的区别，但是你已经可以看到重点是逻辑放在哪里。

# 为什么贫血模型是反模式的？

首先，它违背了 OOP 的主要原则之一— **封装**。什么是封装？封装是将数据与操作该数据的方法捆绑在一起。封装用于隐藏类中结构化数据对象的值或状态，防止未授权方直接访问它们。

贫血模型将所有数据暴露给外部服务，并且没有任何能力维护其内部一致性。

如果我们把类的任何实例看作一个有自己的数据和行为的对象，那么一个贫血的模型根本没有行为，它的所有行为都是由使用它的服务决定的。

从 OOP 原理转移到 DDD，我们应该记住不变量的概念。不变量是您在任何给定时间为维护对象的完整性而强加的业务规则/实施/需求。

在贫血模型方法中，聚集不能控制它的不变量。

另一个 DDD 概念——嵌套实体只能通过集合来访问。如果一个聚合只有简单的 setters 和 getters，这也是无法实现的。

据我所知，主要问题是什么？在贫血模型中，所有的逻辑都被不同的用例分组，这个逻辑越复杂，用例就越多，这些用例开始互相矛盾。没有办法理解哪一个是正确的，因为没有模型的一般行为，它分布在独立的服务中。另一方面，一个丰富的模型控制着它的内部一致性，任何用例都不能违反它。

几个独立的服务不可避免地会复制其流程的至少一部分，这将导致代码重复或非常细粒度的小服务，这些服务通常也应该以特定的顺序调用。这使得很难理解模型的一般概念，因此也很难维护它。

让我们回到取消订单的例子。在某个时刻，状态和相关逻辑的数量会变得非常复杂，以至于需要单独放置。在我们的 rich 模型中，它只是模型中的另一个方法:

```
class Order
  attr_accessor :id, :states, :current_state, :updated_at def cancel!(timestamp)
    assign_state(:cancelled)
    # other logic
  end def assign_state(state)
    …
  end
end
```

在贫血模型方法中，那时它可能是一个单独的服务:

```
class OrderCanceller
  def call
    order = order_repository.find_order
    OrderStateAssigner.call(order, :cancelled)
    # other logic
    order_repository.update_order(order)
  end
endclass OrderStateAssigner
  def call(order, new_state)
    …
  end
end
```

现在 OrderCanceller 类应该知道 OrderStateAssigner 服务，甚至更多——处理订单状态的每个其他服务都应该知道它并在正确的时间调用它。让我们想象一下，将会有几个这样的服务，每个服务做自己的一小部分工作。理解您需要在何时打电话给哪一个人有多容易？

换句话说:

> 一般来说，您在服务中发现的行为越多，您就越有可能剥夺领域模型的好处。如果你所有的逻辑都在服务上，那你就把自己洗劫一空了。马丁·福勒

我希望上面的例子很好地说明了另一个重要的设计原则，它被贫血的模型所违背— **低耦合/高内聚。**这意味着不同的模块/类应该尽可能少地耦合，但是单个模块/类内部的元素应该高度互连。相比之下，使用贫血模型会导致高度耦合的域服务，每个域服务都知道其他域服务，以及非常低内聚的模型，这些模型可以被任何客户端服务以任何方式修改。

# 使用贫血模型是一种功能风格吗？

可能看起来是那样，但不是。这是一种程序风格。至少，通常是这样。不同之处在于，函数式编程是关于**表达式、**的，而过程式编程是关于**语句、**的。当您的服务获取一个对象并操纵它的状态时(即使没有保存到数据库中)，这与函数式编程及其纯函数概念相去甚远。

# 什么时候使用贫血模型？

当然，有时使用贫血模型也没那么糟糕。这可能比构建丰富的模型更简单，尤其是当您的应用程序被分成细粒度的微服务时，每个微服务只有几个简单的用例。在这种情况下，使用简单的模型实现起来更快，也更容易理解。你也不应该绞尽脑汁考虑如何选择一个集合和嵌套实体，它应该包含什么行为等等。基本的 CRUD 系统通常不需要这些努力。但这里有一点值得一提——如果你不需要有钱的模特，也许你根本不需要 DDD？领域驱动的设计非常适合处理领域的复杂性，但是对于简单的应用来说，甚至可能是过度工程化的标志。

# 结论

在我看来，贫血模型适合简单的领域，当它不需要深入思考设计的时候。当领域模型没有那么复杂时，这可能是实现它的最简单和最快的方法。在实现过程中，您不需要知道额外的原则，也几乎不需要记住任何约束。

但是当一个真正复杂的业务逻辑出现时，它就变成了一个反模式，有时会让开发人员的生活变得更加艰难。因此，最好从一开始就关心应用程序的设计，并认为富模型方法更适合 OOP 原则。

# 链接

[https://martinfowler.com/bliki/AnemicDomainModel.html](https://martinfowler.com/bliki/AnemicDomainModel.html)

[https://www . amido . com/blog/ANAE mic-domain-model-vs-rich-domain-model](https://www.amido.com/blog/anaemic-domain-model-vs-rich-domain-model)

[https://en . Wikipedia . org/wiki/Encapsulation _(计算机编程)](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming))