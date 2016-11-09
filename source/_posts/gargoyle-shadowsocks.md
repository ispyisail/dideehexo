---
title: 石像鬼Gargoyle-1.9.1原版 安装 Shadowsocks 教程
date: 2016-10-26 11:30:34
categories:
- OpenWrt
tags:
- Gargoyle
- Shadowsocks
---

## Gargoyle

<img src="/images/14774559124286.jpg" width="184"/>
<!-- more -->
[https://www.gargoyle-router.com/](https://www.gargoyle-router.com/)

---

## 上传ipk
### 下载相关文件并解压
[gargoyle-ss.zip](https://didee.cn/files/gargoyle-ss.zip)


### 包含文件
<img src="/images/Snip.png" width="284"/>


### 上传文件夹到路由器


```bash
$scp -r  /Path/gargoyle-ss root@192.168.1.1:/tmp
```
密码为 Gargoyle 的前台登录密码

---

## 安装

### SSH到路由器

```bash
$ssh root@192.168.1.1 
```

### 进入/tmp

```bash
$cd /tmp
```

### 安装

```bash
$opkg update
$opkg install -w shadowsocks-libev_2.5.5-1_ar71xx.ipk
$opkg install -w pdnsd.ipk
$opkg install -w shadowsocks-ui.ipk
$opkg update
```
可选shadowsocksr-libev_2.4.5-6pre_ar71xx.ipk

### 重启

```sh
$reboot
```

---

## 配置

### 根据 ss 或 ssr 选择

### 其他参考
[（8.23更新） 石像鬼也可以玩SS 支持SS原版和SSR](http://www.right.com.cn/FORUM/thread-191582-1-1.html)

