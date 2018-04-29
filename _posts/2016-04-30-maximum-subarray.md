---
layout: post
title: "Maximum Subarray"
date: 2016-04-30
categories: algorithm
---
# [Maximum Subarray](http://www.lintcode.com/en/problem/maximum-subarray/)

@(算法)[算法, Greedy, Enumeration, LinkedIn, Subarray, Array, Linkedin]

### 这一题怎样用一句话描述？

找出数组中的和最大的子数组(连续)

### 用到什么算法？什么数据结构？

动态规划，数组

### 通过这题学到了什么？

这一题，首先问法就是“找到……的最优解”，可以考虑使用动归。动归的思路如下：

设 f[i] 表示以下标为 i 的元素结尾的所有子数组里，数组和最大的那个的值。这一题是典型的坐标型动态规划，因为有“以……结尾”这一句话。那么 f[i] 的值由谁决定呢？很容易想到，f[i] 有可能等于 f[i - 1] + nums[i]，f[i] 不可能等于 f[i - 1]，因为数组必须连续。但是如果 f[i - 1] + nums[i] 比 nums[i] 本身还要小，还不如直接取 nums[i]，所以就又了递推方程：

```
	f[i] = {f[i - 1] + nums[i], f[i - 1] + nums[i] > nums[i]}
	f[i] = {nums[i], f[i - 1] + nums[i] <= nums[i]}
```

然后在所有的 f[i] 中找到最大值即可

### 可能(已经)遇到的BUG有？

递推公式想错。采用暴力法行不通。

