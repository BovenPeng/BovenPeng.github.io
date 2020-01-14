---
title: leetcode题解-102-二叉树的层次遍历
date: 2019-12-05 19:35:54
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- BFS
- ×
---



## [题目](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

### My way

#### 手动队列**BFS**

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
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        deque<TreeNode*> q;
        q.push_back(root);

        while(!q.empty())
        {
            vector<int> vi;

            int size = q.size();
            for (int i = 0; i < size; ++i)
            {
                auto front = q.front();
                q.pop_front();

                vi.push_back(front->val);
                if (front->left)   q.push_back(front->left);
                if (front->right)  q.push_back(front->right);
            }

            vvi.push_back(vi);
        }

        return vvi;
    }
};
```

时间:$O(N)$, 空间:$O(N)$



### 官方way

#### 递归

```java
class Solution {
    List<List<Integer>> levels = new ArrayList<List<Integer>>();

    public void helper(TreeNode node, int level) {
        // start the current level
        if (levels.size() == level)
            levels.add(new ArrayList<Integer>());

         // fulfil the current level
         levels.get(level).add(node.val);

         // process child nodes for the next level
         if (node.left != null)
            helper(node.left, level + 1);
         if (node.right != null)
            helper(node.right, level + 1);
    }
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return levels;
        helper(root, 0);
        return levels;
    }
}
```

时间:$O(N)$, 空间:$O(N)+O(logN)= O(N)$，因为递归自动压栈需要维护一个栈，故空间上常数项系数更大

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
class Solution {
private:
    void helper(vector<vector<int>>& vvi, int level, TreeNode* root)
    {
        // add new vector<int> for saving new level elements
        if (vvi.size() == level)
        {
            vvi.push_back(vector<int>());
        }

        vvi[level].push_back(root->val);
        if(root->left)     helper(vvi, level+1, root->left);
        if(root->right)    helper(vvi, level+1, root->right);
    }
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        if (!root)  return vector<vector<int>>();

        vector<vector<int>> vvi;
        helper(vvi, 0, root);

        return vvi;   
    }
};
```

优化递归函数`helper`后：

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
class Solution {
private:
    void helper(vector<vector<int>>& vvi, int level, TreeNode* root)
    {
        if (!root)  return ;

        // add new vector<int> for saving new level elements
        if (vvi.size() == level)
        {
            vvi.push_back(vector<int>());
        }

        vvi[level].push_back(root->val);
        helper(vvi, level+1, root->left);
        helper(vvi, level+1, root->right);
    }
public:
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        vector<vector<int>> vvi;
        helper(vvi, 0, root);

        return vvi;   
    }
};
```



## 总结

错误点：

- 需要提前用哨兵记住队列的长度`size`， 否则在遍历对应层时，长度不断变大，影响结果
- 对入队的结点来说，只需要入队左右子树非空结点即可。
- 递归方式BFS实现层次遍历时，需要在`vector<vector<int>>`的对应层数上存储，所以在`helper`函数上，需要传参



## Reference

https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/er-cha-shu-de-ceng-ci-bian-li-by-leetcode/

https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/er-cha-shu-ceng-ci-bian-li-c-by-huang-fu-mai-yan/