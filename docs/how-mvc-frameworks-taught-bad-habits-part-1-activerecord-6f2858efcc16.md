# MVC 框架如何教会我们坏习惯——第 1 部分 ActiveRecord

> 原文：<https://blog.devgenius.io/how-mvc-frameworks-taught-bad-habits-part-1-activerecord-6f2858efcc16?source=collection_archive---------12----------------------->

## 让我们回顾过去，试图弄清楚框架是如何教会我们不去注意错误的事情的。

![](img/f3f105aeac59093f67f89852b0b23852.png)

坏习惯

# 介绍

让我们回到 200X。这是 MVC 框架的繁荣时期。Ruby on Rails，Django，ZendFramework，Spring 等。不同编程语言中的各种框架创建、复制并改进了各种特性。

框架在 web 开发中迈出了巨大的一步。他们给这个行业带来了很多好的东西，禁止为开发者做坏事。使用框架让初学者很容易开始。我的网页开发职业生涯始于 2007 年。几年来，我一直在使用这些框架，而没有考虑这些框架提供的架构和工具。

我们如此习惯于使用框架，以至于我们很难想象可能会有任何替代选项。他们无意中教会我们依赖他们使用的方法。问题是适用于中小型应用的**方法并不适用于大型应用**。反之亦然。因此，如果您正在处理中小型应用程序，您没有什么可担心的。但是，如果您觉得您的应用程序已经缺少了以前的灵活性，欢迎您。

我想分享一些我看到的框架无法有效处理的大型应用程序的缺点。我希望你从不同的角度看待熟悉的事物

我们从 ActiveRecord 模式开始。并不是所有的框架都使用它。因此，如果您最喜欢的框架使用存储库模式，那么一切都很好。你可以跳过这篇文章。

# 我们为什么喜欢 ActiveRecord

还记得在 15 分钟内创建一个博客有多酷吗？当时，许多框架提供了 CLI 甚至 Web 界面，用于在一个动作中创建模型、控制器和视图。你得到了一个和这个相似的骨架。

```
def create
  **@user** = User.new(user_params)
  if **@user**.save
    redirect_to **@user** else
    render *'new'* end
end
```

`Convention Over Configuration`和`Keep It Simple`是主要规则。构建这样一个简单代码的基础是 ActiveRecord。不是所有，但是非常多的框架使用和使用活动记录，不提供任何替代

现在你可以在各种框架指南中找到非常相似的代码。代码简短而优雅。那么 ActiveRecord 有什么问题呢？

# ActiveRecord 怎么了

乍一看，非常方便和直观。实际上**当你处理简单的应用程序时，它非常好用。**

但是在现实生活中，创建新用户或订单不仅仅是写入数据库。如果我们谈论的是用户创建——这是一个复杂的注册过程。让我们估计一下在注册过程中可能会出现哪些附加操作:

*   保存头像
*   来自第三方来源的预填充数据
*   重要的验证—呼叫第三方
*   不同的注册场景——通过 FB 注册、通过邀请函注册、通过表格注册等
*   附加验证—电话号码验证、信用卡的分配和验证
*   黑名单检查
*   附加 post 逻辑——通知其他服务关于新用户的信息
*   等等

订单创建、预约安排、甚至邮件创建等——在现实生活中，业务逻辑比乍看起来要复杂得多。ActiveRecord 如何应对所有这些商业规则？

## 扩展您的验证规则

这些框架提供了广泛的验证能力。您的验证规则像杂草一样生长，试图跟上业务规则。(PHP 和 Yii1)

