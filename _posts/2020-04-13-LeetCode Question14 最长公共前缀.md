---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode Question14 最长公共前缀               # 标题 
subtitle:    Java
date:       2020-04-13              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
---



**LeetCode Question #14 最长公共前缀**  
  
&emsp;&emsp;编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。    
**示例1：**  
&emsp;&emsp;输入：["flower","flow","flight"]  
&emsp;&emsp;输出：fl  
**示例2：**  
&emsp;&emsp;输入：["dog","racecar","car"]  
&emsp;&emsp;输出：""  
&emsp;&emsp;解释：输入不存在公共前缀。  

  
**题解：**  
&emsp;&emsp;巧用startsWith()函数  
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int count = strs.length;
        String prefix = "";
        if (count != 0 ) {
            prefix = strs[0];
        }
        for (int i = 1; i < count; i++) {
            while (!strs[i].startsWith(prefix)) {
                prefix = prefix.substring(0, prefix.length() - 1);
            }
        }
        return prefix;
    }
}
```
