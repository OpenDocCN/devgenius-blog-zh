# Golang 中更好的单元测试的 5 个技巧

> 原文：<https://blog.devgenius.io/5-tips-for-better-unit-testing-in-golang-b25f9e79885a?source=collection_archive---------1----------------------->

![](img/07d1642359c30c343b532b60276dd5dd.png)

拉塞尔·维斯特布鲁克，本·西蒙斯和地鼠不练习他们的投篮(测试)，因此他们的投篮命中率(覆盖率)很低，导致砖块(虫子)扔向篮筐(项目)。

在编写代码项目时，开发人员通常会花费大量时间来选择合适的框架、库、数据库和其他第三方组件。一个经常被忽视的部分是测试。

正确的测试实际上会使您的项目更好，因为它们鼓励您:

1.  应用干净的代码:编写简短的函数，每个函数处理一个任务，等等。
2.  通过使用抽象、接口和模拟来编写可扩展和不可知的代码。
3.  通过测试常规/边缘案例和这些案例的高覆盖率，更好地理解业务逻辑。
4.  避免遗留的、长期未接触的和/或不可维护的代码——测试将简化维护和验证代码变更的过程，使其不会腐烂。
5.  通过基准测试、负载测试等来衡量代码的性能。

虽然测试显然不是由 Golang 发明或发起的，但该语言提供了丰富的标准库、约定和外部库，缩短了您通向经过适当测试的项目的道路。

# 表格驱动测试

当您想要测试的功能处理太多任务时，尤其是当您想要测试许多不同的情况时，测试会很快变得不可读、重复且令人讨厌。

让我们看看下面的代码，它天真地检查了一个 NBA 球员的比赛是好是坏:

```
package main

import (
   "fmt"
)

type Stats struct {
   Name string
   Minutes float32
   Points int8
   Rebounds int8
   Assists int8
   Turnovers int8
}

func main() {
   s := Stats{Name: "Stef Kuri", Minutes: 25.1, Points: 21, Assists: 3, Turnovers: 7, Rebounds: 8}
   fmt.Println(hadAGoodGame(s))
}

func hadAGoodGame(stats Stats) (bool, error) {
   if stats.Assists < 0 || stats.Points < 0 || stats.Rebounds < 0 || stats.Minutes < 0 || stats.Turnovers < 0 {
      return false, fmt.Errorf("stat lines cannot be negative")
   }
   if stats.Name == "" {
      return false, fmt.Errorf("missing player name")
   }
   if stats.Assists >= (stats.Turnovers * 2) {
      return true, nil
   }
   if stats.Assists >= 10 && stats.Rebounds >= 10 && stats.Points >= 10 {
      return true, nil
   } else if stats.Points < 10 && stats.Assists < 10 && stats.Minutes > 25.0 {
      return false, nil
   }
   return false, nil
}
```

显然，我们有许多案例要测试，让我们从两个案例开始:

```
package main

import (
   "github.com/stretchr/testify/assert"
   "github.com/stretchr/testify/require"
   "testing"
)

func TestHadAGoodGame(t *testing.T) {
   t.Run("sad path: invalid stats", func(t *testing.T) {
      s := Stats{Name: "Sam Cassell",
         Minutes: 34.1,
         Points: -19,
         Assists: 8,
         Turnovers: -4,
         Rebounds: 11,
      }
      _, err := hadAGoodGame(s)
      require.NotNil(t, err)
   })

   t.Run("happy path: good game", func(t *testing.T) {
      s := Stats{Name: "Dejounte Murray",
         Minutes: 34.1,
         Points: 19,
         Assists: 8,
         Turnovers: 4,
         Rebounds: 11,
      }
      isAGoodGame, err := hadAGoodGame(s)
      require.Nil(t, err)
      assert.True(t, isAGoodGame)
   })
}
```

你可以看到它的走向:当我们想要测试 4 个案例时会发生什么？5?我们开始一遍又一遍地重复吗？号码

