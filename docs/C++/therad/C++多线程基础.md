### 创建线程

首先要引入头文件**#include<thread>，**管理线程的函数和类在该头文件中声明，其中包括std::thread类。

语句"std::thread th1(proc1);"创建了一个名为th1的线程，并且线程th1开始执行。

实例化std::thread类对象时，至少需要传递函数名作为参数。如果函数为有参函数,如"void proc2(int a,int b)"，那么实例化std::thread类对象时，则需要传递更多参数，参数顺序依次为函数名、该函数的第一个参数、该函数的第二个参数，···，如"std::thread th2(proc2,a,b);"。这里的传参，后续章节还会有详解与提升。

**只要创建了线程对象**（前提是，实例化std::thread对象时传递了“函数名/可调用对象”作为参数），**线程就开始执行。**

**总之，使用C++线程库启动线程，可以归结为构造std::thread对象。**



### 互斥量（锁）使用

**什么是互斥量（锁）？**

这样比喻：单位上有一台打印机（共享数据a），你要用打印机（线程1要操作数据a），同事老王也要用打印机(线程2也要操作数据a)，但是打印机同一时间只能给一个人用，此时，规定不管是谁，在用打印机之前都要向领导申请许可证（lock），用完后再向领导归还许可证(unlock)，许可证总共只有一个,没有许可证的人就等着在用打印机的同事用完后才能申请许可证(阻塞，线程1lock互斥量后其他线程就无法lock,只能等线程1unlock后，其他线程才能lock)。**那么，打印机就是共享数据，访问打印机的这段代码就是临界区，这个必须互斥使用的许可证就是互斥量（锁）**。

互斥量是为了解决数据共享过程中可能存在的访问冲突的问题。这里的互斥量保证了使用打印机这一过程不被打断。



### 死锁

多线程编程时要考虑多个线程同时访问共享资源所造成的问题，因此可以通过加锁解锁来保证同一时刻只有一个线程能访问共享资源；使用锁的时候要注意，不能出现死锁的状况；

**死锁**就是多个线程争夺共享资源导致每个线程都不能取得自己所需的全部资源，从而程序无法向下执行。



**产生死锁的四个必要条件（面试考点）：**

1. 互斥（资源同一时刻只能被一个进程使用）
2. 请求并保持（进程在请资源时，不释放自己已经占有的资源）
3. 不剥夺（进程已经获得的资源，在进程使用完前，不能强制剥夺）
4. 循环等待（进程间形成环状的资源循环等待关系）



**死锁预防：**

破坏死锁产生的四个条件（完全杜绝死锁）

**死锁避免：**

对分配资源做安全性检查，确保不会产生循环等待（银行家算法）

**死锁检测：**

允许死锁的发生，但提供检测方法

**死锁解除：**

已经产生了死锁，强制剥夺资源或者撤销进程



### **临界区、信号量、互斥量（锁）的区别与联系：**

三者都可用来进行进程的同步与互斥；

**临界区**速度最快，但只能作用于同一进程下不同线程，不能作用于不同进程；临界区可确保某一代码段同一时刻只被一个线程执行；

EnterCriticalSection（） 进入临界区

LeaveCriticalSection（） 离开临界区

**信号量**多个线程同一时刻访问共享资源，进行线程的计数，确保同时访问资源的线程数目不超过上限，当访问数超过上限后，不发出信号量；

P操作 申请资源

V操作 释放资源

**互斥量**（锁）比临界区满，但支持不同进程间的同步与互斥；



### **同步与互斥**

任务运行时，有些任务片段间存在严格的先后顺序，**同步指维护任务片段的先后顺序;**

直观的表现就是若A片段执行完才能执行B片段，线程1执行A片段，线程2执行B片段，在B片段执行前申请锁l，在A片段执行结束后解锁l；未申请到锁l即A片段还未执行完，线程1等待线程2执行。A片段解锁了B片段才能申请到锁，保证了A片段执行结束了B片段才能运行，称之为同步；

