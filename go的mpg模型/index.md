# Go的MPG模型


在操作系统提供的内核线程之上，Go搭建了一个特有的两级线程模型。goroutine机制实现了M:N的线程模型，goroutine机制是协程（coroutine）的一种实现，golang内置的调度器，可以让多核CPU中每个CPU执行一个协程。

### 调度器是如何工作的

有了上面的认识，我们可以开始真正的介绍Go的并发机制了，先用一段代码展示一下在G0语言中新建一个“线程“（Go语言中称为Goroutine）的样子：

```go
//1用go关键字加上一个函数（这里用了匿名函数）
//调用就做到了在一个新的“线程”并发执行任务
go func() {

}()
```

理解goroutine机制的原理，关键是理解Go语言`scheduler`的实现。

Go语言中支撑整个scheduler实现的主要有4个重要结构，分别是<u>M、P、G</u>、Sched，前三个定义在runtime.h中，Sched定义在proc.c中。