有一种很酷的替代方法叫做**表格驱动测试** :
表格驱动测试允许你创建紧凑的、可读的测试，并使其更容易覆盖所有情况，因为事实上所有的测试数据都是有组织的和简洁的。

```
package main

import (
   "github.com/stretchr/testify/assert"
   "testing"
)

func TestHadAGoodGame(t *testing.T) {
   tests := []struct {
      name     string
      stats   Stats
      goodGame bool
      wantErr  string
   }{
      {"sad path: invalid stats", Stats{Name: "Sam Cassell",
         Minutes: 34.1,
         Points: -19,
         Assists: 8,
         Turnovers: -4,
         Rebounds: 11,
         }, false, "stat lines cannot be negative",
      },
      {"happy path: good game", Stats{Name: "Dejounte Murray",
         Minutes: 34.1,
         Points: 19,
         Assists: 8,
         Turnovers: 4,
         Rebounds: 11,
      }, true, ""},
   }
   for _, tt := range tests {
      isAGoodGame, err := hadAGoodGame(tt.stats)
      if tt.wantErr != "" {
         assert.Contains(t, err.Error(), tt.wantErr)
      } else {
         assert.Equal(t, tt.goodGame, isAGoodGame)
      }
   }
}
```

# 测试套

有时候你的测试可能需要你初始化一个上下文，声明一个变量，或者在运行测试之前做一些设置，或者在测试完成后进行拆卸。

