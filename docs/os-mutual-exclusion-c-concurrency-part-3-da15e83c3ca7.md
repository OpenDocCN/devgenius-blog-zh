# [OS]互斥(C++) —并发性第 3 部分

> 原文：<https://blog.devgenius.io/os-mutual-exclusion-c-concurrency-part-3-da15e83c3ca7?source=collection_archive---------10----------------------->

到目前为止，我们已经讨论了什么是并发性以及操作系统如何管理并发性。今天我们要讲的是**线程库**，尤其是互斥体(互斥)，以及作为程序员如何使用这个。

## **线程库……**

为了利用并发性，OS 向用户提供了一个线程库。线程库中的主要功能是**“互斥、CV 和线程”**为了正确使用这种并发性，我们需要一些机制来强制每个线程同步(排序)。编写并发程序的主要问题是**识别共享状态和关键部分，并用适当的同步工具保护它们。**

## 互斥体(互斥)

互斥锁提供了对象的独占所有权。它提供了两个功能， **lock()** 和 **unlock()** 。基本上，如果一个线程锁定了这个互斥体，没有线程可以同时锁定同一个互斥体。其他线程应该等待，直到该互斥体的所有者解锁该互斥体。此外，只有锁定互斥体的线程才能解锁互斥体。互斥体就像一个你需要在小组讨论中发言的对象。只有拥有这个对象的人才能说话，他或她应该把这个传给另一个人，这样那个人才能说话。关于 lock()重要的一点是，它"**自动地**"获取一个锁。因此，两个线程不可能同时持有锁。

当我们编写并发程序时，使用它的一个典型方式是作为**保护**。当我们有一个共享状态时，我们基本上锁定互斥体，用共享状态做一些事情，并在事情完成后解锁它。通过这样做，另一个想要修改共享状态的线程应该首先获得互斥体，这就强制了线程之间的排序。让我们看一些代码。

假设我们想要构建一个多线程的银行应用程序。我们有多个线程在监听新的事务，我们希望根据新的事务更新我们的钱包帐户。我们有一个钱包结构，它有一个地址、所有者、余额和交易向量。我们想写一个存款功能，向这个钱包添加一个新的交易，并更新余额。如上所述，我们需要**识别共享状态**和**临界区**。这里的共享状态是“wallet”，因为多个线程正在监听新的事务，它们都访问单个 wallet 帐户来更新余额。因此，我们需要**用互斥体**包装接触“钱包”的部分。识别这些部分就是识别关键部分。我们需要看到**依赖于我们的“共享状态”**的所有变量，并包装所有读取或写入“共享状态”的部分下面是示例代码。

```
#include <thread>

struct Wallet{
  string address; // address of this wallet
  string owner; // owner of wallet
  unsigned int balance; // current balance
  vector<Transaction> txs; // transactions that include this account
  /* Transaction includes senderAddress, receiverAddress, amount 
  */
};

Wallet wallet; 
std::mutex wallet_m;

void deposit(Transaction &tx){
  // verify transaction ... 
  wallet_m.lock(); // critical section starts... 
  // we reads the wallet and,
  if(wallet.txs.find(tx) != wallet.txs.end()){
    throw "This transaction is already processed";
  }
  wallet.txs.push_back(tx); // updates transactions 
  wallet.balance += tx.amount; // updates balance
  // done with modifying wallet, so unlock it.
  wallet_m.unlock();
}
```

假设我们只想检查该事务是否已经被解析，并想使用 for 循环。先看下面的代码。

```
bool isAlreadyParsed(Transaction &tx){
  wallet_m.lock();
  int txRecordSize = wallet.txs.size(); // get the size of txs
  wallet_m.unlock();

  for(int i = 0; i < txRecordSize; i++){
    wallet_m.lock();
    if(wallet.txs[i] == tx)
      return true;
    wallet_m.unlock();
  }
  return false;
}
```

这是正确的吗？我们正在包装我们读到“wallet.txs”的部分。实际上，这是一个错误的实现，也是一个不正确识别依赖关系的例子。当我们做" int txRecordSize = wallet . txs . size()；"变量 txRecordSize 依赖于 wallet，这是一种共享状态。因此，当我们在 for 循环中使用这个变量时，它暴露了没有任何互斥保护的依赖性。因此，我们应该包装整个 for 循环。

```
bool isAlreadyParsed(Transaction &tx){
  wallet_m.lock();
  int txRecordSize = wallet.txs.size(); // get the size of txs

  for(int i = 0; i < txRecordSize; i++){ // txRecordSize depends on wallet,
                                  //So, it should be protected too.
    if(wallet.txs[i] == tx) 
      return true;
  }
  wallet_m.unlock();
  return false;
}
```

这是正确的吗？我们用互斥体保护了一个关键部分，看起来相当不错。然而，如果在我们解锁 wallet_m 之后，另一个线程获得了一个锁，并添加了那个事务，会发生什么呢？“isAlreadyParsed”函数将返回 false。这可以接受吗？在这种情况下，我们可以说，从技术上讲，当我们调用 isAlreadyParsed 时，该事务不在事务向量内。然而，在“返回真”时会发生什么呢？我们持有锁，不解锁就返回 true 所以我们应该在返回之前加上 unlock

## 真麻烦！

每次在临界区内进行提前返回时，我们都应该添加 unlock()。此外，如果我们抛出一个错误，那么我们也应该添加 unlock()。这么多工作！为了解决这个问题，我们可以使用 RAII 模式。

## RAII 模式

RAII 模式使用对象的**析构函数**并使用变量的**作用域**来自动释放资源或锁。函数返回后，函数内部的所有局部变量都超出了作用域，所以它们都被析构了。此外，当函数抛出一个错误时，它会清理它的局部对象，所以局部变量也会被析构。我们可以用这个来简化锁定。

```
class Monitor{
  mutex m;
  Monitor(mutex &_m) : m(_m) {
    m.lock(); // lock mutex when it gets created
  }
  ~Monitor(){
    m.unlock(); // unlock mutex when it gets destructed
  }
}; 
```

现在，我们可以使用这个监视器来锁定/解锁。

```
bool isAlreadyParsed(Transaction &tx){
  Monitor monitor(wallet_m);
  int txRecordSize = wallet.txs.size(); // get the size of txs

  for(int i = 0; i < txRecordSize; i++){ // txRecordSize depends on wallet,
                                  //So, it should be protected too.
    if(wallet.txs[i] == tx) 
      return true;
  }
  return false;
}
```

很简单。当我们创建一个监视器时，我们锁定 wallet_m，由于它是这个函数的一个局部变量，所以当它超出作用域时(当一个函数返回或抛出一个错误时)就会释放这个锁。

其实 cpp 库就是这么实现 uique_lock 和 unique_ptr 的。因此，如果您不想自己实现它，您可以在编写代码时使用 cpp 提供的 unique_lock。它提供了更多的功能，比如“交换”锁，这在你解除锁的时候非常酷。

 [## 标准::唯一 _ 锁定

### unique_lock 类是一个通用的互斥所有权包装器，允许延迟锁定、有时间限制的尝试…

en.cppreference.com](https://en.cppreference.com/w/cpp/thread/unique_lock) 

下一次，我们可以更多地讨论互斥体实际上是如何实现的。它如何在锁定时提供“原子性”？