#### OOP(object-oriented programming) vs GP(Generic Programing)  
OOP将datas与methods关联在一起 
GP将datas与methods分开 
使用**GP**：
1 Containers/Algorithms团队各司其职，以Iterator沟通 
2 Algorithms通过Iterators确定操作范围，通过Iterators取用Containers元素 
![upload successful](../../img/pasted-11.png)

#### 阅读C++标准库，基础  
1 Operator Overloading 操作符重载 
2 Templates 模板 
类模板/函数模板/成员模板 
模板特化/偏特化

#### STL 六大部件
容器（Containiers） 
分配器（Allocators） 
算法（Alogrithms） 
迭代器（Iterators） 
适配器（Adapters） 
仿函数（Functors） 
![upload successful](../../img/pasted-8.png)  

#### 容器  
Sequance Containers： 
Array 
Vector 
Deque 
List 
Forward List  

Associative Containers： 
Set/MultiSet 
Map/Multimap 
![upload successful](../../img/pasted-9.png)  

Unordered Containers： 
Unordered Set/MultiSet 
Unordered Map/Multimap 
用hash table实现的
![upload successful](../../img/pasted-10.png) 

![upload successful](../../img/pasted-12.png)

**1 list** 
环状链表，在list的尾端有一空白节点，用以符合STL的前闭后开区间特点。 有iterator。
**2 vector** 
data有3个指针 strat finish end_of_storage（注意标准库的版本），二倍成长的容器。连续空间。有iterator。 
**3 array** 
没有ctor，没有dtor。有iterator。 
**4 forwrad_list** 
线状单项串列 
**5 deque** 
分段连续，双向进出。deque iterator模拟了连续空间。 
**6 stack** 先进先出 **queue** 先进先出 
deque作为底层容器，不允许遍历，也不提供iterator。 
stack和queue都可以选择list或者deque作为底层容器。stack可选vector做底层结构，queue不可选vector做底层结构，都不可选set或map 
**7 rb_tree** 
红黑树，是平衡二元搜寻树常使用的一种，平衡二元搜寻树的特征：排列规则有利于search和inset，并保持适度平衡-无节点过深。 
提供遍历操作和iterator，按正常规则++ite遍历，便能获得排序状态。 
为set和map作其底层支持。
**8 set/multiset** 
以红黑树作为底层结构，因此元素自动排序，排序依据是key，而set/和multiset的value和key合一。 
无法使用iterator改变元素值，set/multiset的iterator是底部红黑树的const iterator，禁止了user对元素的赋值。 
**9 map/multimap** 
以红黑树为底层结构，元素自动排序，排序的依据是key。提供遍历操作以及iterator，按正常规则++ite遍历可得到排序状态。 
无法使用iterator改变元素的key，但是可以改变元素的data，因此map/multimap内部自动将user制定的key_type设定为const,如此来禁止user对元素的key赋值。

```c++
template<class Key, class T, ...>
class map
{
	typedef pair<const Key, T> value_type;
    typedef rb_tree<key_type, value_type, ...> rep_type;
    rep_type t;
    ...
}
```
**10 hashtable** 
有一个buckets vector，每一个bucket都是一个链表，可以使用hashtable iterator改变元素的data，但不能改变key 
元素个数超过buckets数时进行rehashing，新的buckets数是翻倍后最近邻的素数。

#### 迭代器  
Iterator要遵循的原则。必须有能力回答Algorithms的提问，设计了五种特殊的typedef。 
![upload successful](../../img/pasted-13.png)

Iterator Traits 用于分离class iterator和non-class iterators。（模板的特化） 
声明一个无法被赋值的变量没有意义，所以即使是const iterator的value_type也不应该加上const，iterator若是const int*, 那value_type是int而不是const int。  


#### 分配器  
example：  
```c++  
template<typename T, typename _Alloc = std::allocator<T>>
class vector:protected _Vector_base<T, _Alloc>
```

#### 仿函数  
为算法服务，必须重载（）  

#### 适配器Adapter  
仿函数适配器
？bind函数  绑定成员函数时，为什么要加上&