# Graal VM

:::tip 视频公开课

本节笔者有公开课介绍：[《GraalVM：云原生时代的 Java》](/tricks/2020/graalvm/video)

:::

网上每隔一段时间就能见到几条“未来 X 语言将会取代 Java”的新闻，此处“X”可以用 Kotlin、Golang、Dart、JavaScript、Python……等各种编程语言来代入。这大概就是长期占据[编程语言榜单](https://www.tiobe.com/tiobe-index/)第一位的烦恼，天下第一总避免不了挑战者相伴。

如果 Java 有拟人化的思维，它应该从来没有惧怕过被哪一门语言所取代，Java“天下第一”的底气不在于语法多么先进好用，而是来自它庞大的用户群和极其成熟的软件生态，这在朝夕之间难以撼动。不过，既然有那么多新、旧编程语言的兴起躁动，说明必然有其需求动力所在，譬如互联网之于 JavaScript、人工智能之于 Python，微服务风潮之于 Golang 等等。大家都清楚不太可能有哪门语言能在每一个领域都尽占优势，Java 已是距离这个目标最接近的选项，但若“天下第一”还要百尺竿头更进一步的话，似乎就只能忘掉 Java 语言本身，踏入无招胜有招的境界。

2018 年 4 月，Oracle Labs 新公开了一项黑科技：[Graal VM](https://www.graalvm.org/)，从它的口号“Run Programs Faster Anywhere”就能感觉到一颗蓬勃的野心，这句话显然是与 1995 年 Java 刚诞生时的“Write Once，Run Anywhere”在遥相呼应。

:::center
![](./images/grallvm.png)
Graal VM
:::

Graal VM 被官方称为“Universal VM”和“Polyglot VM”，这是一个在 HotSpot 虚拟机基础上增强而成的跨语言全栈虚拟机，可以作为“任何语言”的运行平台使用，这里“任何语言”包括了 Java、Scala、Groovy、Kotlin 等基于 Java 虚拟机之上的语言，还包括了 C、C++、Rust 等基于 LLVM 的语言，同时支持其他像 JavaScript、Ruby、Python 和 R 语言等等。Graal VM 可以无额外开销地混合使用这些编程语言，支持不同语言中混用对方的接口和对象，也能够支持这些语言使用已经编写好的本地库文件。

Graal VM 的基本工作原理是将这些语言的源代码（例如 JavaScript）或源代码编译后的中间格式（例如 LLVM 字节码）通过解释器转换为能被 Graal VM 接受的[中间表示](https://zh.wikipedia.org/wiki/%E4%B8%AD%E9%96%93%E8%AA%9E%E8%A8%80)（Intermediate Representation，IR），譬如设计一个解释器专门对 LLVM 输出的字节码进行转换来支持 C 和 C++语言，这个过程称为“[程序特化](https://en.wikipedia.org/wiki/Partial_evaluation)”（Specialized，也常称为 Partial Evaluation）。Graal VM 提供了[Truffle 工具集](https://github.com/oracle/graal/tree/master/truffle)来快速构建面向一种新语言的解释器，并用它构建了一个称为[Sulong](https://github.com/oracle/graal/tree/master/sulong)的高性能 LLVM 字节码解释器。

以更严格的角度来看，Graal VM 才是真正意义上与物理计算机相对应的高级语言虚拟机，理由是它与物理硬件的指令集一样，做到了只与机器特性相关而不与某种高级语言特性相关。Oracle Labs 的研究总监 Thomas Wuerthinger 在接受[InfoQ 采访](https://www.infoq.com/news/2018/04/oracle-graalvm-v1/)时谈到：“随着 Graal VM 1.0 的发布，我们已经证明了拥有高性能的多语言虚拟机是可能的，并且实现这个目标的最佳方式不是通过类似 Java 虚拟机和微软 CLR 那样带有语言特性的字节码”。对于一些本来就不以速度见长的语言运行环境，由于 Graal VM 本身能够对输入的中间表示进行自动优化，在运行时还能进行即时编译优化，往往使用 Graal VM 实现能够获得比原生编译器更优秀的执行效率，譬如 Graal.js 要优于 Node.js、Graal.Python 要优于 CPtyhon，TruffleRuby 要优于 Ruby MRI，FastR 要优于 R 语言等等。

针对 Java 而言，Graal VM 本来就是在 HotSpot 基础上诞生的，天生就可作为一套完整的符合 Java SE 8 标准的 Java 虚拟机来使用。它和标准的 HotSpot 差异主要在即时编译器上，其执行效率、编译质量目前与标准版的 HotSpot 相比也是互有胜负。但现在 Oracle Labs 和美国大学里面的研究院所做的最新即时编译技术的研究全部都迁移至基于 Graal VM 之上进行了，其发展潜力令人期待。如果 Java 语言或者 HotSpot 虚拟机真的有被取代的一天，那从现在看来 Graal VM 是希望最大的一个候选项，这场革命很可能会在 Java 使用者没有明显感觉的情况下悄然而来，Java 世界所有的软件生态都没有发生丝毫变化，但天下第一的位置已经悄然更迭。
