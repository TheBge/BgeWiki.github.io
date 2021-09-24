### typeid运算符

查询类型的信息，用于必须知晓多态对象的动态类型的场合以及静态类型鉴别。

返回类型信息const std::type_info&。是不能赋值的。

```c++
const std::typeinfo& a = typeid(int); //初始化可以
a = typeid(double); //再赋值就是错误
```



为了解决这个问题，引入了std::type_index类，封装了一个指向type_info的指针。

```c++
std::type_index a = typeid(int);  	//实际是调用构造函数 std::type_index(typeid(int));
std::type_index b = a;
assert(a==b); 						//a,b都指向同一个type_info
b=typeid(double); 					//重新赋值
assert(a!=b); 						//a,b不再相同，a不受影响，仍指向int的type_info
std::cout <<a.name(); 				//实际调用的是底层type_info::name()函数
```



**例：**

不使用type_index

```c++
class SomeThing{};

int main()
{
    auto comp = [](const std::type_info* x, const std::type_info* y) -> bool 
    {
   	 	return x->before(*y);
    };

    std::map<const std::type_info*, SomeThing, decltype(comp)> data(comp);

    data[&typeid(int)] = SomeThing();
    data[&typeid(double)] = SomeThing();
}
```

例子：使用type_index, 代码清洁了很多

```c++
class SomeThing{};

int main()
{
    std::map< std::type_index, SomeThing> data;

    data[typeid(int)] = SomeThing();
    data[typeid(double)] = SomeThing();
}
```



**Reference**

https://www.cnblogs.com/spruce/p/13275588.html

https://www.apiref.com/cpp-zh/cpp/language/typeid.html

https://www.apiref.com/cpp-zh/cpp/types/type_info.html

https://www.apiref.com/cpp-zh/cpp/types/type_index.html