# 在 Spring boot 中利用缓存和并行化提高服务性能

> 原文：<https://blog.devgenius.io/improving-service-performance-by-utilizing-caching-and-parallelization-in-spring-boot-b154b0b3ba5e?source=collection_archive---------3----------------------->

假设我们必须创建一个服务方法，它接受一个搜索关键字列表并返回一个对象列表。如果我们开始对每个搜索关键字进行循环数据库调用，然后聚合结果作为响应发送出去。无论您的数据库有多高效，从性能的角度来看，这显然是非常糟糕的。因此，在本文中，我们将探讨如何利用 Spring boot 的并行化和缓存特性来提高性能。

![](img/9336cdf30edffb68be5d26babeb3c3a0.png)

照片由[马文·迈耶](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 解决方案概述

因为我们在这里处理一个搜索关键字列表，所以很明显我们想到的第一件事是分批划分列表，然后进行并行数据库调用以获得响应。

但这还不够，我们可以进一步改进。在创建批处理和进行数据库调用之前，我们可以从在缓存中查找搜索关键字开始，如果存在，那么我们可以从列表中跳过这些搜索关键字，只对其他批处理进行异步数据库调用。一旦我们从数据库中获得响应，我们就可以缓存它并将其聚合到一个列表中，然后作为响应发送出去。这样，我们将利用 Spring boot 的并行化和缓存特性来提高性能。

# 让我们直接进入代码

在这个例子中。我们正在创建一个服务，该服务将获得一个`searchKeys`列表，并返回`searchKey`和`movie`细节的地图。

这个想法很简单。

1.  首先，我们将获得不同的搜索关键字。
2.  然后，我们将尝试查找缓存中是否有这些键，如果有，我们将标记这些键，并将电影细节添加到列表中。
3.  然后，我们将移除在上一步中标记的键。
4.  之后，我们将创建特定批量的批次。
5.  然后我们将以异步方式为每个批处理调用助手服务。
6.  一旦我们获得了数据，我们就可以设置缓存并将其聚集在一个列表中。
7.  最后我们会归还地图。

注意:这只是服务端的代码。

```
private Map<String, Movie> getMovies(List<String> searchKeys) { List<String> uniqueSearchKeys = searchKeys.
      stream().
      distinct().collect(Collectors.toList()); List<Movie> movies = new ArrayList<>(); // First, we will get the movies from cache if it is there.
    // To use the jdkTemplate, you have to Autowire it like this.
    // @Autowired
    // @Qualifier("customJDKRedisTemplate")
    // RedisTemplate<String, Object> jdkTemplate; List<String> foundInCache = new ArrayList<>(); for (String uniqueKey: uniqueSearchKeys) {
      Movie movie = (Movie)jdkTemplate.opsForValue().
        get("getMovieDetails" + "::" + uniqueKey.toUpperCase());
      if (null != movie) {
        foundInCache.add(uniqueKey);
        movies.add(movie);
      }
    } // Remove the keys which we have already found
    uniqueSearchKeys.removeAll(foundInCache); // For the rest of the keys we will divide it 
    // in batches of  {BATCH_SIZE} and 
    // then make Database call to get it. int numberOfBatch = (uniqueSearchKeys.size() / BATCH_SIZE);
    numberOfBatch = (numberOfBatch * BATCH_SIZE) 
      == uniqueSearchKeys.size() ? numberOfBatch : numberOfBatch+1; List<CompletableFuture<List<Movie>>> taskList = 
      new ArrayList<>(); for (int i = 0; i < numberOfBatch; i++) {
      // here service is an helper service which 
      // calls the DAO layer to get the data in Async
      taskList.add(
        service.getMovieDetails(
          uniqueSearchKeys.subList(i*BATCH_SIZE,
          uniqueSearchKeys.size() < (i+1)*BATCH_SIZE ? 
          uniqueSearchKeys.size() : (i+1)*BATCH_SIZE )
      ));
    } for (CompletableFuture<List<Movie>> task: taskList) {
      try {
        // You can add a timeout if you want here
        List<Movie> moviesFromDB = task.get();
        // We will set the cache for the searchKey 
        // which we are getting from the DB
        for (Movie movie: moviesFromDB) {
          jdkTemplate.opsForValue().
          set("getMovieDetails" + "::" +
               movie.getSearchKey().toUpperCase(),
             movie);
          movies.add(movie);
        }
      } catch (InterruptedException | ExecutionException e) { 
        log.error("Error while getting movie deatils {}", e);
      }
    }

    // We will return the map of search key and movie details
    return movies.stream().
      filter(
        movie -> null != movie && null != movie.getSearchKey()
      ).
      collect(
        Collectors.toMap(
          Movie::getSearchKey,
          movie -> movie,  (existingValue, newValue) -> newValue)
      );
}
```

要使用自定义的`RedisTemplate`，您需要将其定义为一个 Bean。您可以在任何配置文件中定义它。你可以这样定义它。

```
/**
 * This Bean is used for caching Java object
 * @return redis template
 */
[@Bean](http://twitter.com/Bean)(name="customJDKRedisTemplate")
public RedisTemplate<String, Object> getCustomJDKRedisTemplate() {
 RedisTemplate<String, Object> template = new RedisTemplate<>();
 template.setConnectionFactory(lettuceConnectionFactory());
 template.setKeySerializer(new StringRedisSerializer());
 template.setValueSerializer(new JdkSerializationRedisSerializer());
 return template;
}
```

如果你想了解更多关于异步和缓存的知识，你可以浏览 Spring boot 的文档。

1.  [https://docs . spring . io/spring-data/redis/docs/current/API/org/spring framework/data/redis/core/redis template . html](https://docs.spring.io/spring-data/redis/docs/current/api/org/springframework/data/redis/core/RedisTemplate.html)
2.  https://spring.io/guides/gs/async-method/