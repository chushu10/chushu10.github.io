---
layout: post
title: "Construct Binary Tree From In-order and Post-order Traversal"
date: 2018-08-26
categories: algorithm
mathjax: false
---

# 问题描述

通过二叉树的中序和后序遍历结果，构造这棵二叉树。

注意：
树中不存在值重复的节点。

例如，给出如下中序和后序遍历结果：

```cpp
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

构造出以下二叉树：

```bash
    3
   / \
  9  20
    /  \
   15   7
```

# 解决思路

二叉树的后续遍历是按照“左-右-根”的顺序访问树中的所有节点的，因此后续遍历结果的最后一个值必然是整棵树的根结点，以后续遍历 `postorder = [9,15,7,20,3]` 为例，3 就是整棵树的根结点了。而二叉树的中序遍历是按照“左-根-右”的顺序访问树中的所有节点的，所以根在中序遍历结果的中间。

我们可以根据后续遍历得到的根节点，将中序遍历的结果分成左子树和右子树。以中序遍历 `inorder = [9,3,15,20,7]` 为例，由于之前通过后续遍历知道了 3 是根节点，因此左子树是一棵只包含了 9 一个节点的树，而右子树则是一棵包含了 15、20、7 三个节点的树。

是不是又联想到递归了？没错，我们可以将构建整棵树的任务分解成构建左子树和右子树两个任务，左子树的构建又可以分解成构建“左左子树”和“左右子树”两个任务，右子树的构建同样地又可以分解成构建“右左子树”和“右右子树”两个任务，如此往复。

最后，给出我的代码如下：

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) {
            return NULL;
        }
        // The last element of postorder is the value of the root
        int rootVal = *(postorder.end() - 1);
        TreeNode *root = new TreeNode(rootVal);
        
        // Locate the middle of inorder
        vector<int>::iterator mid = std::find(inorder.begin(), inorder.end(), rootVal);
        // if (mid == inorder.begin()) {
        //     return root;
        // }
        
        // Try to build left Tree recursively
        vector<int> leftInOrder(inorder.begin(), mid);
        vector<int> leftPostOrder(postorder.begin(), postorder.begin() + (mid - inorder.begin()));
        root->left = buildTree(leftInOrder, leftPostOrder);
        
        // Try to build right Tree recursively
        vector<int> rightInOrder(mid + 1, inorder.end());
        vector<int> rightPostOrder(postorder.begin() + (mid - inorder.begin()), postorder.end() - 1);
        root->right = buildTree(rightInOrder, rightPostOrder);
        return root;
    }
};
```

可惜的是，这一份代码的运行效率并不高（仅仅击败了 15% 的 C++ 提交），我想可能是每一次递归都初始化了很多 `vector<int>` 类型，浪费了很多时间，可以考虑优化一下。