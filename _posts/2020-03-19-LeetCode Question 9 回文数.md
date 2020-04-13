---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode Question9 回文数               # 标题 
subtitle:    Java
date:       2020-03-19              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
---


**LeetCode Question #9 回文数**  
  
&emsp;&emsp;判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例1：**  
&emsp;&emsp;输入：121  
&emsp;&emsp;输出：true  
**示例2**  
&emsp;&emsp;输入：-121  
&emsp;&emsp;输出：false  
&emsp;&emsp;解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。  
**示例3**  
&emsp;&emsp;输入：10  
&emsp;&emsp;输出：false  
&emsp;&emsp;解释：从右向左读, 为 01 。因此它不是一个回文数。  
进阶:  
&emsp;&emsp;你能不将整数转为字符串来解决这个问题吗？

**题解：**  
```java
    public boolean isPalindrome(int x) {
        if (x < 0 || (x != 0 && x % 10 == 0))
            return false;
        int reverse = 0;
        while (reverse < x) {
            reverse = reverse * 10 + x % 10;
            x /= 10;
        }
        return x == reverse || x == reverse / 10;
    }
```








