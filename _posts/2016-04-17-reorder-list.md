---
layout: post
title:  "Reorder List"
date:   2016-04-17 11:15:30 +0800
categories: algorithm
---
# [Reorder List](http://www.lintcode.com/en/problem/reorder-list/)

@(算法)[算法, Linked List]

### 这一题怎样用一句话描述？

将链表按照第一个、最后一个、第二个、倒数第二个、第三个、倒数第三个……以此类推，这样的顺序重新排列

### 用到什么算法？什么数据结构？

暴力法，链表

### 通过这题学到了什么？

基础很**重要**！很**重要**！很**重要**！重要的事说三遍。合并的写法一定要规范，错误的写法如下：

~~~

        prev.next = left;
        prev = prev.next;
			
        prev.next = right;
        prev = prev.next;
			
	    left = left.next;
        right = right.next;
        
~~~

正确的写法如下：

~~~

        prev.next = left;
        prev = prev.next;
        left = left.next;
            
        prev.next = right;
        prev = prev.next;
        right = right.next;
        
~~~

看出其中的区别了吗？对，就是left指针和right指针后移的顺序问题！如果不在prev = prev.next的时候就马上后移，那么此时prev指向的就是left，此时再将prev.next赋值为right，就等同于将left.next也指向了right，基础很**重要**呀！也可以参照九章的[*写法*](http://www.jiuzhang.com/solutions/reorder-list/)

最后一步通过快慢指针找中点时记得，此时的函数cutOutLastHalfOfLinkedList返回值是void，所以直接return就行了

### 可能(已经)遇到的BUG有？

以上

