---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode Question1 两数之和               # 标题 
subtitle:    Java
date:       2020-03-19              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - Java
---


**LeetCode Question #1 两数之和**  
  
&emsp;&emsp;给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那**两个**整数，并返回它们的**数组下标**。  
&emsp;&emsp;你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例：**  
&emsp;&emsp;给定 nums = [2, 7, 11, 15], target = 9  
&emsp;&emsp;因为 nums[0] + nums[1] = 2 + 7 = 9  
&emsp;&emsp;所以返回 [0, 1]  

**题解：**  
1.暴力法  
&emsp;&emsp;直接两遍循环查找是否存在数组中存在两个目标元素之和等于target

```java
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] {i, j};
                }
            }
        }
        throw new IllegalArgumentException("no solution!");
    }
```
复杂度分析：  
&emsp;&emsp;时间复杂度：O(n²)  
&emsp;&emsp;空间复杂度：O(1)
***  
2.两遍哈希表  
&emsp;&emsp;为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。  
&emsp;&emsp;通过以空间换取速度的方式。一个简单的实现使用了两次迭代。在第一次迭代中，我们将每个元素的值和它的索引添加到表中。然后，在第二次迭代中，我们将检查每个元素所对应的目标元素是否存在于表中。注意，该目标元素不能是元素本身！
```java
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            int a = target - nums[i];
            if (map.containsKey(a) && map.get(a) != i) {
                return new int[] {i, map.get(a)};
            }
        }
        throw new IllegalArgumentException("no solution!");
    }
```
复杂度分析：  
&emsp;&emsp;时间复杂度：O(n)  
&emsp;&emsp;空间复杂度：O(n)
***  
3.一遍哈希表  
&emsp;&emsp;事实证明，我们可以一次完成。在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

```java
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int a = target - nums[i];
            if (map.containsKey(a) && map.get(a) != i) {
                return new int[] {map.get(a), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("no solution!");
    }
```
复杂度分析：  
&emsp;&emsp;时间复杂度：O(n)  
&emsp;&emsp;空间复杂度：O(n)







