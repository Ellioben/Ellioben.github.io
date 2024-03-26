# Go的MPG模型


在操作系统提供的内核线程之上，Go搭建了一个特有的两级线程模型。goroutine机制实现了M:N的线程模型，goroutine机制是协程（coroutine）的一种实现，golang内置的调度器，可以让多核CPU中每个CPU执行一个协程。

### 调度器是如何工作的

有了上面的认识，可以开始真正的介绍Go的并发机制了，先用一段代码展示一下在Go语言中新建一个“线程“（Go语言中称为Goroutine）的样子：

```go
//1用go关键字加上一个函数（这里用了匿名函数）
//调用就做到了在一个新的“线程”并发执行任务
go func() {

}()
```

理解goroutine机制的原理，关键是理解Go语言`scheduler`的实现。

Go语言中支撑整个scheduler实现的主要有4个重要结构，分别是<u>M、P、G</u>、Sched，前三个定义在runtime.h中，Sched定义在proc.c中。



> MPG：都是go语言运行时系统抽象出来的概念和数据结构的对象，包括内存分配器，并发调度器，垃圾收集器等等（可以理解为Java的jvm）

- M结构是Machine，系统线程，它由操作系统管理的，goroutine就是跑在M之上的；M是一个很大的结构，里面维护小对象内存cache（ncache）、当前执行的goroutine、随机数发生器等等非常多的信息。

  M是Machine的缩写 ，利用系统调用创建的操作系统线程实体，作用是执行G当中包装的并发任务，<u><font color='#44A58C' style="font-weight:bold">可以理解为一个M代表一个内核线程，它是由操作系统来管理的。goroutine就跑在M上面的。</font></u>

- G是goroutine实现的核心结构，它包含了栈，指令指针，以及其他对调度goroutine很重要的信息，例如其阻塞的channel。

  G是goroutine的缩写，go关键字+函数就可以创建G的对象，是对一个并发任务的封装，可以认为是用户态的线程，属于用户资源，对操作系统是透明的，属于轻量级资源。可以大量创建，上下文切换成本比较低

- P结构是Processor，（逻辑）处理器，它的主要用途就是用来执行goroutine的，它维护了一个`goroutine队列，即runqueue`。Processor是让我们从N:1调度到M:N调度的重要部分。

  P是Processor的缩写，主要是管理G对象，并且<u><font color='#44A58C' style="font-weight:bold">为G在M上运行提供一些本地化的资源，主要用途就是用来执行goroutine的</font>font></u>。P可以理解为执行一个go代码片段所必需的资源，所以有一些资料会叫上下文环境。*Processor的数量是在启动时被设置为环境变量GOMAXPROCS的值，或者通过运行时调用函数GOMAXPROCS0进行设置。Processor数量固定意味着任意时刻只有GOMAXPROCS个线程在运行go代码*.

  

- Sched:结构就是调度器，它维护有存储M和G的队列以及调度器的一些状态信息等等。


