### struct  

![upload successful](../img/pasted-3.png)
```c
inflatable hat；   
inflatable woopie_cushion     
inflatable mainframe   

struct inflatable goose；   //C语言结构体对象声明，与C++结构体声明都可以
inflatable Vincent；   //只有C++结构体声明可以用
```
由于hat的类型是inflatable，因此可以使用成员运算符（.）来访问各个成员。例如，hat.volume指的是结构的volume成员，hat.privce,指的是price成员。

![upload successful](../img/pasted-6.png)

指向结构体变量的指针 
要注意的是，只有“指针变量名”后面才能加“->”，千万不要在成员名后面加“->”
以下 3 种形式是等价的： 
• 结构体变量.成员名
• (*指针变量).成员名
• 指针变量->成员名