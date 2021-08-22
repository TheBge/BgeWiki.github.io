```c++
#include <iostream>
#include <cstring>

using namespace std;

class String{
public:
    // 默认构造函数
    String(const char *str = nullptr);
    // 拷贝构造函数
    String(const String &str);
    // 析构函数
    ~String();
    // 字符串赋值函数
    String& operator=(const String &str);

private:
    char *m_data;
    int m_size;
};

// 构造函数
String::String(const char *str)
{
    if(str == nullptr)  // 加分点：对m_data加NULL 判断
    {
        m_data = new char[1];   // 得分点：对空字符串自动申请存放结束标志'\0'的
        m_data[0] = '\0';
        m_size = 0;
    }
    else
    {
        m_size = strlen(str);
        m_data = new char[m_size + 1];
        strcpy(m_data, str);
    }
}

// 拷贝构造函数
String::String(const String &str)   // 得分点：输入参数为const型
{
    m_size = str.m_size;
    m_data = new char[m_size + 1];  //加分点：对m_data加NULL 判断
    strcpy(m_data, str.m_data);
}

// 析构函数
String::~String()
{
    delete[] m_data;
}

// 字符串赋值函数
/*
我们先用delete释放了实例m_data的内存，如果此时内存不足导致new char抛出异常，则m_data将是一个空指针，
这样非常容易导致程序崩溃。违背了异常安全性原则。
*/
String& String::operator=(const String &str)  // 得分点：输入参数为const
{
    if(this == &str)    //得分点：检查自赋值
        return *this;

    delete[] m_data;    //得分点：释放原有的内存资源
    m_size = strlen(str.m_data);
    m_data = new char[m_size + 1];  //加分点：对m_data加NULL 判断
    strcpy(m_data, str.m_data);
    return *this;       //得分点：返回本对象的引用
}

// 字符串赋值函数（推荐使用）
// 保证了异常安全性
String& String::operator=(const String &str)
{
    if(this != &str)
    {
        String strTemp(str);

        char* pTemp = strTemp.m_data;
        strTemp.m_data = m_data;
        m_data = pTemp;
    }
    return *this;
}
```

