这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

**注意：**

- 单例类只能有一个实例。
- 单例类必须自己创建自己的唯一实例。
- 单例类必须给所有其他对象提供这一实例。

**优点：**

- 内存里只有一个实例，减少了内存开销。（频繁的创建和销毁实例）
- 避免对资源的多重占用。（比如写文件操作）

**缺点：**

- 没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

  

### **C++实现**

#### 懒汉式（lazy-Initialzation）

直到使用时才实例化对象，即直到调用GetInstance时才new一个对象。

```c++
class Singleton
{
    public:
        static Singleton* GetInstance()  //公有静态方法，获取该实例
        {
            if(pInstance == nullptr)  //判断是否第一次调用
                pInstance = new Singleton();
            return pInstance;
        }
    private:
        Singleton();   //将构造函数设为私有
 
        static Singleton *pInstance;  //私有静态指针变量，指向唯一实例
};
```

构造函数是私有的，导致在外部任何尝试创建实例的尝试都会失败。

禁止拷贝和赋值。

唯一方法是通过调用公有静态函数GetInstance，返回的实例是该函数首次被访问时创建的。

系统会在程序结束后释放所有全局变量并析构所有类的静态对象。



#### **饿汉式**

```c++
//Singleton.h
class Singleton 
{
    public:
        static Singleton* getInstance();

    private:
        Singleton();
        Singleton(const Singleton&);
        Singleton& operator=(const Singleton&);

        static Singleton* instance_;
};

//Singleton.cpp
#include "Singleton.h"

Singleton* Singleton::instance_ = new Singleton();

Singleton::Singleton() {}

Singleton* Singleton::getInstance() 
{
    return instance_;
}
```

与懒汉式区别在于，在全局作用域进行单例类的实例化，并且此实例初始化单例类的静态成员指针。



#### **Meyers's Singleton**

```c++
class Singleton
{
    public:
        Singleton(const Singleton&)=delete;
        Singleton& operator=(const Singleton&)=delete;
        ~Singleton(){}；
        static Singleton& GetInstance()
        {
            static Singleton instance;
            return instance;
        }
    private:
        Singleton()；
};
```

```C++
static Singleton* GetInstance()
{
    static Singleton instance;
    return &instance;
}
```

**线程安全的单例模式**

双重校验锁

	static SingleTon *getInstance(){
	 
		if(m_instance == nullptr){
	 
			mt.lock();
			if(m_instance == nullptr){
	 
				m_instance = new SingleTon;
				mt.unlock();
				return m_instance;
			}
		}
		mt.unlock();
		return m_instance;
	}

**How should it be used**

You need to have one and only one object of a type in system
你需要系统中只有**唯一**一个实例存在的类的**全局**变量的时候才使用单例。

**How to create the best singleton:**

- The smaller, the better. I am a minimalist
- Make sure it is thread safe
- Make sure it is never null
- Make sure it is created only once
- Lazy or system initialization? Up to your requirements
- Sometimes the OS or the JVM creates singletons for you (e.g. in Java every class definition is a singleton)
- Provide a destructor or somehow figure out how to dispose resources
- Use little memory
  越小越好，越简单越好，线程安全，内存不泄露

**“Singleton仅仅是一个包装好的全局变量，那你能说说它和全局变量的相同与不同么？”**

单例可以说是全局变量的替代品。其拥有全局变量的众多特点：

​		全局可见且贯穿应用程序的整个生命周期。除此之外，单例模式还拥有一些全局变量所不具有的性质：同一类型的对象实例只能有一个，同时适当的实现还拥有延迟初始化（Lazy）的功能，可以避免耗时的全局变量初始化所导致的启动速度不佳等问题。要说明的是，Singleton的最主要目的并不是作为一个全局变量使用，而是保证类型实例有且仅有一个。它所具有的全局访问特性仅仅是它的一个副作用。但正是这个副作用使它更类似于包装好的全局变量，从而允许各部分代码对其直接进行操作。软件开发人员需要通过仔细地阅读各部分对其进行操作的代码才能了解其真正的使用方式，而不能通过接口得到组件依赖性等信息。如果Singleton记录了程序的运行状态，那么该状态将是一个全局状态。各个组件对其进行操作的调用时序将变得十分重要，从而使各个组件之间存在着一种隐式的依赖。

**对象释放问题**

程序中只有new没有delete

A：一般情况下单例模式的实例是常驻内存的，因此不需要手动释放，如果的确需要释放实例占用的内存，一定不能在单例内的析构函数中进行delete操作，这样会造成无限循环。

如果在类的析构函数中有必要的操作，例如关闭文件，释放外部资源，需要正常删除实例。

bad idea：可以在程序结束时调用GetInstance，然后delete，但是delete之后无法保证后面还有没有人调用GetInstance。

一个妥善的方法是让这个类自己知道在合适的时候把自己删除，或者说把删除自己的操作挂在操作系统中的某个合适的点上，使其在恰当的时候被自动执行。

程序在结束的时候，系统会自动析构所有的全局变量。事实上，系统也会析构所有的类的静态成员变量，就像这些静态成员也是全局变量一样。利用这个特征，我们可以在单例类中定义一个这样的静态成员变量，而它的唯一工作就是在析构函数中删除单例类的实例。如下面的代码中的CGarbo类（Garbo意为垃圾工人）：

```c++
class CSingleton  
{  
private:  
    CSingleton()  
    {  
    }  
    static CSingleton *m_pInstance;  
    class CGarbo   //它的唯一工作就是在析构函数中删除CSingleton的实例  
    {  
    public:  
        ~CGarbo()  
        {  
            if(CSingleton::m_pInstance)  
                delete CSingleton::m_pInstance;  // delete
        }  
    };  
    static CGarbo Garbo;  //定义一个静态成员变量，程序结束时，系统会自动调用它的析构函数  
public:  
    static CSingleton * GetInstance()  
    {  
        if(m_pInstance == NULL)  //判断是否第一次调用  
            m_pInstance = new CSingleton();  
        return m_pInstance;  
    }  
};  
```

**Reference：**

https://www.cnblogs.com/leaves1024/p/10985599.html

https://www.cnblogs.com/faith0217/articles/4083476.html

https://zhuanlan.zhihu.com/p/37469260