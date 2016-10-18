---
title: 自签名使 Xcode8 支持三方插件
date: 2016-10-18 12:54:34
categories:
- Xcode
tags:
- Xcode8
---
### 获取签名

Xcode8 有自动管理签名功能,按照XVim作者的方法[INSTALL_Xcode8](https://github.com/XVimProject/XVim/blob/master/INSTALL_Xcode8.md),可以创建一个XcodeSigner用于签名Xcode,但是使用 Xcode 时就无法再使用自己的证书了,解决方法是用自己的开发者证书签名 Xcode
<!-- more -->
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
$ codesign -f -s <IdentifyID>  /Applications/Xcode.app
```

### Xcode8插件

[xcode-8-extensions](https://theswiftdev.com/2016/08/17/xcode-8-extensions/)