**互斥就是保证资源同一时刻只能被一个进程使用；**互斥是为了保证数据的一致性，如果A线程在执行计算式A的时候，某个量被B线程改掉了，这可能会出现问题，于是要求资源互斥，我在用它你就不能用，等我用完了你再用，我们彼此互不干扰。



### **读写锁**

shared_mutex读写锁把对共享资源的访问者划分成读者和写者，多个读线程能同时读取共享资源，但只有一个写线程能同时读取共享资源

shared_mutex通过lock_shared，unlock_shared进行读者的锁定与解锁；通过lock，unlock进行写者的锁定与解锁。

```cpp
shared_mutex s_m;

std::string book;

void read()
{
	s_m.lock_shared();
	cout << book;
	s_m.unlock_shared();
}

void write()
{
	s_m.lock();
	book = "new context";
	s_m.unlock();
}
```



### **自旋锁**

如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁；

自旋锁比较适用于锁使用者保持锁时间比较短的情况。



### 互斥量（锁）怎么使用？

首先需要**#include<mutex>**；（std::mutex和std::lock_guard都在<mutex>头文件中声明。）

然后需要实例化std::mutex对象；

需要在进入临界区之前对互斥量加锁，退出临界区时对互斥量解锁；

**lock()与unlock():**

```text
#include<iostream>
#include<thread>
#include<mutex>
using namespace std;
mutex m;//实例化m对象，不要理解为定义变量
void proc1(int a)
{
    m.lock();
    cout << "proc1函数正在改写a" << endl;
    cout << "原始a为" << a << endl;
    cout << "现在a为" << a + 2 << endl;
    m.unlock();
}

void proc2(int a)
{
    m.lock();
    cout << "proc2函数正在改写a" << endl;
    cout << "原始a为" << a << endl;
    cout << "现在a为" << a + 1 << endl;
    m.unlock();
}
int main()
{
    int a = 0;
    thread t1(proc1, a);
    thread t2(proc2, a);
    t1.join();
    t2.join();
    return 0;
}
```

需要在进入临界区之前对互斥量加锁lock，退出临界区时对互斥量解锁unlock；当一个线程使用特定互斥量锁住共享数据时，其他的线程想要访问锁住的数据，都必须等到之前那个线程对数据进行解锁后，才能进行访问。

程序实例化mutex对象m,本线程调用成员函数m.lock()会发生下面 2 种情况： (1)如果该互斥量当前未上锁，则本线程将该互斥量锁住，直到调用unlock()之前，本线程一直拥有该锁。 (2)如果该互斥量当前被其他线程锁住，则本线程被阻塞,直至该互斥量被其他线程解锁，此时本线程将该互斥量锁住，直到调用unlock()之前，本线程一直拥有该锁。

不推荐实直接去调用成员函数lock()，因为如果忘记unlock()，将导致锁无法释放，使用lock_guard或者unique_lock则能避免忘记解锁带来的问题。



#### **lock_guard:**

std::lock_guard()是什么呢？它就像一个保姆，职责就是帮你管理互斥量，就好像小孩要玩玩具时候，保姆就帮忙把玩具找出来，孩子不玩了，保姆就把玩具收纳好。

其原理是：声明一个局部的std::lock_guard对象，在其构造函数中进行加锁，在其析构函数中进行解锁。最终的结果就是：创建即加锁，作用域结束自动解锁。从而使用std::lock_guard()就可以替代lock()与unlock()。

**通过设定作用域，使得std::lock_guard在合适的地方被析构**（在互斥量锁定到互斥量解锁之间的代码叫做临界区（需要互斥访问共享资源的那段代码称为临界区），临界区范围应该尽可能的小，即lock互斥量后应该尽早unlock），**通过使用{}来调整作用域范围，可使得互斥量m在合适的地方被解锁**：

