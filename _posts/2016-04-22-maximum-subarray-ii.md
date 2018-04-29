---
layout: post
title:  "Maximum Subarray II"
date:   2016-04-22 11:31:30 +0800
categories: algorithm
---
# [Maximum Subarray II](http://www.lintcode.com/en/problem/maximum-subarray-ii/)

@(算法)[算法, Greedy, Enumeration, Forward-Backward Traversal, Subarray, Array, Dynamic Programming]

### 这一题怎样用一句话描述？

找出数组中两个不重叠的子数组，使其和最大

### 用到什么算法？什么数据结构？

动归，数组

### 通过这题学到了什么？

这一题完美用到了[Maximum Subarray](http://www.lintcode.com/en/problem/maximum-subarray/)一题的方法，可以将其解作为库函数用到本题中。

只要遍历数组所有的分割线，找出分割线左右的Maximum Subarrray，最后将它们相加就是最终解。

参考这一篇[博客](http://www.cnblogs.com/FLAGyuri/p/5347267.html)

### 可能(已经)遇到的BUG有？

只要[Maximum Subarray](http://www.lintcode.com/en/problem/maximum-subarray/)不写错，这一题就不会写错，快快复习一下Maximum Subarray吧！

