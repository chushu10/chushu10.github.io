---
layout: post
title:  "Longest Consecutive Sequence"
date:   2016-04-14 16:15:30 +0800
categories: algorithm
---
# [Longest Consecutive Sequence](http://www.lintcode.com/en/problem/longest-consecutive-sequence/)

@(算法)[算法, Hash Table, Array]

### 这一题怎样用一句话描述？

给出一个无序数组，找出最长的连续序列

### 用到什么算法？什么数据结构？

HashSet

### 通过这题学到了什么？

这一题首先可以排序（O(nlogn)）然后再遍历一次排序后的数组（O(n)）。

不过[题目](http://www.lintcode.com/en/problem/longest-consecutive-sequence/)的要求是O(n)的时间。

这里用到了hash表，不过可以不用HashMap类了，因为不需要Key-Value了，只需要知道hash表里有没有没有某个整数存在，所以直接使用HashSet。

首先选定一个数作为起点，可以随机选，也可以就选第一个数。从这个数出发，分别向上，以及向下，寻找连续的数，当两端都走到头时，把走过的数的个数再加上起点本身，就是目前找到的最长的连续序列。此时，已经访问过的数应该从HashSet中remove掉，remove的时间是O(1)，所以我们总共只需要访问每个数一次就能找出最长的连续序列。所以总共的时间是O(n)。

### 可能(已经)遇到的BUG有？

再向下找的时候千万不要犯二，把down也做自减操作了……

```
int down = 0;
curNum = num[i];
while (set.contains(curNum - 1)) {
    set.remove(curNum - 1);
    down++; // not down--
    curNum--;
}
```


