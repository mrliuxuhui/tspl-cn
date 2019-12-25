# 第一章 介绍

Scheme 是一门通用编程语言。它是一门高级语言，它支持对结构化数据进行操作，比如字符串，列表，向量，以及支持操作更传统的数据，比如数字和字符。Scheme 通常被认为适合于符号计算，丰富的数据类型及灵活的控制结构使它成为一种真正的通用编程语言。Scheme 已经被用于编写文本编辑器，优化编译器，操作系统，图形应用，专家系统，数值计算，金融分析，虚拟现实系统以及几乎所有可能的应用类型。Scheme 是一种相当简单易学的语言，因为它基于很少的语法形式和语意概念，并且大多数实现具备的交互性质，鼓励试验性编程。但是，要充分理解 Scheme 是有挑战性的。要在开发中发挥它的潜力需要认真的学习及实践。

在不同机器上的相同版本的 Scheme 实现上，Scheme 程序具有高度的可移植性。因为机器的底层细节几乎完全对程序员隐藏。在不同的 Scheme 实现之间仍然具备可移植性，因为 Scheme 的实现者们一直在努力完善语言标准(Revised Reports), 最新的语言标准是"Revise<sup>6</sup>Report", 通过一组标准库和一个标准机制来定义新的可移植库和顶级程序来强调可移植性。

「注：现在最新的是R7RS了，Scheme 被一分为二，分别为 large 和 small. R7RS small已经发布，large 还在制定当中」

早期的一些 Scheme 实现比较低效，然而，现在很多新的基于编译器的实现运行速度和用低级语言（C，汇编）写的程序一样快。 运行时检查可以帮助程序员发现各种错误，虽然有时候会拖慢速度，但是在大多数实现里，这些检查可以被禁用.

Scheme 支持很多数据类型（或者对象），包括字符，字符串，符号，列表及向量对象，以及一套完整的数值类型，包括复数，实数还有任意精度的有理数。

存储对象所需要的内存空间是动态分配和保持的，直到对象不再需要，然后自动释放，通常由垃圾收集器定期地回收不再需要的对象所占据的内存空间。简单的原子值，例如小整数，字符，布尔值以及空列表等，通常处理成立即数，因而不会产生分配和回收的开销。

不论如何表示，所有对象都是“first-class”对象；因为它们是无限期保留的，它们可以作为参数传递给一个过程，也可以作为过程的返回值 return 回来，或者通过组合形成新的对象。这是和别的语言最大的差别，其它语言的复合数据，比如数组是静态分配的，而且从来不释放，进入一个代码块分配的空间在退出块时无条件地释放，或者由程序员来自己管理释放。

Scheme 是一种 "call-by-value" 语言，即所谓的传值调用。但是，至少可变对象（可以被修改的对象）的值其实是指针，指向真正的存储地址。这些指针隐藏在幕后，程序员不需要关注它们。需要理解的是，当一个对象传递给一个过程或者从过程中返回的时候，并不是对象的拷贝。

Scheme 的核心语法很小，扩展语法是在核心语法的基础上派生而来，核心语法加上扩展语法再加上一组元过程（比如`+ - * /`)构成了完整的 Scheme 语言。一个 Scheme 解释器或编译器可以相当小巧，而且潜在的快速及高度可靠。很多扩展语法及元过程可以由 Scheme 自身定义出来。简化了实现的难度，而且增加了可靠性。

Scheme 代码和数据共享一个相同的打印表示法，这样做的结果是，任何 Scheme 代码都可以自然而然地表示为一个 Scheme 对象。例如，变量及语法关键字可以表示为符号，结构化的语法形式可以表示为列表。这种表示法是 Scheme 提供的语法扩展工具（宏）的基础，通过宏，可以在现有的语法形式及过程的基础上定义出新的语法。它还促进了直接用 Scheme 来实现 Scheme 的解释器，编译器及其它代码转换工具，以及用 Scheme 来实现其它语言转换工具。

Scheme 的变量及关键字是词法作用域的，Scheme 程序使用块结构。可以将标识符导入到一个程序或者库中，或者在一个给定的代码块内部（比如一个库，程序或者函数体）进行局部绑定。局部绑定仅仅是词法可见的，即，程序文本中特定的代码块。块外部出现的相同名字的标识符指向不同的绑定，如果存在于块外部的标识符没有绑定则引用无效。代码块可以嵌套，并且一个块里的绑定可以遮蔽外层的同名绑定。一个绑定的*作用域*就是该绑定所处的代码块减去所有被遮蔽的代码块. 块结构及词法作用域促进了模块化编程，使得程序易读，易维护以及提高可靠性。生成高效的词法作用域的代码是有可能的，because a compiler can determine before program evaluation the scope of all bindings and the binding to which each identifier reference resolves. 当然，这并不意味着编译器可以确定每一个变量的值, 因为大多数情况下，在程序运行之前，真实的值并未计算出来。

