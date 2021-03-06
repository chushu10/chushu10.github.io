---
layout: post
title: "Level Order Traversal of Binary Tree"
date: 2018-03-20
categories: algorithm
img: algorithm/queue.jpg
mathjax: true
---

# How to summarize this question in one sentence？

Using BFS (Breadth-First Search) to traverse the binary tree in level order.

# What algorithm(s) and data structure(s) are involved?

- Algorithm: BFS (Breadth-First Search)
- Data structure: Queue

For example, as the binary tree showed in the graph below:

![level order traversal]({{site.baseurl}}/assets/img/algorithm/level_order.png)

The level order traversal of this binary tree should be:

```bash
[20],
[9, 49],
[5, 12, 23, 52],
[15, 50]
```

# Thought(s) and Solution(s)?

The thoughts behind this question is simple, you have to visit all the nodes in binary tree level by level. Just like knocking the doors of every family from top to bottom.

However, the building containing all the families is like a pyramid, where the top level contains only one family which means you can only knock one door.

Imagine you are the one who is going to knocking all those families' doors, you will be moving just like this:

![level order traversal2]({{site.baseurl}}/assets/img/algorithm/level_order2.png)

See? You will be following a zig-zag path, and that is the solution!

But how can we tell computer to follow a zig-zag path when traversing the binary tree, and record every level in an array, and finally return an array or arrays?

So better have a list, like shopping list. When we have put an item into the cart, we remove that item from our list. And what kind of list we can use on computer? We rely on queue. Using queue, we can store the node(s) we will be visiting next, and after visited a node, we will remove it. It's that simple, let me show you that:

1. Initialization: put the root node '20' into bottom of the queue (It's top of the queue the same time); New a big array $ A $ to contain all the arrays (which representing all the levels).
2. If the queue is not empty, go to 3; Else, go to 6.
3. Record the queue size $ s $, and new an array $ a $ of $ s $ length.
4. For $ s $ times, do this: remove the node from top of the queue, record it into array $ a $, and put it's left child (if exists) and right child (if exists) into the bottom of the queue.
5. Put the small array $ a $ into the big array $ A $, and go back to 2.
6. Array $ A $ is the right answer.

You can make a draft on your draft paper like this:
![level order traversal3]({{site.baseurl}}/assets/img/algorithm/level_order3.png)
![level order traversal4]({{site.baseurl}}/assets/img/algorithm/level_order4.png)

## What have you learned from this question?

BFS is always accompanied by queue, this relation is unbreakable.

# The BUG(s) found?

Pay attention to memory leakage, try memory check tool of [Valgrind](http://valgrind.org/) to detect that.

# My Solution

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

struct QueueNode {
    struct TreeNode *treeNode;
    struct QueueNode *next;
};

struct Queue {
    int size;
    struct QueueNode *head;
    struct QueueNode *tail;
};

struct Queue* init() {
    struct Queue* queue = (struct Queue*)malloc(sizeof(struct Queue));
    queue->head = NULL;
    queue->tail = NULL;
    queue->size = 0;
    return queue;
}

struct Queue* enqueue(struct Queue* queue, struct TreeNode* treeNode) {
    struct QueueNode *tmp = (struct QueueNode*)malloc(sizeof(struct QueueNode));
    if (NULL == tmp) {
        return NULL;
    }
    tmp->treeNode = treeNode;
    tmp->next = NULL;
    if (NULL != queue->tail) {
        queue->tail->next = tmp;
    } else {
        queue->head = tmp;
    }
    queue->tail = tmp;
    queue->size += 1;
    return queue;
}

struct Queue* dequeue(struct Queue* queue, struct TreeNode** treeNode) {
    struct QueueNode *tmp = queue->head;
    *treeNode = queue->head->treeNode;
    if (queue->head != queue->tail) {
        queue->head = queue->head->next;
    } else {
        queue->head = queue->tail = NULL;
    }
    free(tmp);
    queue->size -= 1;
    return queue;
}

int empty(struct Queue* queue) {
    return queue->size == 0 ? 1 : 0;
}

/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** levelOrder(struct TreeNode* root, int** columnSizes, int* returnSize) {
    if (NULL == root) {
        return NULL;
    }

    *returnSize = 0;
    struct Queue *queue = init();
    struct TreeNode *item;
    int **result = NULL;

    queue = enqueue(queue, root);
    while(!empty(queue)) {
        int queue_size = queue->size;
        int *sub_array = (int*)malloc(queue_size * sizeof(int));
        for (int i = 0; i < queue_size; ++i)
        {
            queue = dequeue(queue, &item);
            sub_array[i] = item->val;
            if (item->left) {
                queue = enqueue(queue, item->left);
            }
            if (item->right) {
                queue = enqueue(queue, item->right);
            }
        }
        *returnSize += 1;
        result = (int**)realloc(result, *returnSize * sizeof(int*));
        result[*returnSize - 1] = sub_array;
        *columnSizes = (int*)realloc(*columnSizes, *returnSize * sizeof(int));
        (*columnSizes)[*returnSize - 1] = queue_size;
    }

    return result;
}
```