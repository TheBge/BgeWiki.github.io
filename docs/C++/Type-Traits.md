萃取机
具体的来说，traits就是通过定义一些结构体或类，并利用模板类特化和偏特化的能力，给类型赋予一些特性，这些特性根据类型的不同而异。在程序设计中可以使用这些traits来判断一个类型的一些特性，引发C++的函数重载机制，实现同一种操作因类型不同而异的效果。

cplusplus： 
<type_traits> 
This header defines a series of classes to obtain type information on compile-time.
The header contains:
Helper classes: Standard classes to assist in creating compile-time constants.
Type traits: Classes to obtain characteristics of types in the form of compile-time constant values.
Type transformations: Classes to obtain new types by applying specific transformations to existing types.

感觉就是用traits来问到各种各样的type  

例：
现在定义一个type_traits可以获得类型的如下属性：
1.是否存在non-trivial default constructor
2.是否存在non-trivial copy constructor 
3.是否存在non-trivial assignment operator
4.是否存在non-trivial destructor

``` c
struct __true_type {
};
struct __false_type {
};

template <class _Tp>
struct __type_traits {

   typedef __false_type    has_trivial_default_constructor;
   typedef __false_type    has_trivial_copy_constructor;
   typedef __false_type    has_trivial_assignment_operator;
   typedef __false_type    has_trivial_destructor;
};
```

```c++
template <>
struct __type_traits<int> {
   typedef __true_type    has_trivial_default_constructor;
   typedef __true_type    has_trivial_copy_constructor;
   typedef __true_type    has_trivial_assignment_operator;
   typedef __true_type    has_trivial_destructor;
};

template <>
struct __type_traits<char> {
   typedef __true_type    has_trivial_default_constructor;
   typedef __true_type    has_trivial_copy_constructor;
   typedef __true_type    has_trivial_assignment_operator;
   typedef __true_type    has_trivial_destructor;
};
```

偏特化版本 
``` c++
template <class _Tp>
struct __type_traits<_Tp*> {
   typedef __true_type    has_trivial_default_constructor;
   typedef __true_type    has_trivial_copy_constructor;
   typedef __true_type    has_trivial_assignment_operator;
   typedef __true_type    has_trivial_destructor;
   typedef __true_type    is_POD_type;
};
```

特化版本，比如自定义类型  
``` c++
struct __type_traits<Shape> {
   typedef __false_type    has_trivial_default_constructor;
   typedef __true_type    has_trivial_copy_constructor;
   typedef __true_type    has_trivial_assignment_operator;
   typedef __true_type    has_trivial_destructor;
   typedef __true_type    is_POD_type;
};
```

假设现在用个模板函数fun需要根据类型T是否有non-trivial constructor来进行不同的操作，可以这样来实现：  
``` c++
template<class T>
void fun()
{
     typedef typename __type_traits<T>::has_trivial_constructor _Trivial_constructor
    __fun(_Trivial_constructor()); // 根据得到的_Trivial_constructor来调用相应的函数
}

// 两个重载的函数
void _fun(_true_type)
{
	cout<<"fun(_true_type)called"<<endl;
}
void _fun(_false_type)
{
	cout<<"fun(_false_type) called"<<endl;
}

//测试代码

int main()
{
    fun<char>();
    fun<int>();
    fun<char *>();
    fun<double>();
}
```