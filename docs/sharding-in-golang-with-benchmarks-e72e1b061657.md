# Golang 中的分片与基准

> 原文：<https://blog.devgenius.io/sharding-in-golang-with-benchmarks-e72e1b061657?source=collection_archive---------8----------------------->

## 关于数据库分片的快速回忆

分片数据库是一种机制，通过将数据库分成多个分区(类似于数据库分区机制)并将它们存储在几个实例或服务器中来加快查询或更新数据库的时间。

建议在应用它之前总是先从其他方法开始，因为它会增加你的数据库的复杂性，比如连接性、一致性和向后兼容性。

碎片可以是逻辑的也可以是物理的。我画了下图来描述这一点

逻辑碎片与物理碎片

[https://viewer.diagrams.net/?border=0&tags = % 7B % 7D&highlight = 0000 ff&edit = _ blank&layers = 1&nav = 1&title = Untitled % 20 diagram . draw io&open = Uhttps % 3A % 2F % 2f drive . Google . com % 2 fuc % 3 FID % 3 dcxwcbxd 0 aqft 5 gsx 5 waioobhxwwsj2j 36% 26 export % 3d 下载](https://viewer.diagrams.net/?border=0&tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Untitled%20Diagram.drawio&open=Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1CXWCBxd0aQfT5GSx5waiOBHxwWSj2J36%26export%3Ddownload)

由于其复杂性，我们可以依靠第三方来处理，它作为一个池运行，应用程序只需连接到它，选择碎片的决定可以由池处理。我们称之为动态分片。

另一种方法是我们可以自己实现分片，使用一些算法来决定每个请求应该到达哪个分片。我们称之为算法分片。

同样，请注意，我们需要确保无论我们使用什么方法，它都应该是一致的和向后兼容的，如果每次对于相同的请求，我们都使用不同的碎片，我们就无法得到想要的结果。为了向后兼容，当我们决定改变分片算法时，旧记录仍然可以以某种方式访问。

在这篇文章中，我将使用同样的分片思想来实现一个手工版本来形象化这个思想。

# 实现示例

实现应用于读写表单 map[key]值的算法分片，你可以把它看作一个键值数据库

**问题**:

我引用了 golang 博客中的一些建议:

> “Go 的并发原语——**Go routines 和 channels**——提供了一种优雅而独特的构造并发软件的方法。Go 鼓励使用通道在 goroutines 之间传递对数据的引用，而不是显式地使用锁来协调对共享数据的访问。[【ref】](https://go.dev/blog/codelab-share)

和

> ["地图并发使用不安全](https://go.dev/doc/faq#atomic_maps)没有定义当你同时读写它们时会发生什么。如果您需要从并发执行的 goroutines 中读取和写入映射，那么这些访问必须通过某种同步机制来协调。保护地图的一个常见方法是使用[同步。rw mutex](https://go.dev/pkg/sync/#RWMutex)[【ref】](https://go.dev/blog/maps)

事实上，并不总是可以使用 goroutine 和 channel，map 可能就是其中一种情况。

我们知道在并发中使用 map 时应该使用锁定机制。

这种方法非常有效，但是:

使用 RWMutex，只要没有写锁，任何进程都可以获取读锁，另一方面，只有当没有任何现有的提前读写锁时，才可能获取写锁，并且在任何提前锁被释放之前，获取额外锁的尝试将被阻止。

当处理该资源的并发进程数量急剧增加时，获取锁可能会成为瓶颈。

垂直分片将底层数据结构(map)分成多个可锁定的 map，有助于减少锁争用。

**想法**

分割 map 的机制应该是**一致的、向后兼容的和防冲突的。**

一种方法可以是我们采用密钥(校验和)输入的**散列，并根据碎片的数量进行模运算，以形成一个碎片密钥。对于每一个请求，我们将计算它并将数据存储到相应的 shard 中。**

```
hash := sha1.Sum([]byte(key))
shard_key := hash[17] % numberOfShards // 17 is just an arbitrary number
// Note that the size of checksum sha1 is 160 bits or 20 bytes, the arbitrary
// should be lower than 20
```

**实施**

```
// Define shard is a lockable map
type Shard struct {
	m map[string]interface{}
	sync.RWMutex
}

// ShardMap is a collection of shards, an abstraction for read/write data
type ShardMap []*Shard

func makeShardMap(size int) ShardMap {
	m := make([]*Shard, size)
	for i := 0; i < size; i++ {
		s := Shard{m: make(map[string]interface{})}
		m[i] = &s
	}
	return m
}

func (m ShardMap) getShardKey(key string) int {
	hash := sha1.Sum([]byte(key))
	return int(hash[17]) % len(m)
}

func (m ShardMap) GetShard(key string) *Shard {
	shard_key := m.getShardKey(key)
	return m[shard_key]
}

func (m ShardMap) Get(key string) (interface{}, bool) {
	shard := m.GetShard(key)
	shard.RLock()
	defer shard.RUnlock()
	v, ok := shard.m[key]
	return v, ok
}

func (m ShardMap) Set(key string, val interface{}) {
	shard := m.GetShard(key)
	shard.Lock()
	defer shard.Unlock()
	shard.m[key] = val
}

func (m ShardMap) Delete(key string) {
	shard := m.GetShard(key)
	shard.Lock()
	defer shard.Unlock()
	if _, ok := shard.m[key]; ok {
		delete(shard.m, key)
	}
}
```

test_shard.go

```
func TestGetSetShard(t *testing.T) {
	in := make(map[string]int)
	sm := makeShardMap(20)
	for i := 0; i < 1000; i += 1 {
		k := randomStr(10)
		v := randomInt(0, 1000000)
		in[k] = v
		fmt.Println("shard key: ", sm.getShardKey(k))
		sm.Set(k, v)
	}

	for k, v := range in {
		r, ok := sm.Get(k)
		require.Equal(t, true, ok)
		require.Equal(t, v, r)
	}
}

// Note that init() will always be called automatically before calling main()
func init() {
	rand.Seed(time.Now().UnixNano())
}
func randomStr(size int) string {
	//Random string of character in ASCII subrange ['A'...'z'] (including '/', '?'...)
	//Start index : 0
	//End index: 'z' - 'A'
	var str strings.Builder
	for size > 0 {
		idx := rand.Intn('z' - 'A')
		str.WriteRune(rune(idx + 'A'))
		size -= 1
	}
	return str.String()
}

func randomInt(min, max int) int {
	return min + rand.Intn(max-min) + 1
}
```

一切顺利，1000 把钥匙没有失败。但是它的性能如何呢？
让我们制定一些基准。

# **基准**

我们将编写一个简单的程序，将 4000 个键和每个键大约 4kB (4000 字节)的数据大小写入单个映射，而不是映射的碎片。我们将允许多达 8000 个 goroutines 并发读/写

首先，我们使用单一地图创建一个版本

```
var wg sync.WaitGroup

type SimpleMap struct {
	m map[string]interface{}
	sync.RWMutex
	sem chan int
	buf chan []byte
}

func makeSimpleMap() *SimpleMap {
	var sm SimpleMap
	sm = SimpleMap{
		m:   make(map[string]interface{}),
		sem: make(chan int, concurrentGoroutineLimit),
		buf: make(chan []byte, numberOfKeys),
	}
	return &sm
}

func (sm *SimpleMap) Write(k string) {
	sm.Lock()
	defer wg.Done()
	defer sm.Unlock()
	sm.sem <- 1
	sm.m[k] = randomBytes(dataSize)
	<-sm.sem
}

func (sm *SimpleMap) Read(k string) {
	sm.RLock()
	defer wg.Done()
	defer sm.RUnlock()
	sm.sem <- 1
	if v, ok := sm.m[k]; ok {
		sm.buf <- v.([]byte)
	} else {
		sm.buf <- make([]byte, dataSize)
	}
	<-sm.sem
}
```

一些助手随机生成密钥和索引。它需要时间。现在()。UnixNano 作为种子

```
func randomString(size int) string {
	return randomBuffer(size).String()
}

func randomBytes(size int) []byte {
	return randomBuffer(size).Bytes()
}

func randomBuffer(size int) *bytes.Buffer {
	var buf bytes.Buffer
	for size > 0 {
		idx := rand.Intn('z' - 'A')
		buf.WriteRune(rune(idx + 'A'))
		size -= 1
	}
	return &buf
}
```

简单映射的执行功能

```
func SimpleMapExecute(sm *SimpleMap) {
	var keys [numberOfKeys]string
	for i := 0; i < numberOfKeys; i += 1 {
		keys[i] = randomString(10)
	}
	start := time.Now()
	for i := 0; i < numberOfKeys; i += 1 {
		wg.Add(2)
		go func(key string) {
			sm.Write(key)
		}(keys[i])

		//Read from random key
		idx := rand.Intn(numberOfKeys)

		go func(key string) {
			sm.Read(key)
		}(keys[idx])
	}
	wg.Wait()
	elapsed := time.Since(start)
	fmt.Println("Took: ", elapsed)
}
```

地图碎片的执行功能

```
func ShardMapExecute(sm *ShardMap) {
	var keys [numberOfKeys]string
	for i := 0; i < numberOfKeys; i += 1 {
		keys[i] = randomString(10)
	}
	start := time.Now()
	for i := 0; i < numberOfKeys; i += 1 {
		wg.Add(2)
		data := randomBytes(dataSize)
		go func(key string) {
			defer wg.Done()
			sm.Set(key, data)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			defer wg.Done()
			if v, ok := sm.Get(key); ok {
				Buf <- v.([]byte)
			} else {
				Buf <- make([]byte, dataSize)
			}
		}(keys[rand.Intn(numberOfKeys)])
	}
	wg.Wait()
	elapsed := time.Since(start)
	fmt.Println("Took: ", elapsed)
}
```

准备运行参数

```
const (
	numberOfKeys             = 4000
	concurrentGoroutineLimit = 8000
	dataSize                 = 2000
)
```

最后，主要功能

```
func main() {
	simpleMode := flag.Bool("simple", false, "Run with simple map")
	flag.Parse()

	if *simpleMode {
		fmt.Println("Run with simple map")
		sm := makeSimpleMap()
		SimpleMapExecute(sm)
	} else {
		fmt.Println("Run with shard map")
		shardMap := makeShardMap(20)
		ShardMapExecute(&shardMap)
	}

}
```

让我们看看结果:

```
yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go -simple
Run with simple map
Took:  9.01160475s

yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go
Run with shard map
Took:  8.007320791s
```

哇，增强可以看到产卵😊

现在尝试一些其他测试，我将运行 12000 个并发 goroutines，其中 6000 个用于写，6000 个用于读。(注意，我注释掉了上面的信号量，删除了 8000 的限制)。键的数量将减少到 3000 个键。

稍微修改一下执行功能:

```
func SimpleMapExecute(sm *SimpleMap) {
	var keys [numberOfKeys]string
	for i := 0; i < numberOfKeys; i += 1 {
		keys[i] = randomString(10)
	}
	start := time.Now()
	for i := 0; i < numberOfKeys; i += 1 {
		wg.Add(4) //create 4 goroutines for each iteration
		go func(key string) {
			sm.Write(key)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			sm.Write(key)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			sm.Read(key)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			sm.Read(key)
		}(keys[rand.Intn(numberOfKeys)])
	}
	wg.Wait()
	elapsed := time.Since(start)
	fmt.Println("Took: ", elapsed)
}
```

和

```
func ShardMapExecute(sm *ShardMap) {
	var keys [numberOfKeys]string
	for i := 0; i < numberOfKeys; i += 1 {
		keys[i] = randomString(10)
	}
	start := time.Now()
	for i := 0; i < numberOfKeys; i += 1 {
		wg.Add(4)  //create 4 goroutines for each iteration
		data := randomBytes(dataSize)
		go func(key string) {
			defer wg.Done()
			sm.Set(key, data)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			defer wg.Done()
			sm.Set(key, data)
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			defer wg.Done()
			if v, ok := sm.Get(key); ok {
				Buf <- v.([]byte)
			} else {
				Buf <- make([]byte, dataSize)
			}
		}(keys[rand.Intn(numberOfKeys)])

		go func(key string) {
			defer wg.Done()
			if v, ok := sm.Get(key); ok {
				Buf <- v.([]byte)
			} else {
				Buf <- make([]byte, dataSize)
			}
		}(keys[rand.Intn(numberOfKeys)])
	}
	wg.Wait()
	elapsed := time.Since(start)
	fmt.Println("Took: ", elapsed)
}const (
	numberOfKeys             = 3000
	concurrentGoroutineLimit = 8000
	dataSize                 = 4000
)
```

和结果:

```
yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go -simple
Run with simple map
Took:  14.156822542s
yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go        
Run with shard map
Took:  5.989326334s
```

瞧！巨大的不同😀

再用 4000 把钥匙试试

```
yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go -simple
Run with simple map
Took:  19.437610875s
yenle@MacBook-Air-cua-Yen microservice_pattern % go run -race test2.go        
Run with shard map
Took:  8.30976475s
```

两倍多的速度。

好了，最后一句话，如果你读到这里，谢谢😄帖子到现在都挺长的。

使用分片也是非常有趣和有效的，它不仅适用于读取/更新地图，还适用于任何需要大量 goroutines 并发运行的任务，但缺点是可能会增加更多的复杂性，并可能以某种方式导致死锁，我们需要在使用前仔细测试。

希望这篇文章对你有所帮助。🙌

*最初发表于*[*【http://github.com】*](https://gist.github.com/Yenle9/6612281c69c86876d824b0374311222a)*。*