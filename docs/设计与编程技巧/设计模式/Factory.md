[TOC]



### 简单工厂模式

​		Simple工厂模式描述了一个类，该类有一个创建方法，该方法有一个很大的条件，该条件**根据方法参数**选择要实例化的产品类，然后返回。人们通常会将简单工厂与一般工厂或一种创造性设计模式混淆。在大多数情况下，简单工厂是引入工厂方法或抽象工厂模式的中间步骤。

​		一个简单的工厂通常由一个类中的一个方法表示。随着时间的推移，这个方法可能会变得太大，因此您可能决定将该方法的部分提取到子类中。一旦您多次这样做，您可能会发现整个事情变成了经典的工厂方法模式。顺便说一下，如果你声明一个简单的工厂抽象，它不会神奇地变成抽象工厂模式。

```java
class UserFactory {
    public static function create($type) {
        switch ($type) {
            case 'user': return new User();
            case 'customer': return new Customer();
            case 'admin': return new Admin();
            default:
                throw new Exception('Wrong user type passed.');
        }
    }
}
```

工厂类：创建指定具体实例对象的接口

抽象产品类：具体产品类的父类或接口

具体产品类：工厂类创建的对象是具体产品类的实例

**特点**：工厂类封装了创建产品对象的函数

**缺点**：扩展差，新增产品的时候需要修改工厂类

例：

```c++
// 鞋子抽象类
class Shoes //interface
{
public:
    virtual ~Shoes() {}
    virtual void Show() = 0;
};

// 耐克鞋子
class NiKeShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是耐克球鞋，我的广告语：Just do it" << std::endl;
    }
};

// 阿迪达斯鞋子
class AdidasShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是阿迪达斯球鞋，我的广告语:Impossible is nothing" << std::endl;
    }
};

// 李宁鞋子
class LiNingShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是李宁球鞋，我的广告语：Everything is possible" << std::endl;
    }
};


//ShoesFactory为工厂类，类里实现根据鞋子类型创建对应鞋子产品对象的CreateShoes(SHOES_TYPE type)函数。
enum SHOES_TYPE
{
    NIKE,
    LINING,
    ADIDAS
};

// 总鞋厂
class ShoesFactory
{
public:
    // 根据鞋子类型创建对应的鞋子对象
    Shoes *CreateShoes(SHOES_TYPE type)
    {
        switch (type)
        {
        case NIKE:
            return new NiKeShoes();
            break;
        case LINING:
            return new LiNingShoes();
            break;
        case ADIDAS:
            return new AdidasShoes();
            break;
        default:
            return NULL;
            break;
        }
    }
};
```

```c++
int main()
{
    // 构造工厂对象
    ShoesFactory shoesFactory;

    // 从鞋工厂对象创建阿迪达斯鞋对象
    Shoes *pNikeShoes = shoesFactory.CreateShoes(NIKE);
    if (pNikeShoes != NULL)
    {
        // 耐克球鞋广告喊起
        pNikeShoes->Show();

        // 释放资源
        delete pNikeShoes;
        pNikeShoes = NULL;
    }

    // 从鞋工厂对象创建阿迪达斯鞋对象
    Shoes *pLiNingShoes = shoesFactory.CreateShoes(LINING);
    if (pLiNingShoes != NULL)
    {
        // 李宁球鞋广告喊起
        pLiNingShoes->Show();

        // 释放资源
        delete pLiNingShoes;
        pLiNingShoes = NULL;
    }

    // 从鞋工厂对象创建阿迪达斯鞋对象
    Shoes *pAdidasShoes = shoesFactory.CreateShoes(ADIDAS);
    if (pAdidasShoes != NULL)
    {
        // 阿迪达斯球鞋广告喊起
        pAdidasShoes->Show();

        // 释放资源
        delete pAdidasShoes;
        pAdidasShoes = NULL;
    }

    return 0;
}
```



### 工厂模式

​		工厂方法是一种创建(Creational)设计模式，它解决了创建产品对象而不指定其具体类的问题。Factory方法定义了一个方法，该方法应该用于创建对象，而不是直接调用构造函数(new操作符)。子类可以重写此方法来更改将要创建的对象的类。

**例：**

情景：为了生产某种鞋子，需要为不同牌子的鞋开设独立的生产线。

抽象工厂类：提供创建具体产品类的接口

