---
title: CocoaPods 使用经验教程 For Xcode 8
date: 2016-10-17 19:54:34
categories:
- Xcode
tags:
- CocoaPods
- Xcode8
---

![batch_Snip20161013_3-w1365](/images/batch_Snip20161013_3.jpg)

<!--more-->

## 安装 CocoaPods


请参考 [CocoaPods的安装以及遇到的坑](http://www.cocoachina.com/ios/20160922/17622.html)


## 创建 Podfile

打开终端 , `cd+拖拽`切换到 项目目录

![batch_Snip20161013_6](/images/batch_Snip20161013_6-1.jpg)
<!-- more -->


输入:`vim Podfile`


输入配置信息并保存

```
platform :ios,'8.0'
        target '1011-PodTest' do

        pod 'Masonry', '~> 1.0.2'

        end
```


## 配置说明


`ios,'8.0'`  必填

`target '1011-PodTest'`  //填写target名称

`do~end`  -  导入的框架


## 检索开源框架

如 `pod search mas`

![batch_Snip20161013_9-w724](/images/batch_Snip20161013_9.jpg)


复制标注部分,粘贴到Podfile

![batch_Snip20161013_10-w726](/images/batch_Snip20161013_10.jpg)


## 安装和卸载

**安装:**

`pod install`

![batch_Snip20161013_11-w728](/images/batch_Snip20161013_11.jpg)


**卸载:**

编辑Podfile,删除`pod 'MASAttributes', '~> 1.0.2'`行

输入:`pod update`

会根据Podfile升级或卸载

![batch_Snip20161013_12-w721](/images/batch_Snip20161013_12.jpg)


## 使用xcworkspace打开项目

<img src="/images/batch_Snip20161013_13.jpg" width="191"/>

此时可以`#import`了


## 修改 `User Header Search Paths`

找到`User Header Search Paths` 
添加 `${SRCROOT}`并设置为 `recursive`

![Snip 1](/images/Snip%201.png)


## 常用指令

```
$ sudo gem install cocoapods
$ pod search 查找的库
$ vim Podfile
$ pod install
$ pod update
```



