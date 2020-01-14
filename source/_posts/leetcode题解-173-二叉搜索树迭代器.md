---
title: leetcode题解-173-二叉搜索树迭代器
mathjax: true
date: 2019-12-17 15:37:21
categories:
tags:
- ×
---

## [题目](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

### [windliang](https://leetcode-cn.com/problems/binary-search-tree-iterator/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-4-4/)

#### 队列保存了所有的节点值

##### java实现

```java
class BSTIterator {

    Queue<Integer> queue = new LinkedList<>();

    public BSTIterator(TreeNode root) {
        inorderTraversal(root);
    }

    private void inorderTraversal(TreeNode root) {
        if (root == null) {
            return;
        }
        inorderTraversal(root.left);
        queue.offer(root.val);
        inorderTraversal(root.right);
    }

    /** @return the next smallest number */
    public int next() {
        return queue.poll();
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !queue.isEmpty();
    }
}
```

##### C++实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
private:
    queue<int> q;

    void inOrder(TreeNode* root)
    {
        if (!root)  return;
        inOrder(root->left);
        q.push(root->val);
        inOrder(root->right);
    }

public:
    BSTIterator(TreeNode* root) 
    {
        inOrder(root);
    }
    
    /** @return the next smallest number */
    int next() 
    {
        auto front = q.front(); q.pop();
        return front;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() 
    {
        return !q.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```



#### 要控制中序遍历的进程，一个一个输出

##### java实现

```java
class BSTIterator {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode cur = null;

    public BSTIterator(TreeNode root) {
        cur = root;
    }

    /** @return the next smallest number */
    public int next() {
        int res = -1;
        while (cur != null || !stack.isEmpty()) {
            // 节点不为空一直压栈
            while (cur != null) {
                stack.push(cur);
                cur = cur.left; // 考虑左子树
            }
            // 节点为空，就出栈
            cur = stack.pop();
            res = cur.val;
            // 考虑右子树
            cur = cur.right;
            break;
        }

        return res;
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return cur != null || !stack.isEmpty();
    }
}
```

##### C++实现

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
private:
    stack<TreeNode*> s;
    TreeNode* cur = nullptr;
public:
    BSTIterator(TreeNode* root) 
    {
        cur = root;
    }
    
    /** @return the next smallest number */
    int next() 
    {
        int res = -1;
        while (cur || !s.empty())
        {
            while (cur != nullptr)
            {
                s.push(cur);
                cur = cur->left;
            }

            cur = s.top(); s.pop();
            res = cur->val;

            cur = cur->right;
            break;
        }

        return res;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return cur || !s.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```



## 总结

- 利用 **二叉搜索树** 的 **中序遍历** 是 **升序序列** 的特点，可以提前将树的结构以中序遍历结果存储起来
- 