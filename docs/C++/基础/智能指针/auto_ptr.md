在03版本中标准库第一次引入了智能指针std::auto_ptr，这也是当时唯一的智能指针类型。但是在11版本中，其被废弃。



定义大概如下：

```c++
template <typename _Tp>
class auto_ptr
{
private:
    _Tp *_M_ptr;

public:
    explicit auto_ptr(_Tp *__p = 0) throw();
    auto_ptr(auto_ptr &__a) throw();
    auto_ptr &operator=(auto_ptr &__a) throw();
    ~auto_ptr();

    _Tp &operator*() const throw();
    _Tp *operator->() const throw();

    _Tp *get() const throw();
    _Tp *release() throw();
    void reset(_Tp *__p = 0) throw();
};
```

功能：

1. 支持拷贝构造
2. 支持拷贝赋值
3. 支持operator -> 和 *解引用
4. 支持指针变量reset
5. 保证指针持有者唯一



### 有问题的使用场景

#### 场景一：数组保存std::auto_ptr实例

```cpp
std::vector<std::auto_ptr<People>> peoples;
// 这里实例化多个people并保存到数组中
...
std::auto_ptr<People> one = peoples[5];
...
std::cout << peoples[5]->get_name() << std::endl; 
```

上面程序如果将std::auto_tr类型替换为原始指针，就不会有问题。但是这里却会导致程序报段错误崩溃！问题就出在：

```cpp
std::auto_ptr<People> one = peoples[5];
```

这行代码会将peoples[5]中的指针所有权转移了！即该变量中的指针已经为null了。后续对解引用是不正确的。

**原因在于std::auto_ptr支持operator=，为了确保指针所有者唯一，这里转移了所有权！**



#### 场景二：函数传参

```cpp
void do_somthing(std::auto_ptr<People> people)
{
    // 该函数内不对people变量执行各种隐式/显示的所有权转移和释放
    ...
}

std::auto_ptr<People> people(new People("jony"));
do_something(people);
...

std::cout << people->get_name() << std::endl; 
```

这里对auto_ptr的使用与原始指针的使用完全相同。但是该代码还是会崩溃！问题就在于执行do_something()函数时，传递参数导致原变量的指针所有权转移了，即people变量实际已经变为null了！

**原因在于std::auto_ptr支持拷贝构造，为了确保指针所有者唯一，这里转移了所有权！**

