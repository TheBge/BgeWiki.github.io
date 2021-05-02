title: CallBack
author: Bing
tags:
  - C++
categories: []
date: 2021-03-23 17:47:00
---
##### 同步和异步
同步方式：
通过 参数（例如 qsort 的最后一个参数）传递回调函数
调用者 立即调用 回调函数（调用时刻 在函数返回前）
此处的 qsort 和 compare 调用栈相同
异步方式：
通过 注册（例如 signal 函数）设置回调函数
调用者 先存储 回调函数，在未来的某个 调用时刻，取出并调用 回调函数
此处的 signal 和 block_interrupt 调用栈不同

##### Reference
[1] https://blog.csdn.net/sinat_38183777/article/details/83958887?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-5&spm=1001.2101.3001.4242  
[2] https://bot-man-jl.github.io/articles/?post=2017/Callback-Explained  
[3] https://bot-man-jl.github.io/articles/?post=2019/Inside-Cpp-Callback#%E5%9B%9E%E8%B0%83%E6%98%AF%E5%90%8C%E6%AD%A5%E8%BF%98%E6%98%AF%E5%BC%82%E6%AD%A5%E7%9A%84