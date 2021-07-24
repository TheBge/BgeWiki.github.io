## type_info

type_info类在头文件<`typeinfo`>中定义，代表了一个C++类型的相关信息。一般由typeid操作符返回，不能自己构造。

下面的代码可以打印出int类型的名字：

```c++
const std::type_info &tiInt = typeid(int);
std::cout << "tiInt.name = " << tiInt.name() << std::endl;
```

type_info有下列方法：

name()，返回类型的名字
hash_code()，返回这个类型的哈希值（具有唯一性）
before()，可以判断一个type_info对象的顺序是否在另一个之前（实现相关，同一个程序多次调用都可能不一样，不太理解有什么实际作用）
==和!=操作符，判断两个type_info相等或不等

## typeid

查询类型的信息。

用于必须知晓多态对象的动态类型的场合以及静态类型鉴别。

typeid操作符在`<typeinfo>`中声明，用来在运行时获取类型、变量、表达式的类型信息，适用于C++基础类型、内置类、用户自定义类、模板类等。

它有两种形式：

- typeid( 类型 )
- typeid( 表达式 )



## type_index

type_index类在头文件`<typeindex>`中声明，它是type_info对象的一个封装类，可以用作关联容器（比如map）和无序关联容器（比如unordered_map）的索引。

