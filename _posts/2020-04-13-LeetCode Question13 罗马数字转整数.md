---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode Question13 罗马数字转整数               # 标题 
subtitle:    Java
date:       2020-04-13              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
---


**LeetCode Question #13 罗马数字转整数**  
  
&emsp;&emsp;罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。  
```java
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
&emsp;&emsp;例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。  
&emsp;&emsp;通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

 - I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。  
 - X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。  
 - C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。  

&emsp;&emsp;给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。  
**示例1：**  
&emsp;&emsp;输入："III"  
&emsp;&emsp;输出：3  
**示例2：**  
&emsp;&emsp;输入："IV"  
&emsp;&emsp;输出：4  
**示例3：**  
&emsp;&emsp;输入："IX"  
&emsp;&emsp;输出：9  
**示例4：**  
&emsp;&emsp;输入："LVIII"  
&emsp;&emsp;输出：58  
&emsp;&emsp;解释：L = 50, V= 5, III = 3.  
**示例5：**  
&emsp;&emsp;输入："MCMXCIV"  
&emsp;&emsp;输出：1994  
&emsp;&emsp;解释：M = 1000, CM = 900, XC = 90, IV = 4.  
  
**题解：**  
&emsp;&emsp;HashMap匹配法：从右向左 进行迭代  
```java
import java.util.HashMap;
class Solution {
    public int romanToInt(String s) {
        if (s == null || s.length() == 0)
            return -1;
        HashMap<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        int len = s.length();
        int result = map.get(s.charAt(len - 1));
        for (int i = len - 2; i >= 0; i--) {
            if (map.get(s.charAt(i)) >= map.get(s.charAt(i + 1)))
                result += map.get(s.charAt(i));
            else
                result -= map.get(s.charAt(i));
        }
        return result;
    }
}
```