具体工厂类：继承抽象工厂，实现创建具体产品对象

抽象产品类：具体产品类的接口

具体产品类：具体工厂要创建的对象

**特点：**工厂部分抽象出了接口，实现交给子类

**缺点：**新增产品时，要增加对应产品的具体工厂类。

```c++
// 总鞋厂
class ShoesFactory // Factory interface
{
public:
    virtual Shoes *CreateShoes() = 0;
    virtual ~ShoesFactory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public ShoesFactory
{
public:
    Shoes *CreateShoes()
    {
        return new NiKeShoes();
    }
};

// 阿迪达斯生产者/生产链
class AdidasProducer : public ShoesFactory
{
public:
    Shoes *CreateShoes()
    {
        return new AdidasShoes();
    }
};

// 李宁生产者/生产链
class LiNingProducer : public ShoesFactory
{
public:
    Shoes *CreateShoes()
    {
        return new LiNingShoes();
    }
};
```

```c++
int main()
{
    // ================ 生产耐克流程 ==================== //
    // 鞋厂开设耐克生产线
    ShoesFactory *niKeProducer = new NiKeProducer();
    // 耐克生产线产出球鞋
    Shoes *nikeShoes = niKeProducer->CreateShoes();
    // 耐克球鞋广告喊起
    nikeShoes->Show();
    // 释放资源
    delete nikeShoes;
    delete niKeProducer;

    // ================ 生产阿迪达斯流程 ==================== //
    // 鞋厂开设阿迪达斯生产者
    ShoesFactory *adidasProducer = new AdidasProducer();
    // 阿迪达斯生产线产出球鞋
    Shoes *adidasShoes = adidasProducer->CreateShoes();
    // 阿迪达斯球鞋广喊起
    adidasShoes->Show();
    // 释放资源
    delete adidasShoes;
    delete adidasProducer;

    return 0;
}
```



### 抽象工厂

抽象工厂模式（Abstract Factory Pattern）是围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂。

在抽象工厂模式中，接口是负责创建一个相关对象的工厂，不需要显式指定它们的类。每个生成的工厂都能按照工厂模式提供对象。



情景：现在工厂不止产鞋子了，衣服裤子一起产了。

结构与工厂模式一样。

```c++
// 基类 衣服
class Clothe
{
public:
    virtual void Show() = 0;
    virtual ~Clothe() {}
};

// 耐克衣服
class NiKeClothe : public Clothe
{
public:
    void Show()
    {
        std::cout << "我是耐克衣服，时尚我最在行！" << std::endl;
    }
};

// 基类 鞋子
class Shoes
{
public:
    virtual void Show() = 0;
    virtual ~Shoes() {}
};

// 耐克鞋子
class NiKeShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是耐克球鞋，让你酷起来！" << std::endl;
    }
};

//Factory为抽象工厂，提供了创建鞋子CreateShoes()和衣服产品CreateClothe()对象的接口。
//NiKeProducer为具体工厂，实现了创建耐克鞋子和耐克衣服的方式。
// 总厂
class Factory
{
public:
    virtual Shoes *CreateShoes() = 0;
	virtual Clothe *CreateClothe() = 0;
    virtual ~Factory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public Factory
{
public:
    Shoes *CreateShoes()
    {
        return new NiKeShoes();
    }
	
	Clothe *CreateClothe()
    {
        return new NiKeClothe();
    }
};
```

```c++
int main()
{
    // ================ 生产耐克流程 ==================== //
    // 鞋厂开设耐克生产线
    Factory *niKeProducer = new NiKeProducer();
    
	// 耐克生产线产出球鞋
    Shoes *nikeShoes = niKeProducer->CreateShoes();
	// 耐克生产线产出衣服
    Clothe *nikeClothe = niKeProducer->CreateClothe();
    
	// 耐克球鞋广告喊起
    nikeShoes->Show();
	// 耐克衣服广告喊起
    nikeClothe->Show();
	
    // 释放资源
    delete nikeShoes;
	delete nikeClothe;
    delete niKeProducer;

    return 0;
}
```



**小结：**

简单工厂模式，违背了开闭原则。

工厂和抽象工厂，增加产品类时需要增加对应的具体工厂类。



**Reference：**

https://zhuanlan.zhihu.com/p/83535678

https://zhuanlan.zhihu.com/p/83537599