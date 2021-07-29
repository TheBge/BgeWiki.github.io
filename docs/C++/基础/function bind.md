### std::function

#### std::function 简介

类模板std :: function是一个通用的多态函数包装器。 std :: function的实例可以存储，复制和调用任何可调用的目标 ：包括函数，lambda表达式，绑定表达式或其他函数对象，以及指向成员函数和指向数据成员的指针。

当std::function对象未包裹任何实际的可调用元素，调用该std::function对象将抛出std::bad_function_call异常。



### std::bind

1.bind()函数。可以固化（绑定）目标函数的行参。

2.占位符。可以在bind时保留部分参数不立即固定，起到减少参数数目、交换参数顺序等适配器功能。



### Example

包装普通函数

```c++
void foo(const string &s)
{
    cout << s << endl;
}

function<void(const string &s)> f =&foo;
f("world");
```

包装模板函数

```c++
#include <iostream>
#include <functional>
using namespace std;

template <class T>
T g_Minus(T i, T j)
{
    return i - j;
}

int main()
{
    function<int(int, int)> f = g_Minus<int>;
    cout << f(1, 2) << endl;        
    return 1;
}
```

包装lambda

```c++
#include <iostream>
#include <functional>
using namespace std;

auto g_Minus = [](int i, int j){ return i - j; };

int main()
{
    function<int(int, int)> f = g_Minus;
    cout << f(1, 2) << endl;                                
    return 1;
}
```

包装函数对象

```c++
#include <iostream>
#include <functional>
using namespace std;

struct Minus
{
    int operator() (int i, int j)
    {
        return i - j;
    }
};

int main()
{
    function<int(int, int)> f = Minus();
    cout << f(1, 2) << endl;                             
    return 1;
}
```

包装类成员函数

```c++
#include <iostream>
#include <functional>
using namespace std;

class Math
{
public:
    int Minus(int i, int j)
    {
        return i - j;
    }
};

int main()
{
    Math m;
    function<int(int, int)> f = bind(&Math::Minus, &m, placeholders::_1, placeholders::_2);
    cout << f(1, 2) << endl;                                        
    return 1;
}
```