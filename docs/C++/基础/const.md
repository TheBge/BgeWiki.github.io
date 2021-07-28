#### C++中的const

**<font color='red'>非类成员的const</font>**

①const变量默认是内部连接的，因此在不同的编译单元中可以有同名的const 变量定义。

②编译时常量，因此可以像#define一样使用，而且因为上面一点，可以在头文件中定义const变量，包含的不同的cpp文件（编译单元）中使用而不引起命名冲突。

③编译器默认不为const变量分配内存，除非：1. 使用 extern 申明， 2：程序中有引用const 变量的地址。 

④c++中临时对象/内置变量默认具有const属性。

**<font color='red'>类中的const</font>**

①与c语言中的const一样，只是运行时常量，不能作为数组维数使用，即不能取代#define。

　在类中使用下面两种方式取代#define：

　　1：static const... 

　　2 : enum{....}//enum 不占存储空间。

②**类中的const 变量占用存储空间**。

③**<font color='red'>类中的const成员变量需要在构造函数初始化列表中初始化</font>**。

```c++
class CExample
{
public:
	CExample():m_a(1),m_b(2){/*m_a = 1; compile error*/}
	CExample(const CExample&c):m_a(1){/*m_a = 1; compile error*/}
	~CExample(){}
private:
	const int m_a;
	int m_b;
};
```

类中的const成员变量都要放在初始化列表中初始化。

④**const 对象**：在该对象生命周期内，**必须保证没有任何成员变量被改变**。**const对象只能调用const成员函数**。

⑤**const成员函数**： void fun() const ... 不仅**能被const对象调用，也能被非const对象调用**，因此，如果确认一个任何成员函数不改变任何成员变量，应该习惯性将该函数定义成const类型。



**const和#define的区别**

 **const分配内存的时机，是编译器编译期间，与#define相同** 

 C++中的const常量类似于宏定义#define

　　*const int c=5  等价于 #define c 5*

1) 编译器处理方式不同 **define宏是在预处理阶段展开。 const常量是编译运行阶段使用。**

2) 类型和安全检查不同 **define宏没有类型，不做任何类型检查**，仅仅是展开。 **const常量有具体的类型，在编译阶段会执行类型检查**。

**注意：尽量以const替换#define**



#### **顶层const与底层const？**

**指向常量的指针：**

不能改变指向的内容。const放在类型名前后都可以，const int 和 int const是等价的。声明指向常量的指针也就是**底层const**。

但是指向常量的指针不代表指向的内容一定是常量，只是不能改变指向的内容。

```c++
int num = 1;
int const *p_num = &num;
const int* p_num = &num;
```

**指针常量：**

指针本身是常量，**声明时必须初始化**，存储的地址不能改变。声明时const必须放在指针符号*后面。声明常量指针就是**顶层const**。

```c++
int num = 1;
int *const p_num = &num; //顶层const
```

"effective c++"第三条讲到： 只需要判断const是在 * 的左边还是右边即可。左边则是修饰被指物，即被指物是常量，不可以修改它的值；右边则是修饰指针，即指针是常量，不可以修改它的指向；在左右两边，则被指物和指针都是常量，都不可以修改。

**const在 * 左边，变量为const**

**const在 * 右边，指针为const**

```c++
int *p =&x;//p为指针,指针指向int类型 x     
const int *p = &x; //指针p,指针指向const int类型的x       
int *const p = &x; //指针是const p 地址为常量，说明指针指向的对象不能改变      
const int *const p =&x; //指针是const p，指向cosnt int类型的x
```



#### **函数传递中的const：**

**普通形参**加不加const限定符对实参没有影响，**引用形参和指针形参**前面没有const限定符时，实参必须是非const的，而前面有const限定符时对实参也没有什么影响。

原因在于实参的传递方式不同，函数中的形参是普通形参的时，函数只是操纵的实参的副本，而无法去修改实参，实参会想，你形参反正改变不了我的值，那么你有没有const还有什么意义吗？引用形参和指针形参就下不同了，函数是对实参直接操纵，没有const的形参时实参的值是可以改变的。



#### **const成员函数**

在类的成员函数后面加上const，表明这个函数不会对这个类对象的数据成员作任何改变。

设计类时的一个原则，<font color='red'>对不改变数据成员的成员函数都要在后面加上const</font>。

所以 const 关键字对成员函数的行为作了更加明确的限定：

（1）有 const 修饰的成员函数（指 const 放在函数参数表的后面，而不是在函数前面或者参数表内），只能读取数据成员，不能改变数据成员；没有 const 修饰的成员函数，对数据成员则是可读可写的。
（2）除此之外，在类的成员函数后面加 const 还有什么好处呢？那就是常量（即 const）对象可以调用 const 成员函数，而不能调用非const修饰的函数。

<font color='red'>注意：</font><font color='red'>两个成员函数如果只是常量性不同，是可以被重载的，由类对象的常量性决定调用哪一个函数。</font>

```c++
class A
{
    public:
        A(int N = 0);
        void Fun();
    private:
        int n;
};

A::A(int N):n(N){}
void A::Fun()
{
    cout << n << endl;
}
int _tmain(int argc, _TCHAR* argv[])
{
    A a;
    a.Fun();
    const A &b = a;
    b.Fun();//const引用的对象调用非const成员函数
    //const A a;
    //a.Fun();//const对象调用非const成员函数
    cin.get();
    return 0;
}
```

当使用const对象调用非const成员函数时编译会报错：error C2662: 'A::Fun' : cannot convert 'this' pointer from 'const A' to 'A &'

报错原因：因为const对象在调用成员函数时会隐含的把实参把中*this修改成const class * const this，以导致非const成员函数在接收时还是使用了class *const this接收，结果就是把const的指针赋给非const的指针

修正方法：以前错误只需在对象的成员函数后面加上const即可



**类中的const成员变量都要放在初始化列表之中进行**
const数据成员
引用数据成员
对象数据成员（内置类）

**const成员函数**
void print() const => const 类名 * const this
在其内部是不能修改数据成员
也不能调用非const成员函数
const对象只能调用const成员函数,必须要提供一个const版本的成员函数



### this

- this指针的一般形式为（假如类类型是A，当然与类类型相关）：A * const ，也就是说this指针是一个常量指针（指针的地址值为常量）
- 在类的非const成员函数中，this的类型为一般形式，即 A * const
- 在类的const成员函灵数，this的类型为：const A * const，即指向常量对象的常量指针

const_cast转换是去掉所指向对象的常量性，使得const成员函数可以修改数据成员或调用非const成员函数（本来是不允许的）；但不推荐用这样的方法，如果只是修改数据成员，请用mutable关键字。



#### Reference

https://www.cnblogs.com/cthon/p/9166715.html

