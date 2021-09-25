### shared_ptr

​		shared_ptr实现共享式拥有概念。多个智能指针可以指向相同对象，该对象和其相关资源会在“最后一个引用被销毁”时候释放。从名字share就可以看出了资源可以被多个指针共享，它使用计数机制来表明资源被几个指针共享。可以通过成员函数use_count()来查看资源的所有者个数。除了可以通过new来构造，还可以通过传入auto_ptr, unique_ptr,weak_ptr来构造。当我们调用release()时，当前指针会释放资源所有权，计数减一。当计数等于0时，资源会被释放。



### 初始化

```c++
#include<iostream>
#include<memory>
 
int main(){
    std::shared_ptr<int> p1(new int(1)); //方式1
    std::shared_ptr<int> p2 = p1; //方式2
    std::shared_ptr<int> p3; 
//方式3 reset，如果原有的shared_ptr不为空，会使原对象的引用计数减1
    p3.reset(new int(1));
       //方式4
    std::shared_ptr<int> p4 = std::make_shared<int>(2);

//使用方法例子：可以当作一个指针使用
    std::cout << *p4 << std::endl;
    //std::shared_ptr<int> p4 = new int(1);
    if(p1) { //重载了bool操作符
        std::cout << "p is not null" << std::endl;
    }
    int* p = p1.get();//获取原始指针 
    std::cout << *p << std::endl; 
}

```



### 实现

1. 一个模板指针T* ptr，指向实际的对象。

2. 一个引用次数(必须new出来的，不然会多个shared_ptr里面会有不同的引用次数而导致多次delete)。

3. **重载operator*和operator->**，使得能像指针一样使用shared_ptr。

4. **重载copy constructor**，使其引用次数加一。

5. **重载operator=**，如果原来的shared_ptr已经有对象，则让其引用次数减一并判断引用是否为零(是否调用delete)。

　然后将新的对象引用次数加一。

6. **重载析构函数**，使引用次数减一并判断引用是否为零(是否调用delete)。

```c++
template <typename T>
class shared_ptr 
{
    public:
        shared_ptr(T* p) : count(new int(1)), _ptr(p) {}
    	shared_ptr(shared_ptr<T>& other) : count(&(++*other.count)), _ptr(other._ptr) {}
        shared_ptr<T>& operator=(const shared_ptr<T>& other)
        {
            ++*other.count;
            if (this->_ptr && 0 == --*this->count)
            {
                delete count;
                delete _ptr;
            }
            this->_ptr = other._ptr;
            this->count = other.count;
            return *this;
        }
        ~shared_ptr()
        {
            if (--*count == 0)
            {
                delete count;
                delete _ptr;
            }
        }
    
        T* operator->() { return _ptr; }
        T& operator*() { return *_ptr; }
    
        int getRef() { return *count; }
    
    private:
        int* count;
        T* _ptr;
};
```



#### 循环引用

```c++
define _CRT_SECURE_NO_WARNINGS 1

#include<iostream>
#include<cstdlib>
#include<boost/shared_ptr.hpp>

using namespace std;

struct Node
{

    ~Node()
    {
        cout << "析构函数" << endl;
    }
    int data;
    boost::shared_ptr<Node> next;
    boost::shared_ptr<Node> prev;
};

void test1()
{
    boost::shared_ptr<Node > sp1(new Node);
    boost::shared_ptr<Node > sp2(new Node);
    sp1->next = sp2;
    sp2->prev = sp1;
}

int main()
{
    test1();
    system("pause");
    return 0;
}
```

​		由于先构造的后释放，后构造的先释放可知，先释放的是sp2，那么因为它的引用计数为2，减去1之后就成为了1，不能释放空间，因为还有其他的对象在管理这块空间。但是sp2这个变量已经被销毁，因为它是栈上的变量，但是sp2管理的堆上的空间并没有释放。接下来释放sp1，同样，先检查引用计数，由于sp1的引用计数也是2，所以减1后成为1，也不会释放sp1管理的动态空间。通俗点讲：就是sp2要释放，那么必须等p1释放了，而sp1要释放，必须等sp2释放，所以，最终，它们两个都没有释放空间。

**强引用和弱引用**

​		一个强引用当被引用的对象活着的话，这个引用也存在（就是说，当至少有一个强引用，那么这个对象就不能被释放）。share_ptr就是强引用。相对而言，弱引用当引用的对象活着的时候不一定存在。仅仅是当它存在的时候的一个引用。弱引用并不修改该对象的引用计数，这意味这弱引用它并不对对象的内存进行管理，在功能上类似于普通指针，然而一个比较大的区别是，弱引用能检测到所管理的对象是否已经被释放，从而避免访问非法内存。



### 线程安全

**（1）从引用计数的角度来看：**

虽然引用计数存在于每一个shared_ptr对象中，但是实际上它是要跟随对象所管理的资源。引用计数会随着指向这块资源的shared_ptr对象的增加而增加。因此引用计数是要指向同一块资源的所有的对象共享的，所以实际上引用计数在shared_ptr底层中是以指针的形式实现的，所有的对象通过指针访问同一块空间，从而实现共享。

那么也就是说，引用计数是一个临界资源，所以在多线程中，我们必须要保证临界资源访问的安全性，因此在shared_ptr底层中在对引用计数进行访问之前，首先对其加锁，当访问完毕之后，在对其进行解锁。

所以shared_ptr的引用计数是线程安全的。

**（2）从被shared_ptr对象所管理的资源来看：**

shared_ptr对象所管理的资源存放在堆上，它可以由多个shared_ptr所访问，所以这也是一个临界资源。因此当多个线程访问它时，会出现线程安全的问题。

首先shared_ptr对象有两个变量，一个是指向的对象的指针，还有一个就是我们上面看到的引用计数， 当shared_ptr发生拷贝的时候，是先拷贝智能指针，然后再拷贝引用计数，也就是说，**shared_ptr的拷贝并不是一个原子操作**。而问题就出现在这里。



### Reference

https://blog.csdn.net/weixin_43850474/article/details/106328641

https://blog.csdn.net/qq_34992845/article/details/69218843

