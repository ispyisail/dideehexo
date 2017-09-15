---
title: 自签名使 Xcode8 支持三方插件
date: 2015-10-18 12:54:34
categories:
- Xcode
tags:
- Xcode8
- Plugin
---
### 获取签名

Xcode8 有自动管理签名功能,按照XVim作者的方法[INSTALL_Xcode8](https://github.com/XVimProject/XVim/blob/master/INSTALL_Xcode8.md),可以创建一个XcodeSigner用于签名Xcode<!-- more -->
但是,使用 XcodeSigner 证书就无法使用自己的开发者帐号进行调试了,解决方法是`用自己的开发者证书签名 Xcode`

首先终端输入
```
$ security find-identity -p codesigning
```

得到名称为 `iPhone Developer` 的 `IdentifyID`


### 重新签名 

至 Xcode 安装目录,执行签名即可,此步骤时长根据性能不等

```
$ codesign -f -s <IdentifyID> Xcode.app
```


或直接


```
$ sudo codesign -f -s <IdentifyID>  /Applications/Xcode.app
```

### Xcode8插件

合集
[xcode-8-extensions](https://theswiftdev.com/2016/08/17/xcode-8-extensions/)


