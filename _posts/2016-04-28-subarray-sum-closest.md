---
layout: post
title: "Subarray Sum Closest"
date: 2016-04-28
categories: algorithm
---
# [Subarray Sum Closest](http://www.lintcode.com/en/problem/subarray-sum-closest/)

@(算法)[Subarray, Sort, Presum Array]

### 这一题怎样用一句话描述？

找出数组中和最接近0的子数组

### 用到什么算法？什么数据结构？

排序，数组

### 通过这题学到了什么？

暴力法肯定超时，参见这次提交的[代码](http://www.lintcode.com/submission/3961352/)，不过要是连暴力法都想不出来就GG了。然后就是参见了这篇[博客](http://www.cnblogs.com/easonliu/p/4504495.html)的方法，其实就是和[Subarray Sum](http://www.lintcode.com/en/problem/subarray-sum/)相同的思路，只不过这里要稍作改动：

1. 先求出数组的presum数组（先序和数组），并且**记录下标**
2. 按照sum从小到大排序
3. 找出相邻元素的sum之差绝对值最小的一对，按照下标从小到大输出即可（**注意**：小的那个下标要**+1**，因为我们初始化presum[0] =  (0, -1)，但是下标不可能为负数，所以要**+1**）

### 可能(已经)遇到的BUG有？

输出的小下标没有+1

