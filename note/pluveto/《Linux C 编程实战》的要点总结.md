# 《Linux C 编程实战》的要点总结

## 一到五章

一到五章是基础语法知识和一些基本的函数的用法，如果有其他语言的使用经验，可以略过，需要的时候 Google 一下就行了。

这本书的代码我觉得写得不够好，很多地方都不规范，比如 main 不声明类型，不返回 0，extern 不声明类型等等。大家主要用来看看有哪些 linux 编程需要的函数。

下面都不说具体用法，因为这个东西一查就有了。

第六章笔记我写的不太好，我觉得读书重在理解概念、机制，这些工具函数用到再查也好，所以后面我改进了。

## 第六章 文件

本章讲的是文件的操作

### 最基本的

1. creat - 创建
2. open - 打开
3. read - 读取
4. write - 写入
5. close - 关闭

这几个的作用就是英文字面意思。

而 **lseek** 的作用是移动读取文件的指针。
read和lseek配合使用。`l` 表示的是 long （参考[https://softwareengineering.stackexchange.com/questions/244525/why-is-the-function-called-lseek-not-seek](https://softwareengineering.stackexchange.com/questions/244525/why-is-the-function-called-lseek-not-seek)）

### dup

1. dup - （duplicate）给`file descriptor` 创建别名
2. dup2 - 和 dup 类似，但是可以指定id。

这两个基本初学者用不到，但是它们是有用的，比如 shell 输入输出重定向、进程间通信。可参考：
[http://www.runoob.com/linux/linux-shell-io-redirections.html](http://www.runoob.com/linux/linux-shell-io-redirections.html) 和 [http://mark-shih.blogspot.com/2011/01/dup-dup2.html](http://mark-shih.blogspot.com/2011/01/dup-dup2.html)

### 两个c(n)tl

1. fcntl - 操作普通文件。估计是 file control 的缩写。恕我直言，C 语言的缩写挺烦的，虽然打字方便了，但是大大降低了可读性，后来发明的更高级语言很少这么干。具体用法见：[http://man7.org/linux/man-pages/man2/fcntl.2.html](http://man7.org/linux/man-pages/man2/fcntl.2.html)
2. ioctl - 操作设备文件。你看，前面control用cntl，这里又用ctl，这样简直是乱来。具体用法见：[http://man7.org/linux/man-pages/man2/ioctl.2.html](http://man7.org/linux/man-pages/man2/ioctl.2.html) 这个函数比较实用，可以获取设备信息，比如 IP 地址

### stat, fstat, lstat, fstatat - 获取文件状态

作用是获取文件创建日期、大小、所有者之类的状态（Windows上叫做属性）。

详见 [http://man7.org/linux/man-pages/man2/stat.2.html](http://man7.org/linux/man-pages/man2/stat.2.html)

### ch(ange)系列 - 修改文件状态

1. chmod, fchmod, fchmodat - 改变权限。详见：[http://man7.org/linux/man-pages/man2/chmod.2.html](http://man7.org/linux/man-pages/man2/chmod.2.html)
2. chown, fchown, lchown, fchownat - 改变所有权。详见：[http://man7.org/linux/man-pages/man2/chown.2.html](http://man7.org/linux/man-pages/man2/chown.2.html)
### truncate 
4. truncate, ftruncate - 改变文件大小。truncate 这个单词意思是截短。详见： [http://man7.org/linux/man-pages/man2/ftruncate.2.html](http://man7.org/linux/man-pages/man2/ftruncate.2.html)
### u系列
6. utime, utimes - 改变文件修改时间、最后访问时间。
7. umask - 设置新建文件权限的掩码，比如 0777.
8. 
9. 
### 文件

1. rename - 重命名、移动。
2. remove - 删除文件、目录。本质是 unlink。
3. unlink - 删除文件。
4. 
### 目录

4. mkdir - 创建目录
5. rmdir - 删除目录
6. getcwd - 获取当前目录
7. chdir - 设置工作目录。（什么是工作目录：[https://www.cnblogs.com/nielong/p/5549422.html](https://www.cnblogs.com/nielong/p/5549422.html)）
8. opendir - 打开目录
9. readdir - 读取目录信息
10. closedir - 关闭打开的目录 

## 第七章 进程

进程的状态分为：

1. R  running or runnable (on run queue)
2. D  uninterruptible sleep (usually IO)
3. S  interruptible sleep (waiting for an event to complete)
4. Z  defunct/zombie, terminated but not reaped by its parent
5. T  stopped, either by a job control signal or because it is being traced

记忆口诀（我自己想的）：燃烧的猪蹄（RSDZT）

![](https://idea.popcount.org/2012-12-11-linux-process-states/76a49594323247f21c9b3a69945445ee.svg)

进程的操作可以用到再查。

## 第八章 线程

操作系统通过快速轮流执行几个程序，这样就会产生几个程序同时进行的假象。线程的作用就是这样。

线程的基本操作此处略。

### 互斥锁

作用：互斥锁的作用是对临界区加以保护，以使任意时刻只有一个线程能够执行临界区的代码。实现了多线程之间的互斥。临界区就是代码片段。（[https://blog.csdn.net/qq_33951180/article/details/72801228](https://blog.csdn.net/qq_33951180/article/details/72801228)）

参考：
1. 知乎上的。不过感觉讲得不是太通俗易懂。[https://www.zhihu.com/question/39850927](https://www.zhihu.com/question/39850927)

### 异步信号

#### 什么是异步？

同步：一定要等任务执行完了，得到结果，才执行下一个任务。
异步：不等任务执行完，直接执行下一个任务。
（[https://zhuanlan.zhihu.com/p/22685960](https://zhuanlan.zhihu.com/p/22685960)）
异步信号的作用：进程间异步通信方式。
异步通信又是什么：异步通信中的接收方可以不知道数据什么时候会到达。

举个例子：你打字的时候，如果是同步的方式，则电脑没有处理完一个按键就不能处理下一个按键，也就是你要等他（电脑）。而若是异步方式，则你可以随便打，它只管自己接收你的输入然后一一处理，你不用等他。

参考：
1. 通俗理解同步通信与异步通信 [https://blog.csdn.net/RhythmWANG/article/details/68066584](https://blog.csdn.net/RhythmWANG/article/details/68066584)
2. Linux异步之信号(signal)机制分析 [https://blog.csdn.net/freeking101/article/details/78338497](https://blog.csdn.net/freeking101/article/details/78338497)
3. 什么是阻塞，非阻塞，同步，异步？[https://www.zhihu.com/question/26393784](https://www.zhihu.com/question/26393784)

## 第九章 信号

信号可以理解为**信件**。信号的作用：给其他进程发信息，当然你也会收到其他进程的信息。
信号处理的具体代码略，用到再查。




## 第十章 进程间通信

### 管道方式

特点：1. 单向。2. 亲缘性。（不是父子或兄弟进程不能通信）。具体代码略。

### 消息队列

这个可以理解为你的电子邮箱的信件列表。具体代码略。

### 信号量

表示资源可用量。信号量（Semaphore）不是信号（Signal）。最简单的信号量是只能取0和1的变量，这也是信号量最常见的一种形式，叫做二进制信号量。而可以取多个正整数的信号量被称为通用信号量。（[https://blog.csdn.net/ljianhui/article/details/10243617](https://blog.csdn.net/ljianhui/article/details/10243617)）

信号量就是一个停车场。**当前值**是停车场里还剩下多少个空车位。**最大值**是停车场里最多能容纳多少个车位。

当汽车进入停车场时，首先要在门口**排队**(sem_wait)，得到进入许可后才能进入。

排队顺序原则上**先到先得**。每进一辆车，停车场就少了1个停车位，即信号量当前值-1。当前值为0时，停车场停满了，所有车不得进入统统在门口排队等。当一辆车离开后，释放其所占据的停车位(sem_post)，信号量当前值+1

信号量值得到**释放**后，如果门口有正在排队的车，那么就放进来，每放进来一个就重复前面的步骤。

（https://www.zhihu.com/question/40562993/answer/87204567  ）
  
### 共享内存

共享内存就是能被几个进程一起用的内存。具体代码略。

### 库

库就是一堆函数的封装，可以理解为工具箱，里面放了各种工具（函数），可以随时拿来用。高级语言还有类库，不过C语言没有类，所以C语言的库是函数库。

#### 静态库和动态库有什么区别

静态库就是你每次写个程序都要把工具箱放一个进去，动态库就是你的几个程序用同一个工具箱，用到的时候再去拿，不用每个程序都放一个。
  
## 网络编程

### 基本原理

层：和楼层一样，有了底层才有高层。通过一层层的抽象，可以实现更高级的功能。

#### ISO 七层模型

这个东西很好记，我编了个口诀：巫术网传会嫑用。具体有：

第7层 应用层
第6层 表达层
第5层 会话层
第4层 传输层
第3层 网络层
第2层 数据链路层
第1层 物理层

每一层我们用到再查，不用死记。知乎的人喜欢搞什么“深入理解”，“系统学习”，结果有时候一个小小的知识点扯出几十页纸，连集线器的使用方法都要写进去，我觉得没必要。

#### TCP/IP 四层模型

口诀：连网传用，各层的意思就在口诀里。

1）连接层负责建立**电路连接**，是整个网络的物理基础，典型的协议包括以太网、ADSL等等；

2）网络层负责分配地址和传送**二进制数据**，主要协议是IP协议；

3）传输层负责传送**文本数据**，主要协议是TCP协议；

4）应用层负责传送各种**最终形态的数据**，是直接与用户打交道的层，典型协议是HTTP、FTP等。

（[http://www.ruanyifeng.com/blog/2009/03/tcp-ip_model.html](http://www.ruanyifeng.com/blog/2009/03/tcp-ip_model.html)）

#### 地址

分为物理地址、IP地址。相当于你家的地址。

#### 端口

相当于你家的哪个房间。

IP 协议：略。

### UDP 和 TCP

这个不能略了，因为真的会用到。

UDP 就是我只管发，收不收得到我不在乎。可靠性弱。

TCP 就是先和你联系上了再发，所以可靠性强。

### TCP 的建立、传输与断开

C表示Client，客户端。
S表示Server，服务器。

三次握手：

1. C: 我要连你（客户端给服务端发送请求连接的数据表）
2. S: 好，我准备好了（服务端与客户端建立连接）
3. C: 我发过去了（客户端发送数据）

四次挥手：

4. S/C: 我发完了/我收完了（可以是服务器或客户端）
5. C/S: 行，我同意关闭。
6. S/C: 那我关了。
7. C/S: 你关了那我也关了。

### 套接字编程

套接字其实就是`IP地址：端口号`（我觉得这是个煞笔翻译。建议翻译为“软插口”。）
套接字可以理解为传输数据的通道。具体用法略。

## 图形界面编程

略。