```
<?php 

class Yii1Model 
{
    public function rules()
    {
        return array(
            // Required fields
            array('customer', 'required', 'message' => Yii::t('rezdy', 'Please create or select a customer'), 'on' => 'create, update'),

            // Safe on booking form
            array('customer, customer[mailingAddress], additionalFields, paymentOption, payFullAmount', 'safe', 'on' => 'bookingForm, partners'),
            array('customerAgreedToTermsAndConditions', 'safe', 'on' => 'bookingForm'),

            // Safe on create and update
            array('status, surcharge, sourceChannel, agentUserName', 'safe', 'on' => 'create, update'),
            array('internalNotes, resellerComments, resellerReference, agentUserName', 'safe', 'on' => 'partners'),

            // Safe on commissions report
            array('startTime, endTime, dateRange, resellerId, reconciled, lastInvoice', 'safe', 'on' => 'commissionDetails'),

            // Top level fields with children
            array('customer, items, paymentDetails, creditCard', 'safe'),
            array('status, searchString, startTime, endTime, dateRange, productId, paymentStatus, entityState', 'safe', 'on' => 'search'),

            // Special fields
            array('date', 'date'),
            array('customer[phone], customer[mobile]', 'match', 'pattern' => '/^([0-9+() -])+$/'),

            // Trim
            array('comments, internalNotes, voucherCode, resellerComments, resellerReference', 'filter', 'filter' => [$this, 'sanitize']),

            // Email validation
            array('customer[email]', 'ext.validators.EmailValidator', 'allowEmpty' => true),

            //Check for TC agreement
            array('customerAgreedToTC', 'ext.validators.EConditionalValidator', 'conditionalRules' => array('companySettings[explicitAgreementToTermsAndPolicy]', 'compare', 'compareValue' => true), 'rule' => array('required'), 'on' => 'bookingForm'),
            // Max length
            array('email, customer[email]', 'length', 'max' => 254),
            array('customer[phone]', 'length', 'max' => 25),
            array('customer[firstName], customer[lastName], customer[mailingAddress][addressLine], customer[mailingAddress][addressLine2], customer[mailingAddress][city], customer[mailingAddress][state]', 'length', 'max' => 200),
            array('customer[mailingAddress][countryCode]', 'length', 'max' => 4),
            array('customer[mailingAddress][postCode]', 'length', 'max' => 10),
        );
    }
}
```

如果您的模型处理嵌套或相对模型，我们还可以添加一些选项。Ruby on Rails

```
**#** [https://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html](https://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html) **class Member < ActiveRecord::Base**
  has_many :posts
  accepts_nested_attributes_for :posts, allow_destroy: **true, ** update_only: **true**, reject_if: proc { |attributes| attributes['title'].blank? }
**end**
```

如果你需要一些更新后的逻辑，你可以用

```
class PictureFile < ApplicationRecord
  after_commit **:delete_picture_file_from_disk**, on: **:destroy** def delete_picture_file_from_disk
    if File.exist?(filepath)
      File.delete(filepath)
    end
  end 
end
```

你的模特知道如何

*   验证不同场景的用户输入
*   将更新保存到数据存储
*   找到适合不同场景的模型
*   更新甚至验证相关模型
*   制定一些更新后的逻辑(通常与当前领域没有直接关系)

如果我错了，请纠正我，但是一个非常常见的模式是这样的

*   你需要一个新功能
*   您在数据库中创建新表
*   在您的代码库中创建一个允许进行 CRUD 操作的模型
*   您不断地将与该领域模型相关的功能添加到单个类中

如果您的项目继续发展，新的需求定期出现，几个月后您会发现您的`models` 文件夹有 200 多个类。你所有的模特都互相认识。

