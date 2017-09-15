---
title: 仿 SDWebImage框架 Block回调传值思路
date: 2015-10-23 21:54:34
categories:
- iOSFrame
tags:
- SDWebImage
- Block
---


### 类型

<img src="/images/2016-10-23%20%E4%B8%8B%E5%8D%889.37.30-1.png" width="208"/>



<!-- more -->

* `UIImageView+web` : UIImageView 分类
* `DownloadOperationManager` : NSObject 类,管理下载
* `DownloadOperation` : NSOperation 子类 , 下载图片

### 传值过程概述

* 新建类继承 NSOperation
    * 声明属性 : `NSString` (图片地址) , `void(^Block)(UIImage *image)` (代码块)
    * 重写 main 方法 , main 方法默认在子线程异步执行
    * 在 main 方法中进行下载图片操作
    * 声明和实现构造方法 , 参数为图片地址和 Block , 赋值属性 , 返回一个 NSOperation


```objc
#pragma mark - 构造方法
+(instancetype)downloadWithImgURL:(NSString *)imgURL successBlock:(void(^)(UIImage *image))successBlock{
    
    DownloadOperation *op = [[DownloadOperation alloc]init];
    
    op.imgURL = imgURL;
    op.successBlock =successBlock;
    
    return op;
    
}

#pragma mark - 重写 main 方法
-(void)main{
    
    UIImage *image = [UIImage imageWithData:
                      [NSData dataWithContentsOfURL:
                       [NSURL URLWithString:self.imgURL]]];
    
    [[NSOperationQueue mainQueue]addOperationWithBlock:^{
       
        self.successBlock(image);
        
    }];
    
}
```



* 新建类继承 NSObject
    * 声明属性 : `NSOperationQueue` (队列)
    * 声明和实现单例
    * 实现 init 方法 , 初始化 `NSOperationQueue` 属性 
    * 声明和实现构造方法 , 调用 `DownloadOperation(NSOperation)` 的构造方法 , 添加到队列


```objc
#pragma mark - 实现单例
+(instancetype)shardManager{
    
    static id instance;

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        instance = [[self alloc]init];
    });
    
    return instance;
}

#pragma mark - 初始化
- (instancetype)init
{
    self = [super init];
    if (self) {
        self.queue = [[NSOperationQueue alloc]init];
    }
    return self;
}

#pragma mark - 构造方法
-(void)downloadWithImgURL:(NSString *)imgURL successBlock:(void(^)(UIImage *image))successBlock{
    
    DownloadOperation *op = [DownloadOperation downloadWithImgURL:imgURL successBlock:^(UIImage *image) {
        
        successBlock(image);
        
    }];
    
    [self.queue addOperation:op];
    
}
```



* 新建 UIImageView 的分类
    * 声明和实现分类方法
    * 在方法中调用 `DownloadOperationManager(NSObject)` , Block 传入 设置图片 


```objc
#pragma mark - 分类方法
-(void)DEE_setImageWithURLStr:(NSString *)str{
    
    [[DownloadOperationManager shardManager]downloadWithImgURL:str successBlock:^(UIImage *image) {
        self.image = image;
    }];
    
}
```

* 此时控制器中调用此分类方法
    * 正向传值 : 通过参数(图片地址)传递
    * Block 回调 : 


### 传值过程

1 . `UIImageView+web`(imgURL+Block) ->
2 . `DownloadOperationManager`(imgURL+Block) ->
3 . `DownloadOperation`(imgURL+Block)

3 . `DownloadOperation`(Block(image)) ->
2 . `DownloadOperationManager`(Block(image)) ->
1 . `UIImageView+web`(Block{self.image=image})


### 完整Demo

[https://github.com/didez/DEEWebImage](https://github.com/didez/DEEWebImage)



