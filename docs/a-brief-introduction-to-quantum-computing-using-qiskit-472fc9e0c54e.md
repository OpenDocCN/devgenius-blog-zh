# 使用 Qiskit 的量子计算简介

> 原文：<https://blog.devgenius.io/a-brief-introduction-to-quantum-computing-using-qiskit-472fc9e0c54e?source=collection_archive---------14----------------------->

量子计算被吹捧为所有计算的未来，治愈癌症的关键和加密的终结(所有这些都是相当疯狂的说法，目前最大的量子计算机有 127 个量子位，所以我们离我们的加密钱包处于任何危险中还有很长的路要走)。

![](img/9c5a25a9a977d5fa23faf215a1a63c9f.png)

## **为什么量子计算机如此特别？**

你可能会问，“有什么大不了的？量子计算机能做什么我用 Mac 已经做不到的事情？”嗯，量子计算机与常规计算机相比有两个关键优势:**叠加**和**纠缠。**

**叠加:**在经典计算机中，一个比特可以有 0 或 1 的值，我们可以使用电路打开(1)或关闭(0)这个比特。在量子计算机中，一个量子位可以是 1 和 0 的组合，具有一定的概率(即当你观察量子位时，40%是 1，60%是 0)。(更多信息请参见这个[关于测量量子位的微软文档](https://docs.microsoft.com/en-us/azure/quantum/concepts-the-qubit))。因此，在经典计算机中，如果我们想要存储 001、010、011、100、101、110 和 111，我们需要 3*8 = 24 位。但是在量子计算中，我们可以将所有这些值存储在仅仅 3 个量子位中。更一般地，如果我们想要存储所有可能的 n 长度“比特”序列，我们只需要 n 个量子比特，但是我们需要 N *个 2^N 经典比特。如果这个数字仅仅是 10，那么它将是 10 对 10，240(并且从那里开始呈指数增长)。

![](img/e10e5a3cb73f5bd27e2135fc596b171c.png)

红色:量子位|蓝色:经典位| X:二进制长度| Y:所需“位”的数量

**纠缠**

量子计算的另一个很酷的特性是纠缠，当你纠缠两个量子位时，它们将总是(在一定的容错范围内)具有相同的值。假设我纠缠量子位 A 和量子位 B，然后我把 A 转换成值为 1。如果我观察 B，它的值也是 1。如果我将 A 设置为 0 并观察 B，那么 B 也会将其值更改为 0。这个特性很酷，因为在经典系统中，如果我想将 N 位从 1 翻转到 0，并翻转 K 次，我需要执行 N*K 次操作。在量子计算中，这就变成了单个量子位上的 N + K，N 个纠缠 ops 和 K 个翻转 ops(会自动翻转纠缠量子位)

## 创建量子电路

虽然你可能没有一台量子计算机(除非你从 IBM 偷了一台，在这种情况下，我为你的偷窃能力喝彩),但 python 中有一个名为 Qiskit 的库，它可以让你创建和模拟量子电路。

如果你想继续的话，下面是 colabs 的链接

[](https://colab.research.google.com/drive/1EK1Gsr55MNE92Qi7IRyTjlevunTM6m10) [## 量子计算

colab.research.google.com](https://colab.research.google.com/drive/1EK1Gsr55MNE92Qi7IRyTjlevunTM6m10) 

导入所有必要的包后，我们将创建我们的电路。

```
circuit = QuantumCircuit(2, 2)
```

这是一个简单的电路，有 2 个量子位可以操作，2 个经典位可以读取输出。

```
circuit.h(0)circuit.cx(0, 1)circuit.measure([0,1], [0,1])
```

我们将第一个量子位通过哈达玛门(这使得量子位处于叠加态)。然后，我们让两个量子位通过一个受控的非操作，使它们纠缠在一起，这样量子位 1 将反映量子位 0 取的任何值。然后，我们测量将第一量子位状态放入第一经典位并将第二量子位状态放入第二经典位的状态。

![](img/5fd4922c3fce0e7a94e932230d4848de.png)

最终电路

```
simulator = QasmSimulator()compiled_circuit = transpile(circuit, simulator)job = simulator.run(compiled_circuit, shots=1000)result = job.result()counts = result.get_counts(circuit)print("\nTotal count for 00 and 11 are:",counts)
```

然后，我们可以模拟运行 2 个量子位 1000 次，并测量我们得到 00 和 11 的时间。

![](img/e08387f6a57364e79a4718bf06ac0905.png)

在这个非常简单的电路中，我们使用了叠加(将量子位 0 通过哈达玛门)和纠缠(对我们的量子位执行受控非操作)。