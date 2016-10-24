---
title: 本站多说评论和分享 CSS样式
date: 2016-10-24 11:30:34
categories:
- 建站
tags:
- 多说
- 建站
---

## 效果

![-w750](/images/14772800547694.jpg)


## 站点配置开启


```YAML
#多说评论
duoshuo_shortname: didee

# 多说分享服务
duoshuo_share: true
```

<!-- more -->

## 多说分享 CSS 样式

![-w287](/images/14772797359492.jpg)

修改配置文件为 :

```HTML
<div class="ds-share flat" data-thread-key="{{ page.path }}"
     data-title="{{ page.title }}"
     data-content=""
     data-url="{{ page.permalink }}">
  <div class="ds-share-inline">
    <ul  class="ds-share-icons-16">

      <li>
        <i class="fa fa-fw fa-share-alt"></i>
        分享到：</li>
      <li><a href="javascript:void(0);" data-service="weibo">
        <i class="fa fa-fw fa-weibo"></i>
      微博</a></li>
      <li><a href="javascript:void(0);" data-service="qzone">
        <i class="fa fa-fw fa-star-half-o"></i>
        QQ空间</a></li>
      <li><a href="javascript:void(0);" data-service="qqt">
        <i class="fa fa-fw fa-tencent-weibo"></i>
        腾讯微博</a></li>
      <li><a href="javascript:void(0);" data-service="wechat">
        <i class="fa fa-fw fa-weixin"></i>
        微信</a></li>

    </ul>
    <div class="ds-share-icons-more">
    </div>
  </div>
</div>
```

## 多说评论 CSS 样式

粘贴样式至此 :
![-w1124](/images/14772799228529.jpg)

```CSS
/* 所有样式 */
* {
    -webkit-border-radius: 0px !important;
    border-radius: 0px !important;
    box-shadow: inset 0 0 0px #fff !important;
}

/* 被顶起来的评论 */
#ds-reset .ds-gradient-bg {
    background: #E9E9E9 !important;
}

/* 评论框默认显示文字样式 */
#ds-thread #ds-reset .ds-textarea-wrapper textarea, #ds-thread #ds-reset .ds-textarea-wrapper .ds-hidden-text {
    font-family: "STHeiti", Verdana, sans-serif !important;
}

/* 发布评论按钮 */
#ds-thread #ds-reset .ds-post-button {
    font-family: "STHeiti", Verdana, sans-serif !important;
    border: 0 none !important;
    color: #FFFFFF !important;
    background: none repeat scroll 0 0 #bdc3c7 !important;
    cursor: pointer !important;
    display: inline-block !important;
    text-transform: none !important;
    transition: all 0.3s ease 0s !important;
    -moz-transition: all 0.3s ease 0s !important;
    -webkit-transition: all 0.3s ease 0s !important;
    text-align: center !important;
    text-shadow: 0 0px 0 #fff !important;
}

/* 发布评论按钮  hover */
#ds-thread #ds-reset .ds-post-button:hover {
    background: none repeat scroll 0 0 #7f8c8d !important;
    color: white !important;
}

/* 评论框边框 */
.theme-next #ds-thread #ds-reset .ds-textarea-wrapper {
    border-color: #DEDEDE !important;
}

/* 评论框下面框的边框(好绕啊！) */
#ds-thread #ds-reset .ds-post-toolbar {
    border: 1px solid #DEDEDE !important;
    background: #E9E9E9 !important;
}

/* 昵称 */
#ds-reset .ds-highlight {
    color: #F3726D !important;
}

/* 顶 */
#ds-thread #ds-reset .ds-post-liked a.ds-post-likes {
    color: #F3726D !important;
}

/* 发布旁边的“分享到”图标 */
#ds-reset .ds-service-icon {
    background-image: url("//static.duoshuo.com/images/service-icons-color-flat.png") !important;
    _background-image: url("//static.duoshuo.com/images/service-icons-color-flat.gif") !important;
}

/* 隐藏多说底部的版权 */
#ds-thread #ds-reset .ds-powered-by {
    display: none;
}

/*隐藏分享背景*/
#ds-share #ds-reset .ds-share-icons-16 .flat {
  background-image: none;
}

```


