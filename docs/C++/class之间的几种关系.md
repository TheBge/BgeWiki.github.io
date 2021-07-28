### 类的组合 has a

类A具有类B的属性

```c++  
class Person
{
  public:
	  Person(int _age, string _name) :age(_age), name(_name) {}
	  ~Person() {};
	void print() 
	{
		cout << name<<"	" << age  << endl;
	}
	private:
		int age;
	   string name;
};
class Teacher
{
public:
	Teacher(Person* _person) :person(_person) {}
	~Teacher() {};
	void print()
	{
		this->person->print();
	}
	private:
		Person* person;
		
};
int main()
{
	Person p(40, "lisan");
	Teacher teacher(&p);
	teacher.print();
	system("pause");
	return 0;
}
```



### is-a 和 has-a 的区别：

​		public继承就是一种 is-a 的关系，因为子类继承了父类，也就是说每个子类对象的都是父类对象。类继承允许根据其他类来定义一个类，也就是说通过父类来生成一个子类。这种生成子类的复用通常被称为 “ 白盒复用 ” 。“白盒” 是针对于可视性而言的，父类内部的细节实现在子类中是可见的。

​		组合就是一种 has-a 的关系，在B对象中组合另一个A对象，那么每个B对象都有了一个A对象。这要求A对象具有良好定义的接口。这样就是一个 “ 黑盒复用 ”，在B中并不能看到A的细节实现，只用到了A的接口。

​		继承的缺点：可以看做是破坏了封装，因为在子类中暴露的父类的实现细节。这样本来就违反了面向对象的三大特性之一。而且基类和派生类之间还有很强的依赖关系，耦合度高，这样也违反了 “ 高内聚，低耦合 ” 的特性。
而组合相比于继承来说并没有破坏封装，并且对象之间也没有很强的依赖性。这样针对于用户也比较友好。

总而言之，能使用组合就使用组合，尽量不要使用继承，除非你知道自己在做什么！

