---
title: 'KVO and KVC'
date: 2016-01-17 21:38:50
tags:
---

# KVO
>key Value Observer

## 为想要观察的对象添加观察者
```objc
  [object addObserver:observer
          forKeyPath:@"frame"
          options:NSKeyValueObservingOptionNew
          context:nil];
```
- **object**:被观察对象
- **observer**:观察对象
- **forKeyPath**:需要检测的属性(property的name),如UIViewV的frame,center
- **context**:任意类型(一种标识,可用于同时监听不同对象的相同属性？)
options:
- **options:**

```objc
NSKeyValueObservingOptionNew  更改前的值
NSKeyValueObservingOptionOld  更改后的值
NSKeyValueObservingOptionInitial 初始化的值,一旦注册,立马调用一次,通常会带有新值,不会带有旧值
NSKeyValueObservingOptionPrior 分两次调用,值改变之前和值改变后
```

## 添加观察键值变化的处理方法
```objc
-(void)observeValueForKeyPath:(NSString *)keyPath
                     ofObject:(id)object
                     change:(NSDictionary<NSString *,id> *)change
                     context:(void *)context
}
@end
```
- **KeyPath**:对应forKeyPath
- **object**: 被观察者
- **change**: 对应相应选项下值的改变(字典类型)可通过对应的key取值,key有new,old,等,可打印输出观察取key值
- **context**: 对应context
<!--more-->
# KVC
>key Value Coding

```objc
[x setValue:obj forKey:key];       -> x.key = objc(x中编码为“key”的属性的值设为objc)
[x setValue:obj forKeyPath:path]（用于传递属性）   -> x.path =objc(x中键路径为path的内容设为objc) 如path = a.b 则是把x类中属性a（a是一个类，有b属性）的属性b赋值为 objc
[x setValuesForKeysWithDictionary:dic]; -> x中所有编码为dic中的“key”值的属性的值设为所有dic中对应“key”的值
```

```objc
[x setObject:objc forKey:key]; 不要搞混！！！它是NSMutableDictionary特有的方法（可变字典添加键值对的，可想做类似于[NSMutableArray addObject:objc ]）

```
http://www.cocoachina.com/industry/20140224/7866.html


```objc
@class HerosAllParse;
/*
 解析规则
 1.遇到字典新建类型，
 2.然后在类里面，把所有的键对应的属性声明
 3.每个类型都需要一个解析方法
 4.对于数组类型的属性，标记它的内容，通过@class引入类型
 5.解析方法的实现顺序，由底层向表层实现
*/

@interface HerosParse : NSObject
@property(nonatomic) NSArray<HerosAllParse *> *all;
+(instancetype)parse:(NSDictionary *)dic;
@end

@interface HerosAllParse : NSObject
@property(nonatomic) NSString *cnName;
@property(nonatomic) NSString *enName;
@property(nonatomic) NSString *location;
@property(nonatomic) NSString *price;
@property(nonatomic) NSString *rating;
@property(nonatomic) NSString *tags;
@property(nonatomic) NSString *title;
+(instancetype)parse:(NSDictionary *)dic;
@end
```

```objc
@implementation HerosParse

+(instancetype)parse:(NSDictionary *)dic{
    NSMutableArray *ary = [NSMutableArray new];
    NSArray *arr = dic[@"all"];
    for (NSDictionary *dic in arr) {
        [ary addObject:[HerosAllParse parse:dic]];
    }
    //[dic setValue:ary forKey:@"all"];
    //为什么得转换为可变dic？？？？？？

    NSMutableDictionary *multableDic = [NSMutableDictionary dictionaryWithDictionary:dic];
    [multableDic setObject:ary forKey:@"all"];

    id objc = [self new];
    [objc setValuesForKeysWithDictionary:multableDic];
    return objc;
}
-(void)setNilValueForKey:(NSString *)key{

}
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{

}
@end


@implementation HerosAllParse
+(instancetype)parse:(NSDictionary *)dic{
    id objc = [self new];
    [objc setValuesForKeysWithDictionary:dic];
    return objc;
}
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{

}
-(void)setNilValueForKey:(NSString *)key{

}
@end
```

# 通知
>与KVO的区别？
