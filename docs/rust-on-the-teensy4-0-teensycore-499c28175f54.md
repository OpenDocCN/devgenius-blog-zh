# 青少年身上的铁锈 4.0 —青少年核心

> 原文：<https://blog.devgenius.io/rust-on-the-teensy4-0-teensycore-499c28175f54?source=collection_archive---------8----------------------->

![](img/3e8c43d8f1a7a8d254ad8de51c0918d8.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Rust 是跨平台编译的传奇。ARM 芯片组的变体也在列表中。不久前，我着手为用 rust 编写的 Teensy 4.0 创建一个裸机内核。我很快意识到将我的代码提取到一个可重用的平台中是相当简单的。这样，teensycore 就建成了，你也可以享受这个极其轻量级的内核！

Teensycore 是完全开源的，欢迎投稿！

**资源**

*   Teensy 开发板:[https://www.pjrc.com/store/teensy40.html](https://www.pjrc.com/store/teensy40.html)
*   内核源代码:[github.com/SharpCoder/teensycore](http://github.com/SharpCoder/teensycore)
*   crates . io:[https://crates.io/crates/teensycore](https://crates.io/crates/teensycore)
*   文件:[https://docs.rs/teensycore/0.0.11/teensycore/](https://docs.rs/teensycore/0.0.11/teensycore/)

在本文中，我将解释 teensycore 内置的一些特性，并提供一些如何使用它的示例。首先，简单介绍一下功能。

# **功能**

在撰写本文时，teensycore 已经完全具备了以下特性:

*   对所有 5 个 UART 外设的本机支持
*   软件 i2c
*   定期计时器
*   基于引脚编号的 Gpio 寻址
*   全功能中断系统
*   精确到纳秒`wait()`
*   132MHz 的优美时钟速度
*   硬件优化的浮点数学

除了这些硬件级系统之外，teensycore 还拥有一个非常轻量级的“标准库”(质量可疑)，其中包括如下数据结构:

*   链接列表
*   BTreeMap
*   具有队列/堆栈操作的固定大小数组
*   具有队列/堆栈操作的可变大小向量
*   带有基本回收系统的分页内存
*   堆栈支持的字符串
*   还有更多！

teensycore 拥有这些东西的原因是因为它符合`#![no_std]`。所以我们知道并喜爱的标准库是不可用的。

现在来看一个例子。来看看大家的最爱:blinky。

# Blinky

我们将闪烁内置的 LED。不需要电线！以下是从头开始一个新的 teensycore 项目的逐步指南。

**编制项目**

首先，您需要安装一些构建工具。虽然我们使用`cargo`来完成繁重的工作，但是有一点点`c`，一些链接器动作，以及一堆货物标志发生在幕后。

```
sudo apt install gcc-arm-none-eabi jq
```

接下来，您需要配置 rust，以便它能够利用适当的工具链。

```
rustup default nightly
rustup target add thumbv7em-none-eabi
```

您现在已经准备好创建一个 teensycore 项目了。

```
cargo new blinky --lib
cd blinky
```

修改您的`Cargo.toml`文件，使其包含这个 lib 部分，并在依赖项中包含 teensycore。

```
[package]
name = "blinky"
version = "0.1.0"
edition = "2021"[lib]
crate-type = ["staticlib"]
path = "src/lib.rs"[dependencies]
teensycore = "^0.0.11"
```

大部分都是标准的东西，但是构建本身有点奇怪。不幸的是，它目前是用 bash 编写的——所以它只能在 Linux/WSL/Mac 机器上工作。

下载构建文件。

```
curl [https://raw.githubusercontent.com/SharpCoder/teensycore/main/build-template.sh](https://raw.githubusercontent.com/SharpCoder/teensycore/main/build-template.sh) > build.sh# Add execute permission to the build 
chmod +x ./build.sh
```

因为额外的链接步骤让我们的内核在文件中的正确位置，也因为那个讨厌的`.c`文件——这个`build.sh`脚本诞生了。它本质上是 cargo 构建你的 rust，然后使用`arm-none-eabi-ld`来连接所有东西，使用`objcopy`来发出一个十六进制文件。

剩下的就是青少年中心的入口了。下面是你可以放入`src/lib.rs`开始的代码。

```
#![feature(lang_items)]
#![no_std]teensycore::main!({
    pin_mode(13, Mode::Output); loop {
        pin_out(13, Power::High);
        wait_ns(1 * S_TO_NANO);
        pin_out(13, Power::Low);
        wait_ns(1 * S_TO_NANO);
    }
});
```

`teensycore::main!({ /* code here*/ })`宏包括一堆我的标准库，配置默认 UART，启动时钟外设，等等。

我的设计目标是让这个宏尽可能地接近一个空白的 Arduino 项目。不要想太多正在发生的事情，只要认真对待！

`pin 13`是一个 gpio 引脚，teensy 4.0 中的内部 LED 通过该引脚连接。所以在 teensycore 每秒切换一次是很容易的。

让我们探索一个更高级的项目。

# 在 I2C 上空读/写数据

接下来，我们来探索 i2c 接口。我无耻地模仿 Arduino 接口，因为这很有意义。假设你有一个额外的 eeprom 芯片(在这个例子中，我使用的是 [**24LC512**](https://ww1.microchip.com/downloads/aemDocuments/documents/MPD/ProductDocuments/DataSheets/24AA512-24LC512-24FC512-512K-Bit-I2C-Serial-EEPROM-20001754Q.pdf) )。下面是如何从中读取和写入数据…

(注意:我有 SDA 线连接到引脚 18，SCL 线连接到引脚 19)

```
#![feature(lang_items)]
#![crate_type = "staticlib"]
#![no_std]teensycore::main!({
    use teensycore::*; let mut wire = I2C::begin(18, 19);
    wire.set_speed(I2CSpeed::Fast400kHz); wire.begin_transmission(0x50, true);
    // First two bytes are memory address
    wire.write(&[0, 0]);
    // Next is a sequential write of data
    wire.write(b"EARTH");
    wire.end_transmission();

    // Settle time for whole-page write. Per docs.
    wait_ns(250 * MS_TO_NANO); // Select the address we wish to read
    wire.begin_transmission(0x50, true);
    wire.write(&[0, 0]);
    wire.end_transmission(); // Perform read request
    wire.begin_transmission(0x50, false);

    // Use the `debug_str` functionality to output this data to the
    // TX2 UART. Pin 8 on the teensy. For debugging purposes.
    debug_str(&[
        // Send 'true' as the second parameter to include an ack
        // This tells the chip we wish to do sequential reads
        // with automatic addr incrementation.
        wire.read(true),
        wire.read(true),
        wire.read(true),
        wire.read(true),
        wire.read(true),
    ]);
    wire.end_transmission();
});
```

当我最初编写这段代码时，我将一个实际的 arduino 连接到引脚 8 (TX2 ),并使用 Arduino 串行监视器来监视 eeprom 结果。这是对 UART 和 I2C 系统的一次很好的外部验证。

让我们把它分解一下。

```
let mut wire = I2C::begin(18, 19);
wire.set_speed(I2CSpeed::Fast400kHz);
```

根据 i2c 文档，Teensycore i2c 支持“快速模式”和正常模式。我的 i2c 库在普通 gpio 线上运行速度非常快。因此，您可以将它与任何可用的 GPIO 引脚配合使用，这非常简单。

类似于 Arduino `Wire`库完成一个事务。

```
wire.begin_transmission(0x50, true);
wire.write(&[0, 0]);
wire.write(b"EARTH");
wire.end_transmission();
```

你`begin_transmission( addr, write_mode )`，完成后再`end_transmission()`。

如果您为第二个参数指定`false`，您将处于读取模式。并且抓取字节可以这样做:`wire.read(true)`那个参数只是告诉是否发回 ack。

这就是 i2c 通信库的全部内容。

# 结论

我们已经介绍了从零开始启动一个新的 teensycore 项目以及利用各种子系统来闪烁灯光、通过 i2c 通信和发出调试串行信息的基础知识。随着时间的推移，我会添加到这个内核中，并写更多关于如何使用新老特性的文章。