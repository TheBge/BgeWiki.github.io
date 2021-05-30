### 第4章 智能指针

#### 条款18：使用std::unique_ptr管理具备专属所有权的资源

**<font color='red'>要点</font>**

- std::unique_ptr是小巧、高速的、具备只移型别的智能指针，对托管资源实施专属所有权语义。
- 默认的，资源析构采用delete运算符来实现，但可以指定自定义删除器。具有状态的删除器和采用函数指针实现的删除器会增加unique_ptr型别的对象的尺寸。
- 将std::unique_ptr容易转换成shared_ptr。



#### 条款19：使用std::shared_ptr管理具备共享所有权的资源

**<font color='red'>要点</font>**

- shared_ptr提供了方便的手段，实现了任意资源在共享所有权语义下进行生命周期管理的垃圾回收。
- 与unique_ptr相比，shared_ptr的尺寸通常是裸指针尺寸的两倍，它还会带来控制块的开销，并要求原子化的引用计数操作。
- 默认的资源析构通过delete运算符进行，但同时也支持用户自定义删除器。删除器的型别对shared_ptr的型别没有影响。
- 避免使用裸指针型别的变量来创建shared_ptr。



#### 条款20：对于类似std::shared_ptr但有可能空悬的指针使用weak_ptr

**<font color='red'>要点</font>**

- 使用weak_ptr来代替可能空悬的shared_ptr。
- weak_ptr可能的用武之地包括缓存，观察者列表，以及避免shared_ptr指针环路。



#### 条款21：优选选用std::make_unique和std::make_shared，而非直接使用new

要点

- 相比直接使用new表达式，make系列函数消除了重复代码、改进了异常安全性，并且对于make_shared和std::allcoated_shared而言，生成的目标代码会尺寸更小、速度更快。
- 不适于使用make系列函数的场景包括：需要使用定制删除器，以及期望直接传递大括号初始化。



#### 条款22：使用Pimpl习惯用法时，将特殊成员函数的定义放到实现文件中

Pimpl（point to implementation）把某类的数据成员用一个指涉到某实现类（或结构体）的指针替代，然后把原来放在主类中的数据成员放置到实现类中，并通过指针间接访问这些数据成员。

**<font color='red'>要点</font>**

- Pimpl惯用法通过降低类的客户和类实现者之间的依赖性，减少了构建遍数。
- 对于采用unique_ptr来实现的pImpl指针，须在类的头文件中声明特种成员函数，但在实现文件中实现它们。即使默认函数实现有着正确的行为，也必须这样做。
- 上述建议只适用于unique_ptr，但不适用shared_ptr。