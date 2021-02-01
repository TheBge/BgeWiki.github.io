title: new and delete
author: John Doe
tags:
  - C++
  - 内存管理
categories:
  - C++
date: 2020-12-16 14:11:00
---
##### 重载new和delete   
1. operator new（）的返回值是一个void*,而不是指向任何特定类型的指针。所做的是分配内存，而不是完成一个对象建立——直到构造函数调用了才完成了对象的创建，它是编译器确保的动作，不在我们的控制范围之内。   
2. operator delete（）的参数是一个指向由operator new（）分配的内存的void*,而不是指向任何特定类型的指针。参数是一个void*是因为它是在调用析构函数后得到的指针。析构函数从存储单元里移去对象。operator delete（）的返回类型是void。