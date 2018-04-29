---
layout: post
title:  "Top K Frequent Words"
date:   2016-04-12 17:28:30 +0800
categories: algorithm
---
# [Top K Frequent Words](http://www.lintcode.com/en/problem/top-k-frequent-words/)

@(算法)[算法, Hash Table, Heap, Priority Queue]

### 这一题怎样用一句话描述？

给出一个字符串数组，以及一个整数K，返回出现次数前K多的字符串（从多到少，如果出现次数一样，则按字母顺序从小到大）

### 用到什么算法？什么数据结构？

HashMap，Priority Queue

### 通过这题学到了什么？

这一题通过Priority Queue可以将额外空间消耗优化为O(K)，时间优化为O(NlogK)，另外注意这里的Priority Queue是最小元素在顶部的，这样才能不断顶走最小的，留下最大的。

### 可能(已经)遇到的BUG有？

没有将结果反向输出

