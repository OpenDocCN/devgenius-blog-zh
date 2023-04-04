# Analyzing New Unique Identifier Formats (UUIDv6, UUIDv7, and UUIDv8)

> 原文：<https://blog.devgenius.io/analyzing-new-unique-identifier-formats-uuidv6-uuidv7-and-uuidv8-d6cc5cd7391a?source=collection_archive---------1----------------------->

![](img/0689a363750258aa3e5146484b5d6cae.png)

Photo by [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

I have [written before](https://medium.com/geekculture/the-wild-world-of-unique-identifiers-uuid-ulid-etc-17cfb2a38fce) about UUIDs and other unique identifiers. As a reminder UUIDs are 128-bit identifiers that strive for unique identifier generation without requiring the generation to be done in a centralized location. The specification for UUIDs was written in 2005 and is defined in [RFC 4122](https://www.rfc-editor.org/rfc/rfc4122). This specification has served the industry fairly well. Even so there have been many other mechanisms for generating unique identifiers to try to make up for the shortcomings of the original specification. Some of these shortcomings are the following:

*   Poor index locality of non-time-based UUIDs (such as v4) due to new IDs not being related to previously generated ids. This leads to negative performance impacts when using many common indexing data structures.
*   UUIDv1 use of a 100-nanosecond epoch is a unique and difficult to work with timestamp in many systems.
*   To time order ids introspection and parsing are required. (This is related to point 1).
*   UUIDv1 makes use of MAC addresses for part of its identifier. This leads to privacy and security concerns as well as, with the advent of virtual machines and containers, uniqueness is no longer guaranteed.
*   RFC4122 doesn’t detail the difference in requirements between the generation and storage of UUIDs which often are different.

After reviewing 16 different community ID generation algorithms, the working group created [this draft IETF document](https://www.ietf.org/id/draft-peabody-dispatch-new-uuid-format-04.html). It should be noted this is just a draft document that expires on December 25, 2022\. There is a likelihood that there will be differences between the specification at the time of writing this post and what is finally accepted but it is still instructive to review (there have already been a handful of revisions of this document in the month since the draft was initially released).

Four new UUID specifications are defined:

*   UUID Version 6 (UUIDv6) — A simple reordering of the bits within a UUIDv1 to allow it to be sorted as an opaque sequence of bytes.
*   UUID Version 7 (UUIDv7) — A new time-based UUID bit layout based on the Unix Epoch timestamp already widely used in the industry.
*   UUID Version 8 (UUIDv8) — A free-form format whose only requirement is to keep backward compatibility.
*   马克斯·UUID——一种特殊的 UUID，与 RFC 4122 中提出的[零 UUID](https://www.rfc-editor.org/rfc/rfc4122#section-4.1.7) 相反

让我们来看一下其中每一项的一些细节。

## UUIDv6

UUIDv6 的预期用例是作为 UUIDv1 的替代产品，它提供了改进的数据库本地性。如果您不需要保持与 UUIDv1 的兼容性，建议您使用 UUIDv7。UUIDv6 和 UUIDv1 之间唯一真正的区别是时间戳位的顺序。从 60 位时间戳开始，时间戳的前 48 位首先出现在 UUID 中(规范将它在`time_high`和`time_mid`之间分开，可能会保留与 RFC 4122 相同的术语)。接下来的 4 位包含版本(在本例中是版本 6 的 0110 ),然后可以找到时间戳的最后 12 位。这导致了以下差异:

UUIDv1

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          time_low                             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       time_mid                |         time_hi_and_version   |
```

UUIDv6

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           time_high                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           time_mid            |      time_low_and_version     |
```

其余的 UUID 代与 UUIDv1 相同，尽管在规范草案中有一个注释鼓励使用伪随机值来代替原始规范中指定的旧的 [MAC 地址行为](https://rfc-editor.org/rfc/rfc4122#section-4.1.6)。也就是说，基于 MAC 地址的行为仍然是允许的。

## UUIDv7

这是要使用的新的鼓励 UUID 方法。对于它的时间戳，它没有使用时间戳作为自 1582 年 10 月 15 日 00:000:00.00 以来的 100 纳秒间隔的计数，而是使用更标准的 Unix 纪元作为其源，它计算自 1970 年 1 月 1 日午夜以来的毫秒数，闰秒除外。

考虑到这一点，格式非常简单。前 48 位是自 Unix 纪元以来的大端无符号毫秒数。接下来的四位是版本位(0111)，后面是 12 位伪随机数据。接下来，我们有 2 位变体，最后还有 62 位伪随机数据。

## UUIDv8

我将 UUIDv8 视为 UUIDv4 的兄弟。UUIDv8 和 UUIDv4 仅指定版本和变量位位于正确的位置。不同之处在于 UUIDv4 指定剩余的 122 位是伪随机生成的。UUIDv8 建议生成的值仍然是基于时间的，但是如何生成的实现细节取决于实现者。这意味着我们有 48 位自定义实现，4 位用于版本(1000)，12 位自定义实现，2 位用于变体，最后是 62 位，视实现情况而定。这给实现者留下了很多空间，但它有足够的规则，可以在现有的 UUID 环境中共存。

## 马克斯·UUID

最大 UUID ID 似乎是 RFC 4122 的一点疏忽，因为它只是其中定义的零 UUID 的倒数。零 UUID 是 128 位的 0，而最大 UUID 是 128 位的 1。

有了正式的定义之后，规范就开始描述这些算法的实现者需要考虑的事项。我将提到一些我认为最有趣/最重要的。

**时间戳可靠性:**在这些算法中使用的时间戳非常重要，事实上它们一直在增加。如果时间戳的来源可以向后移动，那么需要注意确保值是不断增加的。这方面的技术将在本文档中进一步讨论。

**时间戳来源:**规范中调出了公历(v1 和 v6)和 Unix (v7)纪元。如果希望使用不同的时期，应该利用 UUIDv8。

**时间戳精度:** UUIDv1 和 UUIDv6 指定时间戳粒度为 100 纳秒。UUIDv7 指定毫秒精度。如果需要不同的精度，应使用 UUIDv8。

**时间戳准确性:**该规范不保证时钟值和实际时间需要有多接近。这使得实现者能够以他们自己的方式考虑安全问题、闰秒和不准确的时钟。

**时间戳填充/截断:**当需要填充时(例如，32 位时间戳将填充 UUIDv7 中的 48 位)，最高有效(最左边)位应该用零填充。类似地，如果要截断时间戳(例如，在 UUIDv7 的 48 位点中使用 64 位时间戳)，则将截断最低有效(最右边)位。

## 单调性

单调，或总是增加的属性，是基于时间的 UUID 生成的支柱。这些 UUIDs 基于时间的性质赋予了它们单调性的一般属性，因为时间不断向前移动。也就是说，例如，如果必须为特定的时间戳生成多个 UUID(例如，对于 UUIDv7，在特定的毫秒内)，则应该引入额外的逻辑来确保 uuid 不断增加。如果需要为一个特定的时间戳生成多个 id，可以利用一些技术。

**定长计数器:**

这种技术留出了固定数量的位，用于方便在特定时间戳内生成的 IDs 计数器。UUID 中计数器位的位置应该直接跟在时间戳位之后，以便于正确排序。应该给计数器分配多少位应该由在特定时间戳期间可以生成多少 id 来确定。还应该考虑时间戳的精度。例如，毫秒精度的计数器的大小需要比纳秒精度的时间戳大。该规范建议计数器的长度至少应为 12 位，但不超过 42 位。拥有一个太大的计数器将会消除 UUID 这一代人的许多不可预测性。计数器应该在每个时间戳滴答时初始化为随机值。如果分配给计数器的位数不足，这种方法特别担心溢出。为了试图避免翻转问题，实现可以考虑仅使用最右边的位来随机初始化计数器的一部分。例如，对于 23 位计数器，只初始化最右边的 22 位并用零填充左边的位，可以帮助确保至少有 4，194，304 个值可用于计数器。

**单调随机**:

另一种技术是使用随机位作为计数器。其工作方式是为每个时间戳标记生成一个新的随机值，然后为特定标记生成的每个 UUID 增加随机值。增量值应该是大于零的随机整数。增量应该是随机的，以允许一定程度的不可避免性。如果可猜测性不是一个问题，你可以选择使增量值为 1。

**反侧翻处理**

即使采取了上述预防措施，反向翻车仍然是可能的。因此，实现应该检测它们何时将要翻转，而不是暂停 UUID 生成，直到下一个时间戳滴答

**单调性验证**

即使在翻转之外，也应该检查每个生成的 UUID 是否大于先前生成的 UUID。如果不是，可能是由于时钟回滚、闰秒或计数器翻转，如上所述。在这种情况下，应用程序应该采取适当的措施来确保下一个生成的 UUID 大于前一个。这可以通过使用先前的时间戳并简单地再次递增计数器、冻结直到时间戳赶上来或者保持单调性的另一种技术来完成。

## 分布式 UUID 发电

虽然这些 UUID 生成机制中固有的随机性在生成将被存储在中央位置的标识符的不同节点之间提供了合理水平的唯一性，但是可能希望采取进一步的行动来避免冲突。一种方法是在使用前通过中央存储库检查每个 ID，这种方法可行，但确实抵消了使用 UUID 的大部分好处。另一种机制可以是为每个生成 IDs 的节点分配一个唯一的标识符。该标识符将是所生成的 ID 的一部分，因此将以这种方式为每个节点提供其自己的比特空间。这个节点标识符在生成的 ID 中显示在哪里取决于实现者，但是要确保生成的 ID 仍然是按时间顺序排列的。使用节点标识符作为设计一部分的自定义 UUID 生成方案应该利用 UUIDv8。当决定应该花费多少努力来确保唯一性时，要考虑重复生成的 ID 的含义。如果影响较低且可接受，则尽可能少地投入额外努力来解决分布式 UUID 发电问题可能是合理的。如果成本和风险很高，那么确保唯一性的额外努力可能是值得的，并且应该进行。如同所有的软件工程一样，权衡总是存在的。

## 最后的想法

规范以一些最后的想法结束。首先，这些 UUIDs 可以提供本地唯一性保证。如果需要更高层次的唯一性保证，那么就需要某种生成 UUIDs 的共享存储库。然后，它继续提醒实现者使用加密安全的伪随机数生成器，并确保它们被正确播种。这是使生成的 UUIDs“不可访问”的根源。UUIDs 应该被它们的消费者视为不透明的字节，并且它们的消费者不应该尽可能多地检查这些位。即便如此，如果使用 UUIDv6 或 UUIDv7，使用者应该将 UUIDs 作为不透明字节排序。这些 UUIDs 的文本表示也应该是按字典顺序排序的。如果出于任何原因需要小端字节顺序，则使用该规范生成的 UUIDs 是以大端字节顺序创建的，应该使用 UUIDv8。当使用 UUID 作为数据库中的列时，应该使用它的字节表示，而不是它的文本表示。通过使用字节表示，每个 UUID 可以节省 40 个字节。可能会有争论。然而，如果数据的消费者总是使用文本表示，那么以其文本形式存储数据可能是值得的，这样可以在节省空间的同时提高速度。最后，该规范简要讨论了 UUIDs 的安全性问题。它再次鼓励实现者不要使用 MAC 地址作为 UUIDs 的一部分，而是使用加密的安全随机数。嵌入在 UUID 中的时间戳确实为 UUID 的消费者提供了有关 UUID 生成的少量信息，并为其提供了排序信息。如果暴露该信息是不可接受的，则应使用 UUIDv4。

我发现开发这些规范的思维过程和方法非常有趣。从一开始就非常小心地试图得到这些规范。我也对原始规范的微小变化如何导致结果的显著变化很感兴趣。时间会告诉我们，在这个规范被接受之前，是否会对它进行额外的修改，以及这些新的标识符类型会有多好，但是我总是喜欢在工具箱中加入额外的工具。