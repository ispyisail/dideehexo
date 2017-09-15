---
title: YYModel的使用 映射Model属性名和数组
date: 2015-10-22 12:54:34
categories:
- iOSFrame
tags:
- YYModel
- YYKit
---


## 项目地址

YYModel : [https://github.com/ibireme/YYModel](https://github.com/ibireme/YYModel)

<!-- more -->

## Model声明

使用泛型声明方便自己调用

```objc
@property(nonatomic, strong) NSArray<ChildModel *> *childName;
```

## Model映射实现

映射是告诉 YYModel : `childName` 是 `ChildModel` 类型的数组

```objc
+ (NSDictionary *)modelContainerPropertyGenericClass {
    return @{
            //映射 childName 类型为 ChildModel
             @"childName" : [ChildModel class]
             };
}
```


## 其他

此处也可以映射自定义属性名,可以一对多,顺序优先

```objc
+ (NSDictionary *)modelContainerPropertyGenericClass {
    return @{
             @"ModelName" : @"JsonName"
             //一对多,顺序查找
             @"userId" : @[@"id",@"ID",@"userId"];
             };
}
```