使用测试套件是实现这一点的便捷方式。
Golang 有一个叫做[套房](http://godoc.org/github.com/stretchr/testify/suite)的不错的套餐可以帮你到达那里。
(您可能已经注意到，整个[evidence](https://github.com/stretchr/testify)存储库包含了帮助您创建质量测试的工具。)

在合并了 evidence 的套件之后，上一节中的测试将如下所示:

```
package main

import (
   "fmt"
   "github.com/stretchr/testify/suite"
   "testing"
)

type GameTestSuite struct {
   suite.Suite
}

func (suite *GameTestSuite) BeforeTest(_, _ string) {
   // execute code before test starts
   fmt.Println("BEFORE")
}

func (suite *GameTestSuite) AfterTest(_, _ string) {
   // execute code after test finishes
   fmt.Println("AFTER")
}

func (suite *GameTestSuite) SetupSuite() {
   // create relevant resources
   fmt.Println("SETUP")
}

func TestGameTestSuite(t *testing.T) {
   suite.Run(t, new(GameTestSuite))
}

func (suite *GameTestSuite) TestHadAGoodGame() {
   tests := []struct {
      name     string
      stats    Stats
      goodGame bool
      wantErr  string
   }{
      {"sad path: invalid stats", Stats{Name: "Sam Cassell",
         Minutes: 34.1,
         Points: -19,
         Assists: 8,
         Turnovers: -4,
         Rebounds: 11,
      }, false, "stat lines cannot be negative",
      },
      {"happy path: good game", Stats{Name: "Dejounte Murray",
         Minutes: 34.1,
         Points: 19,
         Assists: 8,
         Turnovers: 4,
         Rebounds: 11,
      }, true, ""},
   }
   for _, tt := range tests {
      suite.T().Run("test hadAGoodGame(): "+tt.name, func(t *testing.T) {
         isAGoodGame, err := hadAGoodGame(tt.stats)
         if tt.wantErr != "" {
            suite.Require().Contains(err.Error(), tt.wantErr)
         } else {
            suite.Require().Equal(tt.goodGame, isAGoodGame)
         }
      })
   }
}
```

# 使用接口并避免文件 I/O

现在我们有了一个从文件中读取球员数据并打印出来的函数:

```
package main

import (
   "encoding/json"
   "fmt"
   "io/ioutil"
   "os"
)

type Player struct {
   Name   string `json:"name"`
   Age    int    `json:"Age"`
}

func main() {
   processData("data.txt")
}

func processData(file string) error {
   f, err := os.Open(file)
   if err != nil {
      return err
   }
   defer f.Close()
   data, err := ioutil.ReadAll(f)
   if err != nil {
      return err
   }
   var players []Player

   err = json.Unmarshal(data, &players)
   if err != nil {
      return err
   }

   for _, p := range players {
      fmt.Println("Player name: ", p.Name)
   }
   return nil
}
```

有什么不好？

1.  不容易测试，因为在运行测试之前，您需要确保`data.txt`文件存在并且包含数据
2.  如果有大量数据，而执行测试的服务器有一些较低级别的硬件，该怎么办？
3.  您也不希望遇到并发测试使用/修改同一个文件的竞争情况

您希望您的测试快速、独立、孤立、一致而不古怪。

更好的方法是这样的:

```
package main

import (
   "encoding/json"
   "fmt"
   "io/ioutil"
   "os"
   "io"
)

type Player struct {
   Name   string `json:"name"`
   Age    int    `json:"age"`
}

func main() {
   processData("data.txt")
}

func processData(file string) error {
   f, err := os.Open(file)
   if err != nil {
      return err
   }
   defer f.Close()
   return unmarshalAndPrint(f)
}

func unmarshalAndPrint(f io.Reader) error {
   data, err := ioutil.ReadAll(f)
   if err != nil {
      return err
   }
   var players []Player

   err = json.Unmarshal(data, &players)
   if err != nil {
      return err
   }

   for _, p := range players {
      fmt.Println("Player name: ", p.Name)
   }
   return nil
}
```

测试文件:

```
package main

import (
   "github.com/stretchr/testify/assert"
   "strings"
   "testing"
)

func TestUnmarshalAndPrint(t *testing.T) {
   t.Run("testing unmarshalAndPrint()", func(t *testing.T) {
      err := unmarshalAndPrint(strings.NewReader(`[{"name": "Dubi Gal", "age": 900}]`))
      assert.Nil(t, err)
   })
}
```

现在为了测试，我们不用准备数据和打开文件，只需将一个 JSON 字符串传递给`strings.NewReader`

但是，如果出于某种原因，您仍然必须与文件系统进行交互，请考虑使用以下资源:
[AFERO](https://github.com/spf13/afero) :用于 Go
[fstest](https://godocs.io/testing/fstest) —测试文件系统的实现和用户
[TempFile](https://pkg.go.dev/io/ioutil#TempFile) ， [TempDir](https://pkg.go.dev/io/ioutil#TempDir) —创建临时文件/目录

# 使用 httptest

与上面的相似，但是这一个值得它自己的一个部分。

假设您有一个接收 NBA 球员数据、验证数据并通过 HTTP 请求进行保存或进一步处理的函数:

```
package main

import (
   "bytes"
   "fmt"
   "net/http"
   "encoding/json"
)

func main() {
   playerInfo := PlayerInfo{Name: "White Mambda", Team: "San Antonio Spurs", Position: "Forward"}
   err := savePlayerInfo(playerInfo, "http://players.nba.com")
   if err != nil {
      panic(err)
   }
}

type PlayerInfo struct {
   Name    string
   Team    string
   Position string
}

func savePlayerInfo(playerInfo PlayerInfo, url string) error {
   if playerInfo.Name == "" || playerInfo.Position == "" || playerInfo.Team == "" {
      return fmt.Errorf("missing data")
   }
   b, err := json.Marshal(playerInfo)
   if err != nil {
      return err
   }
   body := bytes.NewBuffer(b)
   req, err := http.NewRequest("POST", url, body)
   req.Header.Set("Content-Type", "application/json")
   client := &http.Client{}
   _, err = client.Do(req)
   return err
}
```

你如何测试这个函数？

嗯，您可能已经发现自己正在测试一个发出 HTTP 请求的函数。通常，请求本身并不是您想要测试的部分，而是其他东西，比如输入/输出是如何被处理/操作的..
此外，您不希望您的测试依赖于第三方，第三方可能会限制或限制您的测试，这可能会使您的测试不可靠或影响您的测试结果。

[httptest](https://pkg.go.dev/net/http/httptest) 包提供了用于 HTTP 测试的实用程序，并将轻松地处理您的测试的 HTTP 请求。

使用 httptest，您的测试看起来是这样的:

```
package main

import (
   "github.com/stretchr/testify/require"
   "net/http"
   "net/http/httptest"
   "testing"
)

func TestSavePlayerInfo(t *testing.T) {
   testServer := httptest.NewServer(http.HandlerFunc(func(res http.ResponseWriter, req *http.Request) {
      res.Header().Set("Content-Type", "application/json")
      res.WriteHeader(http.*StatusOK*)
      _, err := res.Write([]byte(`{"message": "success"`))
      if err != nil {
         panic("cannot return http response")
      }
   }))
   defer testServer.Close()

   t.Run("sad path: invalid stats", func(t *testing.T) {
      s := PlayerInfo{Name: "Denis Rodman", Team: "Chicago Bulls", Position: "Forward"}
      err := savePlayerInfo(s, testServer.URL)
      require.Nil(t, err)
   })
}
```

您可以控制响应、标题、状态代码和基本上您需要的一切。

# 基准测试您的代码

Golang 的测试包中集成了基准测试，可以帮助您加快代码执行速度，避免代价高昂的性能损失。

有没有想过解组 JSON 最有效的方法是什么？

```
package main

import (
   "encoding/json"
   "io"
   "io/ioutil"
)
/*
func main() {
   _, err := jsonDecoder(strings.NewReader(`[{"name": "Dubi Gal", "age": 900}]`))
   if err != nil {
      fmt.Println(err)
   }

   _, err = readAll(strings.NewReader(`[{"name": "Dubi Gal", "age": 900}]`))
   if err != nil {
      fmt.Println(err)
   }
}
*/

type Player struct {
   Name   string `json:"name"`
   Age    int    `json:"age"`
}

func jsonDecoder(r io.Reader) ([]Player, error) {
   var players []Player
   return players, json.NewDecoder(r).Decode(&players)
}

func readAll(r io.Reader) ([]Player, error) {
   var players []Player
   bytez, err := ioutil.ReadAll(r)
   if err != nil {
      return players, err
   }

   return players, json.Unmarshal(bytez, &players)
}
```

我们去基准测试吧！(更大的数据集显然会产生更准确的结果)
测试文件:

```
package main

import (
   "strings"
   "testing"
)

var players = `[{"name": "Dubi Gal", "age": 900}, {"name": "jojo", "age": 51}]`

func BenchmarkJsonDecoder(b *testing.B) {
   b.ReportAllocs()
   for n := 0; n < b.N; n++ {
      jsonDecoder(strings.NewReader(players))
   }
}

func BenchmarkReadAll(b *testing.B) {
   b.ReportAllocs()
   for n := 0; n < b.N; n++ {
      readAll(strings.NewReader(players))
   }
}
```

结果:

```
$ go test -bench=.goos: darwin
goarch: amd64
pkg: medium/aaa2
cpu: Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz
BenchmarkJsonDecoder-12           600022              1984 ns/op            1128 B/op         15 allocs/op
BenchmarkReadAll-12               620658              1820 ns/op             928 B/op         13 allocs/op
PASS
ok      medium/aaa2     3.733s
```

在这个基准测试中，我们发现`io.ReadAll`方法的性能稍好一些。这是由于对象已在内存中。
在流式传输和/或处理大量数据(数百兆字节或更多)时，JSON Decoder 可能是更好的选择。但现在我们已经知道如何进行基准测试，您可以自行进行基准测试和实验！