毕竟，你会得到像[这个](https://gist.github.com/radzserg/79a2eaeacd247fec2c12128f60b9b4e3)一样的怪物。只是它太大了，不能放在这里，但看看吧。承认这一点有点尴尬，但这部作品的作者是我🤫。写于 2014 年左右。但是我见过更大的模型。

如此巨大的**模型打破了单一责任原则**。方法的数量不再允许我们的大脑有效地使用它(找出[为什么大脑很难理解意大利面条代码](https://medium.com/@radzserg/why-is-it-hard-for-a-brain-to-understand-spaghetti-code-278d9b5d760c))。最后，你的班级有 1K 或 10K 线已经不重要了。甚至考虑到你可以一直处理 Ctrl+F，这种方法的主要问题- **一切都是紧密耦合的**。现在你开始意识到你的代码极度缺乏灵活性(虽然你仍然可以抵制它)。你需要检查更多的地方，以确保你不会打破任何东西。问题是，这是一个相当典型的代码，在 2020 年的现在很常见。当一个团队和代码成长时，你开始理解现在的变化需要更多的时间。而**当初很方便的现在缺乏灵活性**。

# ActiveRecord 只是一个工具

您曾经不得不将应用程序从一个框架移植到另一个框架吗？平时那么费劲，什么都从零开始更容易。而这些问题的罪魁祸首就是紧耦合模型。

像`AcriveRecord`这样的模式已经成为一些框架中事实上的标准。我们没有围绕业务规则构建领域层，而是按照提供的指南，围绕提供的工具(如 ActiveRecord)构建业务规则。您的业务规则不关心您是使用 ActiveRecord 还是 Repository 模式，您是使用 Mysql 还是 DynamoDB。**业务规则仍然是您系统的一个组成部分**，而活动记录只是一个工具。作为开发人员，我们可以改变保存数据的方式，我们可以改变存储方式，但我们不能改变业务规则。生意决定规则。我们为这些业务规则构建应用程序。

**业务规则不应该依赖于工具。您必须首先设计实现您的业务规则的高层组件。然后才拿起低级工具。而不是相反。数据库只是一个你可以改变的细节。**

# 如何正确地做它

假设您需要实现应用程序中处理订单的部分。订单是在上一步中创建的，现在您需要付款。您有以下要求:

*   您需要验证用户输入
*   您需要重新计算总金额，并确保它与我们在上一步中显示的金额相匹配
*   通过 X 支付网关处理支付
*   更新数据库中的订单状态
*   将新订单通知仓库模块
*   为客户发送收据

与其在数据库中创建一个新表和相关的 ActiveModel，不如让我们用这些高层次的需求做一个草稿

```
class class OrderService {
    public Order processOrder(Order order) {
        *// validate input
        // re-calculate total amount with TAX and make sure that the match previous price
        // process payment
        // update order state in DB
        // notify warehouse
        // send receipt* } public createOrder(...)  {}
}
```

如您所见，保存数据只是众多步骤中的一步。高层操作和它们之间的相互作用现在更重要了。我想推迟保存数据的组件的实现。我该怎么做？

```
public class OrderService {
    priivate final OrderRepository repository;
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }

    public Order processOrder(Order order) {
        *this.orderValidator.validate(order);
        Long total = this.orderPrice.calculateTotal(order);
        if (!total.equal(order.getTotal())) {
            throw new OrderPriceException("Price has been changed")
        }
        try {
            this.stripePaymentGateway.processOrder(order);
        } catch (StripeException exception) {
            //* }***order =* this.repository.confirm(order);** this.applicationEventPublisher.publishEvent(new OrderConfirmed(order)); *this.customerNotifier.sendOrderReceipt(order);* }
    public createOrder(...)  {}
}
```

其中 OrderRepository 是一个接口，甚至不是真正的实现。我使用了[存储库](https://martinfowler.com/eaaCatalog/repository.html)模式。好了，现在我准备好测试我的订单服务了。

```
@ExtendWith(MockitoExtension.class)
public class ChargeServiceTest {

    @Mock
    private OrderRepository orderRepository;

    @InjectMocks
    private OrderService orderService;

    @Test
    public void testProcessOrderSuccess() {
        // test validation
        // test order price recalculation
        // etc
        *when*(orderRepository.confirm(order)).thenReturn(order);
    }
}
```

我们能够构建和测试实现业务规则的高级组件。

**注:**

*   我不需要数据库或与 DynamoDB 或任何其他存储的集成来做到这一点
*   代码不依赖于框架或特定的库来处理数据库。
*   这很容易测试。我们将 OrderRepository 视为另一个单元。它将接受另一项测试。由于我们不需要将数据写入真实的数据库，所以测试是单元的和快速的。
*   组件的结构使用对业务和开发人员来说很清楚的通用语言

如果我们想将我们的应用程序迁移到新的框架中，我们只需照原样复制代码，它就会继续工作。它没有任何硬编码的依赖关系。

等等，但是我们实际上需要将数据保存到存储器中。我们需要一个真正的实现。真的吗？哦，对了。好了，补充一下吧。这些天你更喜欢什么？Mysql 或 DynamoDb

```
[@Repository](http://twitter.com/Repository)
public class MysqlOrderRepository implements OrderRepository {

    [@PersistenceContext](http://twitter.com/PersistenceContext)
    private EntityManager entityManager; [@Transactional](http://twitter.com/Transactional)
    public void confirm(Order order) {
        order.setConfirmed(true);         
        this.entityManager.persist(person);
    } 
}
```

# 结论

*   尽管 ActiveRecord 违反了 SRP 并倾向于耦合组件，但它仍然是中小型应用程序的好选择。
*   ActiveRecord 只是一个工具。始终关注备选方案，并根据业务需求选择您需要的方案。
*   尽管存储库和实体更好😛
*   基于业务规则设计应用程序。高层重要，低层细节可以延后。