在大多数语言里，定义一个过程仅仅是简单地将一块代码与一个名字关联起来，块内部的变量无疑就是过程的参数。在一些语言里，一个过程可以定义在另一个过程或块里面，但是只有在封闭的块被执行时才能在内部调用。在另一些语言里，过程只能全局定义。在 Scheme 里，过程可以定义在另一个过程或块里，然后就可以任意次地调用，甚至在外层的块执行已经结束以后。Scheme 的过程一旦定义，就携带了词法上下文（环境）。

此外，并非每一个 Scheme 过程（函数）都需要命名。过程是第一级对象，就如同普通的字符串或数字一样，将一个变量绑定为普通对象与绑定为一个过程的方式是一样的。

和大多数语言的过程一样，Scheme 的过程可以递归调用。这就是说，任何过程可以直接或间接地调用它自己。很多算法用递归来实现更优美或高效。尾递归是递归的一种特例，它用于表达迭代（循环）。尾调用发生在当一个过程将另一个过程的调用作为自己的返回值直接返回给调用者（真够绕）；而尾递归则是一个过程递归地，直接或间接地对自己进行尾调用。一个 Scheme 实现必须将尾调用实现为跳转（goto），所以，避免了通常与递归联系在一起的内存开销。结果就是，Scheme 程序员只需要考虑简单的过程调用及递归，而不用被各式各样的循环结构所困扰。

Scheme 支持通过 *continuations* 定义任意的控制结构。continuation 体现的是在一个程序中给定的位置“接下来”要做的事。continuation 程序执行中的任意时刻捕获。和其它过程一样，continuation 也是第一级对象，可以任意次地调用。无论何时被调用，程序立即从捕获 continuation 的点继续执行。通过 continuation 可以实现复杂的控制结构，包括回溯，多线程及协程。

Scheme 允许程序员自己定义新的语法形式（语法扩展），通过编写转换程序，确定每一个新的语法形式如何印射到现在有语法形式。这些转换程序本身和一个自动执行语法检查的高级模式语言本身就用 Scheme 来表示的。转换程序解析输入并重建输出。缺省情况下，转换程序维持词法作用域（卫生宏），但是程序员可以控制宏展开后出现的所有标识符的作用域。语法扩展可以定义出有用的语法结构，比如模仿其它语言里的语法结构，获取内联代码的高效率，甚至用 Scheme 虚拟一整个别的语言。大多数规模比较大的 Scheme 程序都混合使用过程定义及宏定义。

Scheme 从 Lisp 语言进化而来，而且被认为是 Lisp 的方言之一。Scheme 从 Lisp 那里继承了将值作为第一级对象处理的方式，还有几种重要的数据类型，包括符号，列表，and the representation of programs as objects, among other things. 词法作用域及块结构继承自 Algol 60[[21]](../ref.md)。在 Lisp 的方言中，Scheme 第一个支持词法作用域，块结构，第一级过程，尾递归优化，continuation，以及词法限定的语法扩展（卫生宏）。

Common Lisp [[27]](../ref.md) 和 Scheme 是同时代的 Lisp 方言, 并且相互受到对方的影响. 和 Scheme 一样而不同于早期的 Lisp 语言, Common Lisp 采用词法作用域及第一级过程, 尽管 Common Lisp 的宏并不遵守词法作用域。Common Lisp 对过程的求值规则与其它对象的求值规则并不相同，它为过程变量（函数名）维护了一个独立的命名空间，从而限制了将过程作为第一级对象使用。另外，Common Lisp 不支持 continuation 及对尾调用作适当的处理（优化为goto, Common Lisp 语言规范并未做此要求，但很多 CL 实现自行提供了尾递归优化支持），但是它支持几种 Scheme 中没有的控制结构。两种语言是相似的，Common Lisp 包含更多的专门的结构，while Scheme includes more general-purpose building blocks out of which such constructs (and others) may be built.

本章剩下的部分描述 Scheme 的语法及命名约定，以及在本书中贯穿使用的排版约定。