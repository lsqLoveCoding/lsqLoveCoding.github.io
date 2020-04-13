---
layout:     post                    # 使用的布局（不需要改）
title:      git clone速度慢的怀疑人生的解决办法（win10系统）               # 标题 
subtitle:    #副标题
date:       2020-04-13              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 乱七八糟的教程
---

## git clone速度慢的怀疑人生的解决办法（win10系统）
&emsp;&emsp;第一步：查找域名对应的ip地址，在cmd下输入
```bash
nslookup github.global.ssl.fastly.Net
nslookup github.com 
```
&emsp;&emsp;第二步：在C:\Windows\System32\drivers\etc文件夹下修改hosts文件，最后一行加入：  

```txt
# GItHub
199.232.69.194 github.global.ssl.fastly.net
140.82.113.4 github.com
```
&emsp;&emsp;第三步：刷新dns地址。cmd下输入：  

```bash
ipconfig /flushdns
```
