---
layout: post
title:  "Ugly Number II"
date:   2016-04-20 11:31:30 +0800
categories: algorithm
---
# [Ugly Number II](http://www.lintcode.com/en/problem/ugly-number-ii/)

@(算法)[算法, Priority Queue]

### 这一题怎样用一句话描述？

找出第N个丑数（丑数的定义是：它只能有2，3，5这三个因子，也就是说只能被2，3，5或者2，3，5的倍数整除）

### 用到什么算法？什么数据结构？

用了不准备肯定想不到的方法，数组。

### 通过这题学到了什么？

又是一道不准备就肯定做不出来的题目，首先丑数的定义就很难理解，或者说很难用程序实现。什么叫“只能被2，3，5或者2，3，5的倍数整除”呢？为了不把脑子烧糊，我还是决定用一个更简单，并且效率更高的方法做，只要看一下下面这三个序列就能明白：

```
1 X 2  2 X 2  3 X 2  4 X 2  5 X 2  6 X 2...
1 X 3  2 X 3  3 X 3  4 X 3  5 X 3  6 X 3...
1 X 5  2 X 5  3 X 5  4 X 5  5 X 5  6 X 5...
```

因为每一个丑数，都可以通过乘2、乘3或者乘5的到，所以，我们用1乘2、乘3或者乘5就得到了2、3、5，用2乘2、乘3或者乘5就得到了4、6、10，以此类推……它们的到的结果都是丑数，而我们只需要每次拿到结果中最小的那个，再用它乘2、乘3或者乘5就又能得到3个候选值，然后再取最小，再乘2、乘3或者乘5……

代码如下：

```
public int nthUglyNumber(int n) {
  // Write your code here
  List<Integer> uglyNumber = new ArrayList<>(n);
  uglyNumber.add(1);
  int i2 = 0, i3 = 0, i5 = 0;
  while (uglyNumber.size() < n) {
    int m2 = uglyNumber.get(i2) * 2;
    int m3 = uglyNumber.get(i3) * 3;
    int m5 = uglyNumber.get(i5) * 5;
    int mn = Math.min(m2, Math.min(m3, m5));
    if (mn == m2) i2++;
    if (mn == m3) i3++;
    if (mn == m5) i5++;
    uglyNumber.add(mn);
  }
  return uglyNumber.get(n - 1);
}
```

好吧，其实代码基本上是照抄的这个[博客](http://www.cnblogs.com/grandyang/p/4743837.html)

### 可能(已经)遇到的BUG有？

不准备就肯定跪的题，哪来的BUG？


