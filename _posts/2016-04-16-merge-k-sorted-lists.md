---
layout: post
title:  "Merge k Sorted Lists"
date:   2016-04-16 11:15:30 +0800
categories: algorithm
---
# [Merge k Sorted Lists](http://www.lintcode.com/en/problem/merge-k-sorted-lists/)

@(算法)[算法, Divide and Conquer, Linked List, Priority Queue, Heap, Uber, Google, Linkedin, Airbnb, Facebook]

### 这一题怎样用一句话描述？

合并K个排序列表，形成一个排序列表

### 用到什么算法？什么数据结构？

暴力法，链表

### 通过这题学到了什么？

代码的质量很重要，要尽量做到一次AC，这一题我CE了3次，TLE了2次，WA了3次，最终才AC。要记住，企业级的代码不允许这样的质量，你需要保证自己提交的代码正确无误，否则造成的损失不可估量

建议参照九章的[*答案*](http://www.jiuzhang.com/solutions/merge-k-sorted-lists/)，使用分支等算法做一遍

### 可能(已经)遇到的BUG有？

找最小时忘记给minimum赋值，循环的结束条件处理不当

