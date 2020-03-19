---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode Question7 整数反转               # 标题 
subtitle:    Java
date:       2020-03-19              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - Java
---  
  
**LeetCode Question #7 整数反转**  
  
&emsp;&emsp;给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。 

**示例1：**  
&emsp;&emsp;输入：123  
&emsp;&emsp;输出：321  
**示例2**  
&emsp;&emsp;输入：-123  
&emsp;&emsp;输出：-321  
**示例3**  
&emsp;&emsp;输入：120  
&emsp;&emsp;输出：21  
注意:  
&emsp;&emsp;假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

**题解：**  
```java
    public int reverse(int x) {
        if (x == 0)
            return 0;
        long result = 0;
        while (x != 0) {
            result = result * 10 + x % 10;
            x /= 10;
        }
        if (result > Integer.MAX_VALUE)
            return 0;
        if (result < Integer.MIN_VALUE)
            return 0;
        return (int) result;
    }
```
