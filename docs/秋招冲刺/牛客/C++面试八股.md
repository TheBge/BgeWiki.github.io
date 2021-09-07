https://mp.weixin.qq.com/s/qhFERQ1odNkgZS0PkHm1EA



## C++新特性

智能指针

auto、decltype

左值引用、右值引用

移动语义和完美转发

enum和enum class有什么区别

function、bind、lambda使用场景

share_ptr和unique_ptr有什么区别

unique_ptr如何转移所有权

mutex、RAII lock

一般什么情况下会出现内存泄漏？出现内存泄漏怎么调试



## STL

使用过的标准库

list的使用场景

string的常用函数

deque的底层数据结构是什么？

map和unordered_map有什么区别？使用场景

C++直接使用数组好还是array好？array是怎么实现的？

vector的最大特点？内部实现？resize和reserve的区别是什么？clear实现？

push_back/emplace_back



## 设计模式

如果你能知道所有的设计模式最好，即便不知道所有的设计模式，下面这些高频设计模式你还是需要了解的。

1. 分别写出饿汉和懒汉线程安全的单例模式
2. 说出观察者模式类关系和优点
3. 说出代理模式类关系和优点
4. 说出工厂模式概念和优点
5. 说出构造者模式概念
6. 说出适配器模式概念



## 操作系统

1. 进程和线程的区别？[深度好文|面试官：进程和线程，我只问这19个问题](http://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247484716&idx=1&sn=d9eb61162dab2c6da9b9f153fe4319b4&chksm=c21d3790f56abe8682f52b11ed9a65c7138a4942f34336d3745355d44fffd535eadb13d2b9a4&scene=21#wechat_redirect)

2. 操作系统是怎么进行进程管理的？

   进程控制块(PCB)（系统为了管理进程设置的一个专门的数据结构，用它来记录进程的外部特征，描述进程的运动变化过程。系统利用PCB来控制和管理进程，所以PCB是系统感知进程存在的唯一标志。进程与PCB是一一对应的）

3. 操作系统是如何做到进程阻塞的？

4. 进程之间的通信方式有哪些？

    [进程间通信方式.md](..\操作系统\进程线程\进程间通信方式.md) 

5. 什么是上下文切换，操作系统是怎么做的上下文切换？

6. 线程是如何实现的？

7. 线程之间私有和共享的资源有哪些？

8. 一般应用程序内存空间的堆和栈的区别是什么？

9. 进程虚拟空间是怎么布局的？[10张图22段代码，万字长文带你搞懂虚拟内存模型和malloc内部原理](http://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247484726&idx=1&sn=18d9dc7f8b76a2a9a0b29b39ff6dabea&chksm=c21d378af56abe9c56f3d4da55b4d2d90995bae0e1a4a13c3e5cf7f33473d2642615b445aeb1&scene=21#wechat_redirect)

10. 虚拟内存是如何映射到物理内存的？了解分页内存管理吗？[操作系统内存管理，你能回答这8个问题吗？](http://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247484706&idx=1&sn=f99afd2c8c38d97d0e4d333f9c821143&chksm=c21d379ef56abe881bc15e963ea0a8417fb2e80ca022d86024be1d9dbe6e6009396520031e0d&scene=21#wechat_redirect)

11. 产生死锁的必要条件有哪些？如何避免死锁？

12. 什么是大端字节，什么是小端字节？如何转换字节序？

13. 信号和信号量的区别是什么？

14. 锁的性能开销，锁的实现原理？

15. 了解CPU的Cache吗？如何写一个高效的多维矩阵乘法？[如何利用CPU Cache写出高性能代码，看这些图就够了！](http://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247484740&idx=1&sn=d8fc981374061ed8d75b9e29cd6d16da&chksm=c21d37f8f56abeee9cc7594ab57bc3694984f00a363ad4cbbd5f0ecc5554628e793eed25a120&scene=21#wechat_redirect)

