# Golang 中智能的错误处理方式便于调试

> 原文：<https://blog.devgenius.io/smart-way-of-error-handling-in-golang-for-easy-debuging-83ef6ed33622?source=collection_archive---------4----------------------->

![](img/b9164e12a1f3d19b161cfe7cf8e67df3.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Chinmay Bhattar](https://unsplash.com/@geekgunda?utm_source=medium&utm_medium=referral) 拍摄的照片

在 golang 中，没有像 try/catch 这样的异常处理来管理错误，但它确实有一个简单的原则，即从函数返回(比如 func3())的早期抛出错误，并从函数的调用方(比如 func2())捕获错误，然后在那里做一些事情，或者它可以简单地返回错误，以便调用方(比如 func1())可以决定如何处理错误。

示例:

```
// You can edit this code!
// Click here and start typing.
package mainimport (
 "errors"
 "fmt"
 "log"
 "os"
)func func1(message string) error {
 if err := func2(message); err != nil {
  fmt.Printf("func2 failed with err:%v\n", err)
  fmt.Println("Trying to create the file and store it in mongo DB") if err := createAndStoreInDB(message); err != nil {
   fmt.Printf("failed to create file in DB err:%v\n", err)
   return fmt.Errorf("failed to create file on neither OS nor in DB")} else {
   fmt.Println("file creation successful in mango DB")
   return nil
  }
 }
 fmt.Println("file creation successful in OS")
 return nil
}func func2(message string) error {
 message = message + "added more content"
 if err := func3(message); err != nil {
  fmt.Printf("func3 failed with err:%v\n", err)
  return err
 }
 return nil
}func func3(message string) error {
 f, err := os.Open("filename.ext")
 if err != nil {
  return err
 }
 fmt.Printf("file created successfully file:%v \n", f)
 return nil
}
func createAndStoreInDB(message string) error {
 message = message + "added more content for DB"
 // call the db function and return error
 //return nil
 return errors.New("failed to connect to db")
}func main() {
 msg := "create the file in the os by adding more content"
 if err := func1(msg); err != nil {
  log.Fatal(err)
 }}
```

输出

```
func3 failed with err:open filename.ext: no such file or directory
func2 failed with err:open filename.ext: no such file or directory
Trying to create the file and store it in mongo DB
failed to create file in DB err:failed to connect to db
2009/11/10 23:00:00 failed to create file on neither OS nor in DB

Program exited.
```

去操场吧

[](https://go.dev/play/p/j9oP9wF5DS8) [## Go Playground-Go 编程语言

### Go Playground 是一个运行在 go.dev 服务器上的 web 服务。该服务接收 Go 程序，审查，编译…

go.dev](https://go.dev/play/p/j9oP9wF5DS8) 

使调试更加直观

如果您在这里观察到开发人员不清楚最初的错误是从哪个文件和行号发生的。他必须浏览所有的代码库，找出其中的位置，这非常耗时，有时还令人沮丧。让我们来看看同一个程序的改进版本，但是使用了更直观的方式来方便调试，特别是对于有数千行代码和不同模块/包的大型项目。

```
// You can edit this code!
// Click here and start typing.
package mainimport (
 "fmt"
 "log"
 "os"
 "runtime"
)func NewError(err error) error {
 _, file, line, _ := runtime.Caller(1)
 return fmt.Errorf("[%s][%d] : %v\n", file, line, err)
}func func1(message string) error {
 log.Println("entering func1")
 defer log.Println("exiting func1")
 if err := func2(message); err != nil {
  log.Printf("func2 failed with err:%v\n", err)
  log.Println("Trying to create the file and store it in mongo DB")if err := createAndStoreInDB(message); err != nil {
   log.Printf("failed to create file in DB err:%v\n", err)
   return NewError(fmt.Errorf("failed to create file on neither OS nor in DB"))} else {
   log.Println("file creation successful in mango DB")
   return nil
  }
 }
 /*
  more code logic
 */
 log.Println("file creation successful in OS")
 return nil
}func func2(message string) error {
 log.Println("entering func2")
 defer log.Println("exiting func2")message = message + "added more content"
 /*
  more code logic
 */
 if err := func3(message); err != nil {
  return err}
 return nil
}func func3(message string) error {
 log.Println("entering func3")
 defer log.Println("exiting func3")/*
  more code logic
 */f, err := os.Open("filename.ext")
 if err != nil {
  return NewError(err)
 }
 log.Printf("file created successfully file:%v \n", f)
 return nil
}
func createAndStoreInDB(message string) error {
 log.Println("entering createAndStoreInDB")
 defer log.Println("exiting createAndStoreInDB")message = message + "added more content for DB"
 // call the db function and return error
 //return nil
 /*
  if err:=connectDB(); err!=nil{
  return NewError(fmt.Errorf("failed to connect to db err:%v",err))
  }
 */
 return NewError(fmt.Errorf("failed to connect to db"))
}func main() {
 /*
  more code logic
 */
 msg := "create the file in the os by adding more content"
 if err := func1(msg); err != nil {
  //log.Println(err)
  log.Fatalf("failed the job err:%v", err)
 }
 /*
  more code logic
 */
}
```

输出:

```
2009/11/10 23:00:00 entering func1
2009/11/10 23:00:00 entering func2
2009/11/10 23:00:00 entering func3
2009/11/10 23:00:00 exiting func3
2009/11/10 23:00:00 exiting func2
2009/11/10 23:00:00 func2 failed with err:[/tmp/sandbox1645831660/prog.go][66] : open filename.ext: no such file or directory

2009/11/10 23:00:00 Trying to create the file and store it in mongo DB
2009/11/10 23:00:00 entering createAndStoreInDB
2009/11/10 23:00:00 exiting createAndStoreInDB
2009/11/10 23:00:00 failed to create file in DB err:[/tmp/sandbox1645831660/prog.go][83] : failed to connect to db

2009/11/10 23:00:00 exiting func1
2009/11/10 23:00:00 failed the job err:[/tmp/sandbox1645831660/prog.go][26] : failed to create file on neither OS nor in DB

Program exited.
```

去操场吧

[](https://go.dev/play/p/yb-btQxE500) [## Go Playground-Go 编程语言

### Go Playground 是一个运行在 go.dev 服务器上的 web 服务。该服务接收 Go 程序，审查，编译…

go.dev](https://go.dev/play/p/yb-btQxE500) 

结论:

我们必须以更有意义的方式包装错误，以便调用者可以在任何类型的错误发生时决定做什么，这需要更多的定制错误。我们可以通过在 struct 数据类型上实现 Error() func 来做到这一点。

[](https://go.dev/play/p/Kl6lU_mVzhe) [## Go Playground-Go 编程语言

### Go Playground 是一个运行在 go.dev 服务器上的 web 服务。该服务接收 Go 程序，审查，编译…

go.dev](https://go.dev/play/p/Kl6lU_mVzhe)