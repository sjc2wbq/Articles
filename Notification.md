---
title: 'Notification'
date: 2016-01-17 21:38:50
tags:
---
# NSNotificationCenter
>iOS通知中心

## - NSNotification类
>一个消息对象类,有三个成员变量

```objc
@property (readonly, copy) NSString *name;
@property (readonly, retain) id object;
@property (readonly, copy) NSDictionary *userInfo;
```
- name:辨别消息对象的唯一标识
- object:针对某一个对象的消息
- userInfo:用来传值的一个字典

## - NSNotificationCenter类
>通知中心,使用单例设计,每个应用程序都会有一个默认的通知中心.用于调度通知的发送和接受

```objc
```