```c++
#include<iostream>
#include<thread>
#include<mutex>
using namespace std;
mutex m;//实例化m对象，不要理解为定义变量
void proc1(int a)
{
    lock_guard<mutex> g1(m);//用此语句替换了m.lock()；lock_guard传入一个参数时，该参数为互斥量，此时调用了lock_guard的构造函数，申请锁定m
    cout << "proc1函数正在改写a" << endl;
    cout << "原始a为" << a << endl;
    cout << "现在a为" << a + 2 << endl;
}//此时不需要写m.unlock(),g1出了作用域被释放，自动调用析构函数，于是m被解锁

void proc2(int a)
{
    {
        lock_guard<mutex> g2(m);
        cout << "proc2函数正在改写a" << endl;
        cout << "原始a为" << a << endl;
        cout << "现在a为" << a + 1 << endl;
    }//通过使用{}来调整作用域范围，可使得m在合适的地方被解锁
    cout << "作用域外的内容3" << endl;
    cout << "作用域外的内容4" << endl;
    cout << "作用域外的内容5" << endl;
}
int main()
{
    int a = 0;
    thread t1(proc1, a);
    thread t2(proc2, a);
    t1.join();
    t2.join();
    return 0;
}
```

#### **unique_lock:**

std::unique_lock类似于lock_guard,只是std::unique_lock用法更加丰富，同时支持std::lock_guard()的原有功能。 使用std::lock_guard后不能手动lock()与手动unlock();使用std::unique_lock后可以手动lock()与手动unlock(); std::unique_lock的第二个参数，除了可以是adopt_lock,还可以是try_to_lock与defer_lock;

try_to_lock: 尝试去锁定，**得保证锁处于unlock的状态**,然后尝试现在能不能获得锁；尝试用mutx的lock()去锁定这个mutex，但如果没有锁定成功，会立即返回，不会阻塞在那里，并继续往下执行；

defer_lock: 始化了一个没有加锁的mutex;



### 条件变量

需要#include<condition_variable>，该头文件中包含了条件变量相关的类，其中包括std::condition_variable类

如何使用？std::condition_variable类搭配std::mutex类来使用，std::condition_variable对象(std::condition_variable cond;)的作用不是用来管理互斥量的，它的作用是用来同步线程，它的用法相当于编程中常见的flag标志（A、B两个人约定flag=true为行动号角，默认flag为false,A不断的检查flag的值,只要B将flag修改为true，A就开始行动）。

类比到std::condition_variable，A、B两个人约定notify_one为行动号角，A就等着（调用wait(),阻塞）,只要B一调用notify_one，A就开始行动（不再阻塞）。

std::condition_variable的具体使用代码实例可以参见文章中“生产者与消费者问题”章节。

wait(locker) :

wait函数需要传入一个std::mutex（一般会传入std::unique_lock对象）,即上述的locker。wait函数会自动调用 locker.unlock() 释放锁（因为需要释放锁，所以要传入mutex）并阻塞当前线程，本线程释放锁使得其他的线程得以继续竞争锁。一旦当前线程获得notify(通常是另外某个线程调用 notify_* 唤醒了当前线程)，wait() 函数此时再自动调用 locker.lock()上锁。

cond.notify_one(): 随机唤醒一个等待的线程

cond.notify_all(): 唤醒所有等待的线程



### 异步线程

需要#include<future>

#### async与future

std::async是一个函数模板，用来启动一个异步任务，它返回一个std::future类模板对象，future对象起到了**占位**的作用（记住这点就可以了），占位是什么意思？就是说该变量现在无值，但将来会有值（好比你挤公交瞧见空了个座位，刚准备坐下去就被旁边的小伙给拦住了：“这个座位有人了”，你反驳道：”这不是空着吗？“，小伙：”等会人就来了“）,刚实例化的future是没有储存值的，但在调用std::future对象的get()成员函数时，主线程会被阻塞直到异步线程执行结束，并把返回结果传递给std::future，即通过FutureObject.get()获取函数返回值。

