# 二叉树的迭代遍历

递归版的二叉树遍历十分简单，可谓名副其实的 Clean Code。如果不是为了面试，恐怕没有几个人会选择使用迭代来完成二叉树的前序、中序、后序遍历。

现场手写迭代版本的二叉树遍历并非易事，所以本文旨在找出最优、最容易理解的算法模板，帮助大家记忆迭代版本的代码。

# 前序和后序遍历

首先，我们来整理一下前序遍历迭代版的思路。前序遍历的顺序为：

根 -> 左 -> 右

即访问过根节点之后，先访问左节点再访问右节点，根据栈 FILO（先进后出）的特性，那么再访问完根节点后，先将右子节点压栈，再将左子节点压栈即可。迭代版的代码就不难写出了：

```java
// 以 Java 为例
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        List<Integer> result = new LinkedList<>();
        
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (cur == null) {
                continue;
            }
            result.add(cur.val);
            stack.push(cur.right);
            stack.push(cur.left);
        }
        
        return result;
    }
}
```

把前序和后序放到一起讲，是因为它们俩是有关联的。参考[这篇](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/solution/die-dai-jie-fa-shi-jian-fu-za-du-onkong-jian-fu-za/) LeetCode 题解：

> 前序遍历顺序为：根 -> 左 -> 右
> 
> 后序遍历顺序为：左 -> 右 -> 根
> 
> 如果1：我们将前序遍历中节点插入结果链表尾部的逻辑，修改为将节点插入结果链表的头部
> 
> 那么结果链表就变为了：右 -> 左 -> 根
> 
> 如果2：我们将遍历的顺序由从左到右修改为从右到左，配合如果1
> 
> 那么结果链表就变为了：左 -> 右 -> 根
> 
> 这刚好是后序遍历的顺序

因此，我们仅需在前序遍历代码的基础上，微调几行，即可写出后序遍历的迭代版代码（注释标注出了微调的代码行）：

```java
// 以 Java 为例
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        LinkedList<Integer> result = new LinkedList<>(); // 我们需要直接使用 LinkeList，为的是它的 addFirst 方法
        
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (cur == null) {
                continue;
            }
            result.addFirst(cur.val); // 如果1：我们将前序遍历中节点插入结果链表尾部的逻辑，修改为将节点插入结果链表的头部
            stack.push(cur.left); // 如果2：我们将遍历的顺序由从左到右修改为从右到左，配合如果1
            stack.push(cur.right); // 如果2：我们将遍历的顺序由从左到右修改为从右到左，配合如果1
        }
        
        return result;
    }
}
```

# 中序遍历

我认为中序遍历，是三种遍历里最难的，因为它的解法不那么直观，但是如果我们看一下中序遍历的递归版（请用真实的栈，模拟一下递归算法的执行过程）：

```java
// 以 Java 为例
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return new LinkedList<>();
        }
        
        List<Integer> result = new LinkedList<>(inorderTraversal(root.left));
        result.add(root.val);
        result.addAll(inorderTraversal(root.right));
        
        return result;
    }
}
```

我们可以从中发现，中序遍历，首先要对从根节点开始的一系列“左子树”进行压栈处理，直到再也没有左子树可以压栈。此时，我们访问树上“最左边的”节点，然后再对它的右子树（如果有）做相同的处理。下面给出实现代码：

```java
// 以 Java 为例
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<>();
        List<Integer> result = new LinkedList<>();
        
        while (root != null || !stack.isEmpty()) {
            if (root != null) {
                stack.push(root);
                root = root.left;
                continue;
            }
            // root == null
            root = stack.pop();
            result.add(root.val);
            root = root.right;
        }
        
        return result;
    }
}
```

这里 `root` 变量的使用十分巧妙，通过判断 `root == null`，便可以知道当前节点是否还有左子树可以访问。这里建议大家用简单的用例走读一遍代码，或者使用 IDE 的 Debugger 调试一遍，以加深对算法的理解。


