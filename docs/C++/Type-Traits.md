### C++ Traits

​		C++模板中的类型参数T是抽象的，我们并不能在模板内部直接获得它的具体特征。**类型萃取（抽取）技术就是要抽取类型的一些具体特征(trait)，比如它是哪种具体类型，它是引用类型，内建类型，还是类类型等。**可见，类型萃取技术其实就是trait模板技术的具体体现。获取类型的具体特征在Java、C#等语言中也称为反射(reflection)，C++中通过模板技术也可以实现一定的反射行为。

​		类型信息是编译期的实体，现在要针对类型来进行编程，这其实就是模板元编程的一个方面。我们平常使用的if/else，while，for等基本的逻辑结构都是运行期的行为，在面向类型的编程中并不能使用，这就需要用到一些特殊的模板技术。实现类型萃取要用到的基本思想一个是特化，一个就是用typedef来携带类型信息。实际上，我们在用模板做设计时，一般建议在模板定义内部，为模板的每个类型参数提供typedef定义，这样在泛型代码中可以很容易地访问或抽取这些类型。



​		迭代器有常见有五种类型: value_type, difference_type, reference_type, pointer_type都比较容易在 traits 和 相应偏特化中提取。但是，iterator_category一般也有5个，这个相应型别会引发较大规模的写代码工程。

​		例如，我们实现了 func_II, func_BI, func_RAI 分别代表迭代器类型是Input Iterator，Bidirectional Iterator和Random Access Iterator的对应实现。

​		现在，当客户调用func()的时候，我们可能需要做一个判断：

```c++
template<class Iterator>
void func(Iterator& i) {
    if (is_random_access_iterator(i))
        func_RAI(i);
    if (is_bidirectional_iterator(i))
        func_BI(i);
    else
        func_II(i);
}
```

​		但这样在执行时期才决定使用哪一个版本，会**影响程序效率**。最好能够在编译期就选择正确的版本。

```c++
1 template<class Iterator>
2 inline void func(Iterator& i)
3 {
4     typedef typename Iterator_traits<Iterator>::iterator_category category;
5     __func(i, category()); // 各型别的重载
6 }
```



traits一方面，在面对不同的输入类时，能找到合适的返回型别；另一方面，当型别对应有不同的实现函数的时候，能起到一个提取型别然后分流的作用。



### Reference

https://www.cnblogs.com/mangoyuan/p/6446046.html

