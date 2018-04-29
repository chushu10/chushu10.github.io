---
layout: post
title:  "Rehashing"
date:   2016-04-13 17:41:30 +0800
categories: algorithm
---
# [Rehashing](http://www.lintcode.com/en/problem/rehashing/)

@(算法)[算法, Hash Table]

### 这一题怎样用一句话描述？

这是一道模拟题，说的是hash表的长度是不定的，当有两个元素被hash函数map到同一个位置时，就要把hash表的长度加倍。这时候需要我们返回rehashing后的hash表

### 用到什么算法？什么数据结构？

暴力，hash map

### 通过这题学到了什么？

hash map的迭代操作，有两种，第一种是可以同时拿到hash map中某一个位置的Key以及Value，参见Java官方[文档](http://docs.oracle.com/javase/7/docs/api/java/util/Map.Entry.html)，代码如下：

```
for (Map.Entry<K, V> entry : map.entrySet()) {
  result[entry.getKey()] = entry.getValue();
}
```

另一种只能遍历Key，如果要获取Value就再使用一次map的get方法，代码如下：

```
for (K key : map.keySet()) {
  result[key] = map.get(key);
}
```

第二种方法没有用到Map.Entry<K, V>，没法同时取到Key和Value，效率偏低（因为还要再使用一次map的get方法嘛）。

### 可能(已经)遇到的BUG有？

无。

