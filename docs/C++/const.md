#### **顶层const与底层const？**

**指向常量的指针：**

不能改变指向的内容。const放在类型名前后都可以，const int 和 int const是等价的。声明指向常量的指针也就是底层const。

但是指向常量的指针不代表指向的内容一定是常量，只是不能改变指向的内容。

```c++
int num = 1;
int const *p_num = &num;
const int* p_num = &num;
```

**指针常量：**

指针本身是常量，**声明时必须初始化**，存储的地址不能改变。声明时const必须放在指针符号*后面。声明常量指针就是顶层const。

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



#### Reference

https://www.cnblogs.com/cthon/p/9166715.html