相当于你去办政府办业务（主线程），把资料交给了前台，前台安排了人员去给你办理（std::async创建子线程），前台给了你一个单据（std::future对象），说你的业务正在给你办（子线程正在运行），等段时间你再过来凭这个单据取结果。过了段时间，你去前台取结果（调用get()），但是结果还没出来（子线程还没return），你就在前台等着（阻塞），直到你拿到结果（子线程return），你才离开（不再阻塞）。

```c++
#include <iostream>
#include <thread>
#include <mutex>
#include<future>
#include<Windows.h>
using namespace std;
double t1(const double a, const double b)
{
 double c = a + b;
 Sleep(3000);//假设t1函数是个复杂的计算过程，需要消耗3秒
 return c;
}

int main() 
{
 double a = 2.3;
 double b = 6.7;
 future<double> fu = async(t1, a, b);//创建异步线程线程，并将线程的执行结果用fu占位；
 cout << "正在进行计算" << endl;
 cout << "计算结果马上就准备好，请您耐心等待" << endl;
 cout << "计算结果：" << fu.get() << endl;//阻塞主线程，直至异步线程return
        //cout << "计算结果：" << fu.get() << endl;//取消该语句注释后运行会报错，因为future对象的get()方法只能调用一次。
 return 0;
}
```



#### shared_future

std::future与std::shard_future的用途都是为了**占位**，但是两者有些许差别。std::future的get()成员函数是转移数据所有权;std::shared_future的get()成员函数是复制数据。 因此： **future对象的get()只能调用一次**；无法实现多个线程等待同一个异步线程，一旦其中一个线程获取了异步线程的返回值，其他线程就无法再次获取。 **std::shared_future对象的get()可以调用多次**；可以实现多个线程等待同一个异步线程，每个线程都可以获取异步线程的返回值。



### **原子类型atomic<>**

原子操作指“不可分割的操作”，也就是说这种操作状态要么是完成的，要么是没完成的，不存在“操作完成了一半”这种状况。互斥量的加锁一般是针对一个代码段，而原子操作针对的一般都是一个变量(操作变量时加锁防止他人干扰)。 std::atomic<>是一个模板类，使用该模板类实例化的对象，提供了一些保证原子性的成员函数来实现共享数据的常用操作。

可以这样理解： 在以前，定义了一个共享的变量(int i=0)，多个线程会用到这个变量，那么每次操作这个变量时，都需要lock加锁，操作完毕unlock解锁，以保证线程之间不会冲突；但是这样每次加锁解锁、加锁解锁就显得很麻烦，那怎么办呢？ 现在，实例化了一个类对象(std::atomic<int> I=0)来代替以前的那个变量（这里的对象I你就把它看作一个变量，看作对象反而难以理解了），每次操作这个对象时，就不用lock与unlock，这个对象自身就具有原子性（相当于加锁解锁操作不用你写代码实现，能自动加锁解锁了），以保证线程之间不会冲突。

提到std::atomic<>，你脑海里就想到一点就可以了：std::atomic<>用来定义一个自动加锁解锁的共享变量（“定义”“变量”用词在这里是不准确的，但是更加贴切它的实际功能），供多个线程访问而不发生冲突。

```c++
//原子类型的简单使用
std::atomic<bool> b(true);
b=false;
```

std::atomic<>对象提供了常见的原子操作（通过调用成员函数实现对数据的原子操作）： store是原子写操作，load是原子读操作。exchange是于两个数值进行交换的原子操作。 **即使使用了std::atomic<>，也要注意执行的操作是否支持原子性**，也就是说，你不要觉得用的是具有原子性的变量（准确说是对象）就可以为所欲为了，你对它进行的运算不支持原子性的话，也不能实现其原子效果。一般针对++，–，+=，-=，&=，|=，^=是支持的，这些原子操作是通过在std::atomic<>对象内部进行运算符重载实现的。



### **生产者消费者问题**

生产者-消费者模型是经典的多线程并发协作模型。

