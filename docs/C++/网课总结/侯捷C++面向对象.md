#### (I)C++面向对象开发

**//complex class** 
1.带指针的class/不带指针的class 
2.初始化列表/不使用初始化列表赋值 （头文件与类的声明 
构造函数） 
3.constructor放在private里（参数传递与返回值）
singleton 
4.考虑return by value与return by reference  

**//string class** 
**只要你的类里面带指针，必须有拷贝构造/拷贝赋值。** 
1.调试模式和release模式   



Object Oriented Programming, Object Oriented Design (OOP, OOD) 

Inheritance 继承 
Composition 复合 
Delegation 委托
1.Composition（组合与继承） 
Adapter 
2.pimpl 
pointer to implementation 
3.Inheritance with virtual 
template method 
4.Delegation + Inheritance 
Observer 
Composite
5.Prototype  

#### (II)面向对象开发--兼谈对象模型

**1.non-explicit-one-argument ctor** 
转换函数
关键字 explicit
**2.pointer-like classes** 
share-pointer 
iterator 
**3.function-like classes** 
仿函数 
**4.namespace** 
**5.class template**
**6.function template** 
**7.member template** 
**8.specialization特化** 
**9.模板偏特化** 
**10.template template parameter** 
**11.C++11新特性** 
**12.reference** 
**13.Object model** 
**14.Vptr & Vtbl**
虚函数 虚表 
动态绑定：1通过指针调用 2向上转型 3调用的是虚函数 
多态：

```c
A *p = new B; //A是父类 B是子类
虽然P是一个基类的指针。但是new B调用的是派生类B的构造方法，所以构造的是B类对象。先调用A的构造函数，再调用B的构造函数。构造完后会返回B类对象的地址，然后将它赋给一个基类指针P。
```
**15.const** 
1放在小括号后面大括号前面 修饰成员函数，不改变class的data 
const也算函数签名的一部分 
当成员函数的const和non-const版本同时存在的时候，const object只能只会调用const版本，non-const object只能只会调用non-const版本。 
**16.new & delete** 
new 先分配memory，再调用ctor 
delete 先调用dtor，再释放memory
new和delete都是expression，调用的是operator new/operator delete，底层是调用malloc 
**17.重载operator new/operator delete/operator new【】/operator delete【】** 
**18.placement new()**