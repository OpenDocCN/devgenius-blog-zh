# 在 Spring Boot 使用 Redis 缓存实现限速器

> 原文：<https://blog.devgenius.io/implementing-rate-limiter-using-redis-cache-in-spring-boot-1ddd0a9cf8da?source=collection_archive---------1----------------------->

在 Bot 攻击的时代，几乎所有主要科技公司发布的 API 都实施了某种速率限制。在本文中，我将介绍在 Spring boot 中基于固定窗口计数器算法使用 Redis 缓存实现速率限制器。

![](img/755d04952edd35e032ef8faea0b2370b.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[哈里·格劳特](https://unsplash.com/@photographybyharry?utm_source=medium&utm_medium=referral)拍摄

# 什么是速率限制？

> 速率限制是一种限制网络流量的策略。它对某人在一定时间内重复一个动作的频率设置了上限。

# 使用限速器有什么好处？

*   **防止拒绝服务(DoS)攻击引起的资源饥饿**。
*   **降低成本**。限制过多的请求意味着减少服务器数量，将更多的资源分配给高优先级 API。
*   **防止服务器过载**。

# 设计和算法

## 设计

有两种实现方式可供我们选择:

1.  **客户端**
2.  **服务器端**

客户端有点不可靠，因为机器人可以很容易地找到一些伪造请求的方法，或者他们可以直接攻击服务 API 并伤害我们。

在本文中，我们将从**服务器端实现**开始。

即使在**服务器端实现**中，也有两种方法可供我们选择:

1.  **作为 API 网关内部的中间件**
2.  **在我们的控制器端点的应用程序内部**

在我看来，两者都很好。但是，如果您没有 API 网关或有第三方 API 网关(在这种情况下，您没有太多的自由)，您可以使用第二个选项，并可以自由定制速率限制器。

在本文中，我们将采用第二种方法。

## 算法

有各种速率限制算法。

1.  **令牌桶**
2.  **漏桶**
3.  **固定窗口计数器**
4.  **滑动窗口日志**
5.  **滑动窗口计数器**

在本文中，我们将使用**固定窗口计数器。**

先来了解一下**固定窗口计数器**是如何工作的。

*   对于每个时间线，我们创建计数器并用零初始化它。
*   每次请求后，我们将计数器加 1。
*   一旦计数器达到预定义的阈值，我们就可以开始抛出异常，并向客户端发送 429 Http 状态代码。

使用**固定窗口计数器的利弊:**

## 赞成的意见

*   **内存高效**。因为我们只是将计数存储为值，将用户 id 存储为键
*   **通俗易懂**。
*   **在间隔结束时重置可用配额**符合特定用例。

## 骗局

*   **窗口边缘的流量峰值**可能导致超过允许限制的更多请求通过。

# 履行

现在让我们深入研究一下实现。

我们可以使用基于 **IP 地址**的计数器或基于**用户**的计数器。如果你的应用程序是安全的，那么我一定会推荐基于**用户**的计数器。对机器人来说，不断改变 IP 地址比每次注册和创建新用户更容易。

首先，我们将创建**服务**来**获取并递增**用户的 **API 命中计数。**

```
@Slf4j
@Service
public class RateLimiterService {@Autowired
    RedisTemplate<String, Object> template; /**
     * This method is to return the current number 
     * of api calls made by this user from cache
     * @param userId - user id
     * @return String - number of calls made by this user
     */
    @Cacheable(cacheNames = "apiHitCount", key ="{#userId}")
    public String getApiHitCount(Integer userId) {
        return "0";
    } /**
     * This method is to increment the number of api 
     * calls made by this user in cache
     * @param userId - user id
     * @return void
     */
    public void incrementApiHitCount(Integer userId) {
        template.
            opsForValue().
            increment("apiHitCount" + "::" + userId);
    }
}
```

现在让我们将 **TTL(生存时间)**设置为 1 小时，这将是我们的时间线。

这可以在**缓存配置**中进行配置。我将展示如何为一个特定的缓存键设置 TTL。

我们必须为**重新分配另一个 **Bean** 。**

```
@Bean
public CacheManager cacheManager(
    RedisConnectionFactory connectionFactory
) {
     RedisCacheConfiguration defaultCacheConfig =
       RedisCacheConfiguration.defaultCacheConfig();
     defaultCacheConfig.disableCachingNullValues();
     Map<String, RedisCacheConfiguration> cacheConfigurations = 
       new HashMap<>();

    // This is how, you can set the TTL for a particular cache key.
    // Here as you can see, we have set it as 1 hour.
    // After one hour it will be cleared from the cache.
    cacheConfigurations.put(
      "apiHitCount", defaultCacheConfig.
      entryTtl(Duration.ofHours(1)).
      serializeValuesWith(
        SerializationPair.fromSerializer(RedisSerializer.string())
      )
    );
    return RedisCacheManager.builder(connectionFactory).
      cacheDefaults(defaultCacheConfig).
      withInitialCacheConfigurations(cacheConfigurations).
      build();
}@Bean
public RedisTemplate<String, Object> redisTemplate() {
   RedisTemplate<String, Object> template = new RedisTemplate<>();
   template.setConnectionFactory(lettuceConnectionFactory());
   template.setDefaultSerializer(new StringRedisSerializer());
   return template;
}
```

现在让我们看看**控制器**的逻辑。

```
@Slf4j
@RestController
@RequestMapping("/api")
public class DemoController { @Autowired
    RateLimiterService rateLimitedService;

    @Autowired
    DemoService demoService;

    // We can get it from properties or, 
    // we can get it from config table (
    // for this we will need to have a DAO call 
    // to get the limit)
    @Value("${api.count.limit:100}")
    private int apiCountLimit;

    /**
     * GET endpoint to get bank balance of a particular user
     * @param user - authenticated user
     * @param request - http servlet request
     * @return double - bank balance
     */
    @GetMapping("/get-details")
    public double getDetails(
      @AuthenticationPrincipal AvlUser user
      HttpServletRequest request
    ){ if (null != user && null != user.getUsername())) {
        // if the number of calls exceed the configured value, 
        // then we throw Too many calls exception.
        if (
           Integer.parseInt(
           rateLimitedService.getApiHitCount(user.getUserId())) 
           > apiCountLimit
        ){
          // we can log the user name
          // such that later we can go through the logs
          // and ban the malicious users
          log.info(
            "User {}, id {} has exceeded configured 
            per day api limit.", 
            user.getUsername(), 
            user.getUserId()
          );
          // we can catch this exception in exception handler to 
          // send 429 http status code
          throw new TooManyCallsException();
        }
        // incrementing the count
        rateLimitedService.incrementApiHitCount(
          user.getUserId()
        );
        return demoService.getDetails(user);
     } else {
        throw new PermissionException();
     }
   }
}
```

*   如果你想用基于 **IP 地址的**计数器。可以使用`request.getRemoteAddr()`(其中请求为`HttpServletRequest`)。
*   你可以看看这篇文章，我解释了如何使用 `**@ControllerAdvice**`来**捕捉异常并发送任何你想要的 Http 状态代码。**

对于上面的例子。

1.  我们可以发送[**Http 状态码 429**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) 为`**TooManyCallsException**`。
2.  和 [**Http 状态码 401**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401) 为`**PermissionException**`。

## 注意

之前我使用了`@CachePut`来设置 API 命中数。在控制器中，我首先获取 API 命中数，然后加 1。但是正如@Vovabear 所指出的，当多个线程开始触及端点时，这可能会导致问题。

> 让我们假设有两个线程 A 和 B，它们都有相同的 userId，当计数为 0 时，它们都同时到达了我们的端点。理想情况下，我们的计数应该是 2。但是由于两者都将计数读取为 0，所以它会尝试将其更新为 1，这是不正确的。

我做的解决这个用的是`RedisTemplate` `opsForValue().increment`。现在 Redis 将处理并发线程，并且一次只允许一个线程更新值。

# 结论

现在我们知道**什么是限速器**、**为什么有利**、**什么是所有的方法和算法都有**。然后我们**通过选择将速率限制器**放在我们的应用程序**中，然后使用**固定窗口计数器算法，实现了**一个**服务器端速率限制器**。**

这将取决于哪种方法更适合您的系统，然后您可以相应地实现。

在这里，我们只是研究了事情的**速率限制方面**。但是如果你也想做**僵尸管理**，那么你必须**整体检测恶意活动**，然后给他们**警告或者封锁账户**。您可以利用**限速部件**跟踪**异常活动**。同样在上述实施中，如果您注意到我们正在记录即将达到限制的用户，这样我们就可以跟踪所有此类用户，如果我们看到特定用户的任何模式或一贯的异常活动，我们就可以开始阻止这些帐户。

如果你想了解更多关于这个主题和其他系统设计概念，你应该浏览这本书。

1.  [https://www . Amazon . com/System-Design-Interview-Insiders-Guide-ebook/DP/b 08 B3 fwybx](https://www.amazon.com/System-Design-Interview-Insiders-Guide-ebook/dp/B08B3FWYBX)