### 类与类的关系

#### 类的组合 

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

