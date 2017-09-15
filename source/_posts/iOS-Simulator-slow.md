---
title: 一条命令解决 10.12 下 iOS模拟器滑动卡顿
date: 2015-11-10 17:54:34
categories:
- Xcode
tags:
- iOS-Simulator
---

```sh
$ sudo sysctl -w kern.timer.coalescing_enabled=0
```

执行成功结果
<img src="/images/14787719452597.jpg" width="447"/>



