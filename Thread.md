---
title: Thread
date: 2016-02-12 09:35:22
tags:
---
# 多线程的实现方式
 1.pThread:最早期，纯c，兼容所有c语言基础的代码
 2.NSThread:本质上是使用oc对pThread的一个封装，是早期iOS使用的多线程
 3.GCD：使用C语法+block语法，目前为止，苹果主推的多线程技术，功能强大，效率高
 4.NSOperation：使用oc对GCD的封装，在GCD基础上额外增加了几个特性，不过效率没有GCD高，通常在不使用这些特性时，依然选择GCD

<!--more-->
## NSThread

## 3GCD
### 进程锁


### Barrier
### GCD单例模式
```objc
+(MySingle *)sharedMySingle{
    static MySingle *my = nil;
    //传统的单例模式在多线程情况下容易出现同一时间被多线程同时调用的现象
    //基于线程安全的考虑，GCD提供了专门制作单例的代码
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        my = [MySingle new];
    });
    return my;
}
```

## NSOperation


## GCD与NSOperation的选择
GCD是基于c的底层api，NSOperation属于object-c类。ios 首先引入的是NSOperation，IOS4之后引入了GCD和NSOperationQueue并且其内部是用gcd实现的。

相对于GCD：
1，NSOperation拥有更多的函数可用，具体查看api。
2，在NSOperationQueue中，可以建立各个NSOperation之间的依赖关系。
3，有kvo，可以监测operation是否正在执行（isExecuted）、是否结束（isFinished），是否取消（isCanceld）。
4，NSOperationQueue可以方便的管理并发、NSOperation之间的优先级。
GCD主要与block结合使用。代码简洁高效
