---
title: 网络请求
date: 2016-02-13 09:35:22
tags:
---
# 工作流程（AFNetworking）

## 环境配置
### 工作模式
创建一个NSURLSessionConfiguration，用于第二步创建NSSession时设置工作模式和网络设置：
### 网络设置
创建一个NSURLSession，系统提供了两个创建方法：

## task
  >创建一个NSURLRequest调用刚才的NSURLSession对象提供的Task函数，创建一个**NSURLSessionTask**。
根据职能不同Task有三种**子类**：
NSURLSessionUploadTask：上传用的Task，传完以后不会再下载返回结果；
NSURLSessionDownloadTask：下载用的Task；
NSURLSessionDataTask：可以上传内容，上传完成后再进行下载。

<!--more-->
## 得到的Task，调用resume开始工作。

## 解耦和封装网络请求处理
```objc
+(id)setServerListCompletionHandle:(void(^)(NSArray *model,NSError *error))completionHandle;
```


```objc
NSURLSession *session = [NSURLSession sharedSession];
   NSURLSessionTask *task = [session dataTaskWithURL:[NSURL URLWithString:kHeroAddress]
                                  completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
       /*data参数是从 kHeroAddress上下载的数据
        这个数据是服务器把字典/数组根据Json协议编码以后生成的
        拿到Jason数据以后，反编码 回到字典或者数组
       */
       //因为根据返回值类型，可以看到是字典类型的
       //Serialization:序列化
       //参数2：代表当前data的原始数据是什么类型
       //NSJSONReadingMutableContainers:代表原始数据是数组/字典
       NSError *err = nil;
       NSDictionary *responseObj;
       reponseObj = [NSJSONSerialization JSONObjectWithData:data
                                                    options:NSJSONReadingMutableContainers
                                                      error:&err];
#warning 解析数据转化
       self.parse = [HerosParse parse:responseObj];
       //数据获取完以后，刷新界面，网络请求是子线程的，刷新界面需要回到主线程
       //开始发送请求
       [[NSOperationQueue mainQueue]addOperationWithBlock:^{
           [self.tableView reloadData];
       }];
   }];
   [task resume];
}
```