生产者用于生产数据，生产一个就往共享数据区存一个，如果共享数据区已满的话，生产者就暂停生产，等待消费者的通知后再启动。

消费者用于消费数据，一个一个的从共享数据区取，如果共享数据区为空的话，消费者就暂停取数据，等待生产者的通知后再启动。

生产者与消费者不能直接交互,它们之间所共享的数据使用队列结构来实现;



```cpp
#include<iostream>
#include<thread>
#include<mutex>
#include<queue>
#include<condition_variable>


using namespace std;

//缓冲区存储的数据类型 
struct CacheData
{
	//商品id 
	int id;
	//商品属性 
	string data;
};

queue<CacheData> Q;
//缓冲区最大空间 
const int MAX_CACHEDATA_LENGTH = 10;
//互斥量，生产者之间，消费者之间，生产者和消费者之间，同时都只能一个线程访问缓冲区 
mutex m;
condition_variable condConsumer;
condition_variable condProducer;
//全局商品id 
int ID = 1;

//消费者动作 
void ConsumerActor()
{
	unique_lock<mutex> lockerConsumer(m);
	cout << "[" << this_thread::get_id() << "] 获取了锁" << endl; 
	while (Q.empty())
	{
		cout <<  "因为队列为空，所以消费者Sleep" << endl; 
		cout << "[" << this_thread::get_id() << "] 不再持有锁" << endl;
		//队列空， 消费者停止，等待生产者唤醒 
		condConsumer.wait(lockerConsumer);
		cout << "[" << this_thread::get_id() << "] Weak, 重新获取了锁" << endl; 
	}
	cout << "[" << this_thread::get_id() << "] "; 
	CacheData temp = Q.front();
	cout << "- ID:" << temp.id << " Data:" << temp.data << endl;
	Q.pop(); 
	condProducer.notify_one();
	cout << "[" << this_thread::get_id() << "] 释放了锁" << endl; 
}

//生产者动作 
void ProducerActor()
{
	unique_lock<mutex> lockerProducer(m);
	cout << "[" << this_thread::get_id() << "] 获取了锁" << endl; 
	while (Q.size() > MAX_CACHEDATA_LENGTH)
	{
		cout <<  "因为队列为满，所以生产者Sleep" << endl; 
		cout << "[" << this_thread::get_id() << "] 不再持有锁" << endl; 
		//对列慢，生产者停止，等待消费者唤醒 
		condProducer.wait(lockerProducer);
		cout << "[" << this_thread::get_id() << "] Weak, 重新获取了锁" << endl; 
	}
	cout << "[" << this_thread::get_id() << "] "; 
	CacheData temp;
	temp.id = ID++;
	temp.data = "*****";
	cout << "+ ID:" << temp.id << " Data:" << temp.data << endl; 
	Q.push(temp);
	condConsumer.notify_one();
	cout << "[" << this_thread::get_id() << "] 释放了锁" << endl; 
}

//消费者 
void ConsumerTask()
{
	while(1)
	{
		ConsumerActor();
	}	
}

//生产者 
void ProducerTask()
{
	while(1)
	{
		ProducerActor();
	}	
}

//管理线程的函数 
void Dispatch(int ConsumerNum, int ProducerNum)
{
	vector<thread> thsC;
	for (int i = 0; i < ConsumerNum; ++i)
	{
		thsC.push_back(thread(ConsumerTask));
	}
	
	vector<thread> thsP;
	for (int j = 0; j < ProducerNum; ++j)
	{
		thsP.push_back(thread(ProducerTask));
	}
	
	for (int i = 0; i < ConsumerNum; ++i)
	{
		if (thsC[i].joinable())
		{
			thsC[i].join();
		}
	}
	
	for (int j = 0; j < ProducerNum; ++j)
	{
		if (thsP[j].joinable())
		{
			thsP[j].join();
		}
	}
}

int main()
{
	//一个消费者线程，5个生产者线程，则生产者经常要等待消费者 
	Dispatch(1,5);
	return 0; 
}
```
