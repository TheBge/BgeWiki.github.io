**push_back vs emplace_back** 

For emplace_back constructor A (int x_arg) will be called. And for push_back A (int x_arg) is called first and move A (A &&rhs) is called afterwards. 
Of course, the constructor has to be marked as explicit, but for current example is good to remove explicitness.  

``` c++
#include <iostream>
#include <vector>
class A
{
public:
  A (int x_arg) : x (x_arg) { std::cout << "A (x_arg)\n"; }
  A () { x = 0; std::cout << "A ()\n"; }
  A (const A &rhs) noexcept { x = rhs.x; std::cout << "A (A &)\n"; }
  A (A &&rhs) noexcept { x = rhs.x; std::cout << "A (A &&)\n"; }

private:
  int x;
};

int main ()
{
  {
    std::vector<A> a;
    std::cout << "call emplace_back:\n";
    a.emplace_back (0);
  }
  {
    std::vector<A> a;
    std::cout << "call push_back:\n";
    a.push_back (1);
  }
  return 0;
}
output:

call emplace_back:
A (x_arg)

call push_back:
A (x_arg)
A (A &&)
```
**emplace_back的特点** 

1、当调用push_back或insert成员函数时，是把元素类型的对象传递给它们，这些对象被拷贝到容器中。而当我们调用一个emplace系列函数时，则是将相应参数传递给元素类型的构造函数。

2、这样emplace_back能就地通过参数构造对象，不需要拷贝操作，相比push_back能更好的避免内存的拷贝和移动，提升容器插入元素的性能。

3、大多数情况都应该使用emplace系列函数：emplace; emplace_back; emplace_hit; emplace_fornt; emplace_after 

**Be careful**
1、emplace函数需要对应的参数对象有对应的构造函数，不然编译报错 

2、emplace函数在容器中直接构造元素。传递给emplace函数的参数必须与元素类型的构造函数相匹配