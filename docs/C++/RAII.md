## 1.什么是RAII

RAII（**R**esource **A**cquisition **I**s **I**nitialization）是由c++之父Bjarne Stroustrup提出的，中文翻译为资源获取即初始化，他说：使用局部对象来管理资源的技术称为资源获取即初始化；这里的资源主要是指操作系统中有限的东西如内存、网络套接字等等，局部对象是指存储在栈的对象，它的生命周期是由操作系统来管理的，无需人工介入；



## 2.RAII的原理

资源的使用一般经历三个步骤a.获取资源 b.使用资源 c.销毁资源，但是资源的销毁往往是程序员经常忘记的一个环节，所以程序界就想如何在程序员中让资源自动销毁呢？c++之父给出了解决问题的方案：RAII，它充分的利用了C++语言局部对象自动销毁的特性来控制资源的生命周期。给一个简单的例子来看下局部对象的自动销毁的特性：

```c++
#include <iostream>
using namespace std;
class person {
  public:
      person(const std::string name = "", int age = 0) : 
      name_(name), age_(age) {
            std::cout << "Init a person!" << std::endl;
      }
      ~person() {
            std::cout << "Destory a person!" << std::endl;
      }
      const std::string& getname() const {
            return this->name_;
      }    
      int getage() const {
            return this->age_;
      }      
  private:
      const std::string name_;
      int age_;  
};

int main() {
    person p;
    return 0;
}
```

从person class可以看出，当我们在main函数中声明一个局部对象的时候，会自动调用构造函数进行对象的初始化，当整个main函数执行完成后，自动调用析构函数来销毁对象，整个过程无需人工介入，由操作系统自动完成；于是，很自然联想到，当我们在使用资源的时候，在构造函数中进行初始化，在析构函数中进行销毁。

整个RAII过程总结为四个步骤：

- 设计一个类封装资源
- 在构造函数中初始化
- 在析构函数中执行销毁操作
- 使用时声明一个该对象的类



## 3.简单实例

每当需要处理配对的获取/释放函数调用的资源时，都应该将资源封装在一个类中，实现自动资源释放。

```c++
class FileRAII{  
public:  
    FileRAII(FILE* aFile):file_(aFile){}  
    ~FileRAII() { fclose(file_); }//在析构函数中进行文件关闭  
    FILE* get() {return file_;}  
private:  
    FILE* file_;  
}; 

void Func()  
{  
  FILE *fp;  
  char* filename = "test.txt";  
  if((fp=fopen(filename,"r"))==NULL)  
  {  
      printf("not open");  
      exit(0);  
  }  
  FileRAII fileRAII(fp);  
  ... // 如果 在使用fp指针时产生异常 并退出  
       // 那么 fileRAII在栈展开过程中会被自动释放，析构函数也就会自动地将fp关闭  
 
  // 即使所有代码是都正确执行了，也无需手动释放fp，fileRAII它的生命期在此结束时，它的析构函数会自动执行！      
 } 
```

