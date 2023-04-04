# 在 Dockerised Go 服务器中缓存 Instagram API 响应

> 原文：<https://blog.devgenius.io/caching-instagram-api-response-in-a-go-server-b67beaeaa1fd?source=collection_archive---------8----------------------->

![](img/7cc6f9d78e61eba9a0b4ad4b08387c4b.png)

**目录:**

[缓存结果](#13e6)∘[cache . go](#9aaa)∘[types . go](#7807)∘[fetch . go](#6fb3)t15】∘[service . go](#2825)t18】∘[。env](#db05)
∘[main . go](#1bb2)
[dockere 服务器](#e988)
∘[docker file](#23ad)
∘[docker-compose . yml](#0a2a)
t34】在 Firebase 上发布服务器

我一直面临着一个有趣的挑战:在 React 应用程序中以 3x3 的网格显示 Instagram 帖子。我在最后几篇文章中致力于这一努力。

在本系列的第一篇文章中，我讨论了在 React 应用程序中显示 Instagram 帖子的方法。

在[的第二篇文章](/improving-instagram-posts-in-react-cea3eec4dbaa)中，我概述了一种使用 Google Cloud 函数改进我的 Instagram Feed React 组件的方法，以便通过隐藏`access_token`和`user_id`来提高安全性。

我遇到的问题是，云函数会为每个请求获取相同的数据，我很快就会达到 API 速率限制(Instagram 基本显示 API 每小时约 200 个请求)。

基于前面的文章，我的**目标**是:

*   在 Go 服务器中缓存 Instagram API 响应，以避免超过 Instagram API 速率限制
*   停靠服务器
*   在 Firebase 上发布服务器

GitHub 上有[后端](https://github.com/fs1g17/http-server)和[前端](https://github.com/fs1g17/instafeed)代码。

# 缓存结果

在这一部分，我将回顾一下我在好朋友 Victor 的帮助下编写的 go 服务器。完整的代码可以在 GitHub 上公开获得。

后端代码保存在这个结构中:

```
http-server
├── cache
│   └── cache.go
├── instagram
│   ├── fetch.go
│   └── types.go
├── service
│   └── service.go
├── .env
└── main.go
```

这种结构提供了关注点的分离:`cache.go`仅用于提供缓存功能(以避免超过 Instagram API 的限制)，`fetch.go`用于执行获取，`service.go`启动 [echo](https://echo.labstack.com/) 服务器。

## cache.go

```
package cache

import (
 "time"
)

// Fetcher can fetch items that will be held by Cache.
type Fetcher[T any] interface {
 Fetch() (T, error)
}

// Cache contains cached items and keeps track of time.
type Cache[T any] struct {
 Fetcher[T]
 cache            T
 lastRefreshTime  time.Time
 expirationPeriod time.Duration
}

func New[T any](fetcher Fetcher[T], expirationPeriod time.Duration) *Cache[T] {
 return &Cache[T]{
  Fetcher: fetcher,
  // Cache is expired by default as there are no items in it.
  lastRefreshTime:  time.Now().Add(-2 * expirationPeriod),
  expirationPeriod: expirationPeriod,
 }
}

// Get cached item(s) that are refreshed if expired or returned as is.
func (c *Cache[T]) Get() (T, error) {
 err := c.refresh()
 return c.cache, err
}

// refresh expired cache if necessary.
func (c *Cache[T]) refresh() (err error) {
 if c.isFresh() {
  return
 }

 c.cache, err = c.Fetch()
 if err != nil {
  return
 }

 c.lastRefreshTime = time.Now()
 return
}

// isFresh tells us whether it's time to refresh our cache.
func (c *Cache[T]) isFresh() bool {
 return time.Now().Sub(c.lastRefreshTime) < c.expirationPeriod
}
```

构造函数将`lastRefreshTime`及时设置为 2 个`expirationPeriod`单位，这确保了`refresh`在第一次加载时发生。

`isFresh`函数检查自上次刷新以来是否经过了`expirationPeriod`时间量，如果经过的时间量小于`expirationPeriod`则返回`true`(即不需要刷新)，如果自上次刷新以来经过的时间大于`expirationPeriod`(即需要刷新)，则返回`false`。

## types.go

```
package instagram

type Feed struct {
 Data []struct {
  ID string `json:"id"`
 } `json:"data"`
 Paging struct {
  Cursors struct {
   Before string `json:"before"`
   After  string `json:"after"`
  } `json:"cursors"`
  Next string `json:"next"`
 } `json:"paging"`
}

type Item struct {
 MediaURL     string `json:"media_url"`
 Permalink    string `json:"permalink"`
 ThumbnailURL string `json:"thumbnail_url"`
 MediaType    string `json:"media_type"`
 ID           string `json:"id"`
}
```

`types.go`包含了`Feed`和`Item`的类型定义，它们是使用[这个 JSON to Go transformer 工具](https://transform.tools/json-to-go)从 Instagram API 响应中自动生成的。

`Feed`类型是通过查询这个 API 端点的 JSON 响应生成的:

`https://graph.instagram.com/{user_id}/media?access_token={access_token}`

`Item`类型是从查询该 API 端点的 JSON 响应中生成的(使用之前响应中的 Instagram post `id`):

`https://graph.instagram.com/{id}?access_token={access_token}&fields=media_url,permalink,thumbnail_url,media_type`

## 去拿吧

```
package instagram

import (
 "encoding/json"
 "fmt"
 "net/http"
)

type Fetcher struct {
 instaURL    string
 accessToken string
}

func NewFetcher(instaURL string, accessToken string) *Fetcher {
 return &Fetcher{
  instaURL:    instaURL,
  accessToken: accessToken,
 }
}

// Fetch gets the Feed by calling feed(), and for each Instagram post it 
// gets the Item by calling Item(id) and puts them all in a slice
func (f *Fetcher) Fetch() (items []*Item, err error) {
 feed, err := f.feed()
 if err != nil {
  return
 }

 for i := 0; i < 9 && i < len(feed.Data); i++ {
  item, err := f.item(feed.Data[i].ID)
  if err != nil {
   return nil, err
  }
  items = append(items, item)
 }

 return
}

// feed performs the GET request using instaURL
func (f *Fetcher) feed() (feed *Feed, err error) {
 resp, err := http.Get(f.instaURL)
 if err != nil {
  return nil, fmt.Errorf("error making HTTP request: %s", err)
 }

 feed = new(Feed)
 if err := json.NewDecoder(resp.Body).Decode(feed); err != nil {
  return nil, fmt.Errorf("error decoding insta feed: %s", err)
 }

 return
}

// item performs the GET request for a specific Instagram post given the id
func (f *Fetcher) item(id string) (instaItem *Item, err error) {
 mediaUrl := fmt.Sprintf(
  "https://graph.instagram.com/%s?access_token=%s&fields=media_url,permalink,thumbnail_url,media_type",
  id,
  f.accessToken,
 )

 resp, err := http.Get(mediaUrl)
 if err != nil {
  return nil, fmt.Errorf("error getting media item with id %s: %s", id, err)
 }

 instaItem = new(Item)
 if err := json.NewDecoder(resp.Body).Decode(instaItem); err != nil {
  return nil, fmt.Errorf("error decoding media item with id %s: %s", id, err)
 }

 return
}
```

`Fetch`函数执行提取操作。

`https://graph.instagram.com/{user_id}/media?access_token={access_token}`

`Fetch`调用`feed`函数，该函数查询上述端点并返回一个`Feed`。`Feed`包含`Data`:一片介质`id`。

在这些`id`的循环中，`Fetch`为每个`id`调用`item`函数，该函数返回一个`Item`。这个`Item`然后被加到`items`片上并返回。

## service.go

```
package service

import (
 "net/http"

 "github.com/fs1g17/http-server/instagram"
 "github.com/labstack/echo/v4"
 "github.com/labstack/echo/v4/middleware"
)

type Getter interface {
 Get() ([]*instagram.Item, error)
}

type Service struct {
 Getter
}

func New(getter Getter) *Service {
 return &Service{
  Getter: getter,
 }
}

// Server returns an echo server with CORS permissions and serves the items
func (s *Service) Server() *echo.Echo {
 e := echo.New()
 e.Use(
  middleware.Recover(),
  middleware.Logger(),
  middleware.CORS(),
 )
 e.GET("/", s.items)
 return e
}

// items gets the items using the Getter
func (s *Service) items(c echo.Context) (err error) {
 items, err := s.Get()
 if err != nil {
  c.Logger().Errorf("error getting instagram feed items: %s", err)
  return c.String(http.StatusInternalServerError, "can't fetch instagram items")
 }
 return c.JSON(http.StatusOK, items)
}
```

`Server`函数启动 echo 服务器并启用 CORS，服务于`items`函数以响应路径`/`的请求。

## 。包封/包围（动词 envelop 的简写）

`.env`文件包含了`access_token`和`user_id`，因此可以在`main.go`中访问它们:

```
ACCESS_TOKEN="your-access-token"
USER_ID="your-user-id"
```

## main.go

最后，`main.go`看起来是这样的:

```
package main

import (
 "fmt"
 "log"
 "os"
 "time"

 "github.com/fs1g17/http-server/cache"
 "github.com/fs1g17/http-server/instagram"
 "github.com/fs1g17/http-server/service"

 "github.com/joho/godotenv"
)

func main() {
 err := godotenv.Load(".env")

 if err != nil {
  log.Fatalf("Error loading .env file")
 }

 accessToken := os.Getenv("ACCESS_TOKEN")
 userId := os.Getenv("USER_ID")
 addr := ":9090"
 cacheExpirationTime := time.Hour

 instaURL := fmt.Sprintf("https://graph.instagram.com/%s/media?access_token=%s", userId, accessToken)
 fetcher := instagram.NewFetcher(instaURL, accessToken)
 cache := cache.New[[]*instagram.Item](fetcher, cacheExpirationTime)
 service := service.New(cache)
 if err := service.Server().Start(addr); err != nil {
  log.Fatalln(err)
 }
}
```

`main`函数从`.env`文件中检索`user_id`和`access_token`变量，并启动 echo 服务器。

# 停靠服务器

## Dockerfile 文件

docker 文件的灵感来自于 [Firebase 容器化文档](https://firebase.google.com/docs/hosting/cloud-run#containerize)

```
FROM golang:1.19 as builder
WORKDIR /app
COPY . .
RUN go mod download
RUN CGO_ENABLED=0 GOOS=linux go build -mod=readonly -v -o server

FROM alpine:3
RUN apk add --no-cache ca-certificates
WORKDIR /app
COPY .env .
COPY --from=builder /app/server .
EXPOSE 8080
CMD [ "/app/server" ]
```

## docker-compose.yml

```
version: "3"
services:
  gw:
    build: .
    image: gw
    container_name: gw
    ports:
      - 8080:8080
```

# 在 Firebase 上发布服务器

为了在 Firebase 上发布服务器，我首先创建了一个 Firebase 项目，然后按照[的说明提供动态内容](https://firebase.google.com/docs/hosting/cloud-run)。此时，剩下的唯一事情就是[将](https://github.com/fs1g17/instafeed/blob/c3bc6c262d20a9ec430ede18f1eebc349599338d/src/components/InstaFeed/InstaFeed.tsx#L16)[前端](https://github.com/fs1g17/instafeed/)中的 URL 更新为 Firebase 上发布的 Go 服务器的 URL。

# 结论

在本文中，我讨论了一种使用用 Go 编写的服务器缓存 Instagram API 结果的方法，这种服务器是容器化的，托管在 Firebase 上。

通过这种方式，`access_token`和`user_id`被隐藏起来，结果被缓存在云中，每小时重新获取一次，并且不会超过 Instagram API 的限制。

然而，这种方法的一个潜在问题是，`access_token`和`user_id`可能会改变(例如，如果`access_token`到期)。如果这些变量确实改变了，