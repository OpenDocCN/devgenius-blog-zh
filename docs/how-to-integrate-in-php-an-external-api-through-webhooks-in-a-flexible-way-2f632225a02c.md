# 如何灵活地通过 webhooks 在 PHP 中集成外部 API？

> 原文：<https://blog.devgenius.io/how-to-integrate-in-php-an-external-api-through-webhooks-in-a-flexible-way-2f632225a02c?source=collection_archive---------11----------------------->

在这篇文章中，我将与你分享一个 PHP 示例代码，我用它在一个 LARAVEL 项目中集成了 API webhooks。如果支付成功，API 会将 JSON 数据发送到包含支付交易详细信息的回调 URL。

我使用的 API 来自于 [Fastspring](https://fastspring.com) 支付网关。允许你自动管理数字产品的订购。

假设订阅创建成功的回调 URL 为:【https://website.com/webhooks/subscription-c】T2 已创建

接收的数据采用 JSON 格式，如下所示:

```
{  
 "id": "nEdzdi5eSoO43fbnXE8DfA",   "quote":"QUOT2J52LKCFCHPOYSW6UTRMNZJA",   
 "subscription": "nEdzdi5eSoO43fbnXE8DfA",  
 "active": true,  
 "state": "active",  
 "changed": 1585930046426,   "changedValue": 1585930046426,   "changedInSeconds": 1585930046,   "changedDisplay": "4/3/20",   "live": false,   "currency": "USD", 
  "account": "gB_slATyQBqSpAxA7-1YAg", 
  "product": "example-subscription-annual",
   "sku": null,   "display": "Example Subscription - Annual",   "quantity": 1,   "adhoc": false,   "autoRenew": true,   "price": 100,   "priceDisplay": "$100.00",   "priceInPayoutCurrency": 100,   "priceInPayoutCurrencyDisplay": "$100.00",   "discount": 0,   "discountDisplay": "$0.00",   "discountInPayoutCurrency": 0,   "discountInPayoutCurrencyDisplay": "$0.00",   "subtotal": 110,   "subtotalDisplay": "$110.00",   "subtotalInPayoutCurrency": 110,   "subtotalInPayoutCurrencyDisplay": "$110.00",   "next": 1617408000000,   "nextValue": 1617408000000,   "nextInSeconds": 1617408000,   "nextDisplay": "4/3/21",   "end": null,   "endValue": null,   "endInSeconds": null,   "endDisplay": null,   "canceledDate": null,   "canceledDateValue": null,   "canceledDateInSeconds": null,   "canceledDateDisplay": null,   "deactivationDate": null,   "deactivationDateValue": null,   "deactivationDateInSeconds": null,   "deactivationDateDisplay": null,   "sequence": 1,   "periods": null,   "remainingPeriods": null,   "begin": 1585872000000,   "beginValue": 1585872000000,   "beginInSeconds": 1585872000,   "beginDisplay": "4/3/20",   "intervalUnit": "year",   "intervalLength": 1,   "nextChargeCurrency": "USD",   "nextChargeDate": 1617408000000,   "nextChargeDateValue": 1617408000000,   "nextChargeDateInSeconds": 1617408000,   "nextChargeDateDisplay": "4/3/21",   "nextChargePreTax": 110,   "nextChargePreTaxDisplay": "$110.00",   "nextChargePreTaxInPayoutCurrency": 110,   "nextChargePreTaxInPayoutCurrencyDisplay": "$110.00",   "nextChargeTotal": 110,   "nextChargeTotalDisplay": "$110.00",   "nextChargeTotalInPayoutCurrency": 110,   "nextChargeTotalInPayoutCurrencyDisplay": "$110.00",   "nextNotificationType": "PAYMENT_REMINDER",   "nextNotificationDate": 1616803200000,   "nextNotificationDateValue": 1616803200000,   "nextNotificationDateInSeconds": 1616803200,   "nextNotificationDateDisplay": "3/27/21",   "paymentReminder": {     "intervalUnit": "week",     "intervalLength": 1   },   "paymentOverdue": {     "intervalUnit": "week",     "intervalLength": 1,     "total": 4,     "sent": 0   },   "cancellationSetting": {     "cancellation": "AFTER_LAST_NOTIFICATION",     "intervalUnit": "week",     "intervalLength": 1   },   "addons": [     {       "product": "example-product-1",       "sku": "skuex1",       "display": "Example Product 1",       "quantity": 1,       "price": 10,       "priceDisplay": "$10.00",       "priceInPayoutCurrency": 10,       "priceInPayoutCurrencyDisplay": "$10.00",       "discount": 0,       "discountDisplay": "$0.00",       "discountInPayoutCurrency": 0,       "discountInPayoutCurrencyDisplay": "$0.00",       "subtotal": 10,       "subtotalDisplay": "$10.00",       "subtotalInPayoutCurrency": 10,       "subtotalInPayoutCurrencyDisplay": "$10.00",       "discounts": []     }   ],   "setupFee": {     "price": {       "USD": 10     },     "title": {       "en": "One-time Setup Fee"     }   },   "fulfillments": {     "example-subscription-annual_file_1": [       {         "display": "example1.exe",         "size": 129,         "file": "https://yourexamplestore.onfastspring.com/account/file/YOU200403-5980-20157F",         "type": "file"       }     ]   },   "instructions": [     {       "product": "example-subscription-annual",       "type": "regular",       "periodStartDate": null,       "periodStartDateValue": null,       "periodStartDateInSeconds": null,       "periodStartDateDisplay": null,       "periodEndDate": null,       "periodEndDateValue": null,       "periodEndDateInSeconds": null,       "periodEndDateDisplay": null,       "intervalUnit": "year",       "intervalLength": 1,       "discountPercent": 0,       "discountPercentValue": 0,       "discountPercentDisplay": "0%",       "discountTotal": 0,       "discountTotalDisplay": "$0.00",       "discountTotalInPayoutCurrency": 0,       "discountTotalInPayoutCurrencyDisplay": "$0.00",       "unitDiscount": 0,       "unitDiscountDisplay": "$0.00",       "unitDiscountInPayoutCurrency": 0,       "unitDiscountInPayoutCurrencyDisplay": "$0.00",       "price": 100,       "priceDisplay": "$100.00",       "priceInPayoutCurrency": 100,       "priceInPayoutCurrencyDisplay": "$100.00",       "priceTotal": 100,       "priceTotalDisplay": "$100.00",       "priceTotalInPayoutCurrency": 100,       "priceTotalInPayoutCurrencyDisplay": "$100.00",       "unitPrice": 100,       "unitPriceDisplay": "$100.00",       "unitPriceInPayoutCurrency": 100,       "unitPriceInPayoutCurrencyDisplay": "$100.00",       "total": 100,       "totalDisplay": "$100.00",       "totalInPayoutCurrency": 100,       "totalInPayoutCurrencyDisplay": "$100.00"     }   ] }
```

如您所见，为了将相关数据存储在数据库中，需要解析大量 json 数据。最常见的方法是在 LARAVEL 控制器中使用类似以下代码的代码:

```
public function subscription_created(Request $request){$array_data=json_decode($reqyest->json_data,true);
$relevant_data["subscription"]=$array_data["subscription"];
$relevant_data["product"]=$array_data["items"][0]["product"];
$relevant_data["price"]=$array_data["items"][0]["price"];/** insert relevant_data in mysql database **/
}
```

对于另一个挂钩，例如订阅更新，另一个动作被添加到控制器等等。

```
public function subscription_created(Request $request){$array_data=json_decode($request->json_data,true);
$relevant_data["subscription"]=$array_data["subscription"];
$relevant_data["product"]=$array_data["items"][0]["product"];
$relevant_data["price"]=$array_data["items"][0]["price"];/** insert relevant_data in mysql database **/
}public function subscription_renew(Request $request){$array_data=json_decode($request->json_data,true);
$relevant_data["subscription"]=$array_data["subscription"];
$relevant_data["product"]=$array_data["items"][0]["product"];
$relevant_data["next"]=$array_data["schedule"]["next"];/** insert relevant_data in mysql database **/
}
```

假设 API 响应中的一个字段更改了名称或顺序。例如**订阅**字段变成**订阅 _id。**在这种情况下，您应该在所有控制器代码中搜索该字段，并更改字段名。此外，这些代码是多余的，难以维护。

让我们首先尝试创建一个名为***HookSubscriptionData***的类，它包含常见的 API 响应字段。

```
**class** HookSubscriptionData {**public** $subscription;
**public** $product;
public $next;
public $price;**public** **function** **__construct**(Array $params){
$this->subscription=$params['subscription']??"";
$this->next=$params["nextInSeconds"]??"";
$this->product=$params["items"][0]["product"]??"";
$this->price=$params["items"][0]["price"]??0;
}
}
```

和 ***JSONParser*** 类，解析传入的 JSON 数据并将其转换为数组。

```
**class** JSONParser {
**public** **function** parse($json_data) {
$result= json_decode($json_data,**true**);
**if**(!is_array($result)){
**throw** **new** \Exception("Not valid Json Data");
}
**return** $result;
}
}
```

最后处理订阅激活钩子的类叫做***HookSubscriptionActivated。***

```
**class** HookSubscriptionActivated{**public** $hook_data;**public** **function** **__construct**(HookSubscriptionConfig $hook_config)
{
$this->hook_data=$hook_config;
}**public** **function** handle(Callable $callback){
 **if**($callback!=**null**){
  $callback($this->hook_data);
  }
}
}
```

现在，在 controller 中，情况发生了变化。

```
public function subscription_created(Request $request){
**try**{
 $hook=**new** HookSubscriptionActivated(**new** hookSubscriptionData((new JSONParser())->parse($request->json_data)));
 $hook->handle(**array**($this,"f_activated"));
}**catch**(\Exception $ex){}
}**public** **function** f_activated($hook_data){ 
/** store data in databse **/
}public function subscription_renew(Request $request){
**try**{
$hook=**new** HookSubscriptionRenew(**new** hookSubscriptionData((new JSONParser())->parse($request->json_data)));
$hook->handle(**function($hook_data){** /** store data in databse **/ **}**);
}**catch**(\Exception $ex){}
}
```

在这种情况下，如果一个字段名改变，它不会影响所有的代码。更改将在单个类 ***中完成。***

```
**public** **function** **__construct**(Array $params){
**$this->subscription=$params['subscription_id']??"";**
$this->next=$params["nextInSeconds"]??"";
$this->product=$params["items"][0]["product"]??"";
$this->price=$params["items"][0]["price"]??0;
}
}
```