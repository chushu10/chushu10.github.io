---
layout: post
title:  "Partition Array"
date:   2016-04-23 11:31:30 +0800
categories: algorithm
---
# [Partition Array](http://www.lintcode.com/en/problem/partition-array/)

@(算法)[算法, Two Pointers, Sort, Array]

### 这一题怎样用一句话描述？

给定数组和target值，划分数组，使得以target为分界线小于target的都在数组前面，大于等于target的都在数组后面

### 用到什么算法？什么数据结构？

两个指针的经典应用！我能一次AC了！数组数据结构

### 通过这题学到了什么？

简单题一次AC，中等题做出来=有Offer！

### 可能(已经)遇到的BUG有？

left和right指针相遇的时候才break循环，否则当target值正好大于数组中所有的数时，left的值正好等于最后一个数的下标，比答案正好少1

