# 事件循环（EventLoop）

## 技术背景

javascript是一门单线程语言，虽然在html5中提出了Web-Woker，但这个没有改变javascript是单线程的核心，其原因是javascript是为了协调事件、用户交互、UI渲染和网络处理等行为所创建出来的脚本，所以用户引擎必须使用event loop；（如果多线程，会带来复杂的同步问题，比如同时操作同一个DOM）；

其Event loop分为两类：一类是基于Browsing Context（浏览器），另一种是基于Worker，两者是独立运行的

## 技术术语

任务队列：所有任务都可以为分同步任务和异步任务，其中同步任务一般会直接进入到主线程中执行；而异步任务，就是异步执行的任务；异步任务会通过任务队列（Event Queue）的机制进行协调；

## 定义

同步和异步任务分别进入不同的环境，同步的进入到主线程，即主执行栈，异步的进入Event Queue。当主线程内的任务执行完毕，就会去Event Queue中读取对应的任务，推入主线程执行。此过程不断重复就是Event Loop

## 任务执行流程

在事件循环中，每进行一次循环操作称为tick，每次tick的任务处理模型是比较复杂的，其关键步骤如下；

1. 在此次tick中选择最先进入任务队列的任务（oldest tack），如果有则执行
2. 检查是否存在Microtasks（微任务），如果存在则不停执行，直至清空Microtask Queue
3. 更新render
4. 主线程重复上述步骤

