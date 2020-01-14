---
title: leetcode题解-606-根据二叉树创建字符串
date: 2019-12-09 21:25:07
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- 栈
- 括号匹配
- stringstream
- ×
---



## [题目](https://leetcode-cn.com/problems/construct-string-from-binary-tree/)

### [官方](https://leetcode-cn.com/problems/construct-string-from-binary-tree/solution/gen-ju-er-cha-shu-chuang-jian-zi-fu-chuan-by-leetc/)

#### 递归实现

考虑4种情况：

1. 如果当前节点有两个孩子，那我们在递归时，需要在两个孩子的结果外都加上一层括号；
2. 如果当前节点没有孩子，那我们不需要在节点后面加上任何括号；
3. 如果当前节点只有左孩子，那我们在递归时，只需要在左孩子的结果外加上一层括号，而不需要给右孩子加上任何括号；
4. 如果当前节点只有右孩子，那我们在递归时，需要先加上一层空的括号 `()` 表示左孩子为空，再对右孩子进行递归，并在结果外加上一层括号。

##### java实现

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public String tree2str(TreeNode t) {
        if(t==null)
            return "";
        if(t.left==null && t.right==null)
            return t.val+"";
        if(t.right==null)
            return t.val+"("+tree2str(t.left)+")";
        return t.val+"("+tree2str(t.left)+")("+tree2str(t.right)+")";   
    }
}
```

改下排版

```java
public class Solution {
    public String tree2str(TreeNode t) {
    	if (!t)		return "";
        if (!t.left && !t.right)	return t.val + "";
        if (!t.right)	return t.val + "(" + tree2str(t.left) + ")";
        
        return t.val+"("+tree2str(t.left)+")("+tree2str(t.right)+")";  
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
class Solution {
public:
    string tree2str(TreeNode* t) 
    {
    	if (!t)	return "";
        if (!t->left && !t->right)	return to_string(t->val);
        if (!t->right)	return to_string(t->val) + "(" + tree2str(t->left) + ")";
        return	to_string(t->val) + "(" + tree2str(t->left) + ")(" + tree2str(t->right) + ")";
    }
};
```



### 非递归手动压栈

- 因为要保存括号，所以需要一个集合存储所有遍历过的节点
- 它没有子节点，什么都不做
- 有两个节点，右孩子先入栈，再左孩子入栈，保证先序遍历顺序
- 如果只有左孩子，则左孩子入栈
- 如果只有右孩子，则末尾添加一个`()`后，再将右孩子入栈

##### java实现

```java
public class Solution {
    public String tree2str(TreeNode t) {
        if (t == null)
            return "";
        Stack < TreeNode > stack = new Stack < > ();
        stack.push(t);
        Set < TreeNode > visited = new HashSet < > ();
        StringBuilder s = new StringBuilder();
        while (!stack.isEmpty()) {
            t = stack.peek();
            if (visited.contains(t)) {
                stack.pop();
                s.append(")");
            } else {
                visited.add(t);
                s.append("(" + t.val);
                if (t.left == null && t.right != null)
                    s.append("()");
                if (t.right != null)
                    stack.push(t.right);
                if (t.left != null)
                    stack.push(t.left);
            }
        }
        return s.substring(1, s.length() - 1);
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
class Solution {
public:
    string tree2str(TreeNode* t) 
    {
		if (!t)	return "";
        
        string str = "";
        set<TreeNode*> visited;
        stack<TreeNode*> s;
        s.push(t);
       	
        
        while (!s.empty())
        {
            t = s.top();
            
            if (visited.find(t) != visited.end())
            {
            	s.pop();
                str += ")";
			}
            else
            {
                visited.insert(t);
                str += "(" + to_string(t->val);
                
                if (!t->left && t->right)	str += "()";
                if (t->right)	s.push(t->right);
                if (t->left)	s.push(t->left);
            }
        }
        return str.substr(1, str.size() - 2);
    }
};
```

$$
\begin{align*}
& e.g. \\
& INPUT:  & [1,2,3,4] \\
& OUTPUT: & (1\\
& & (1(2\\
& &(1(2(4\\
& &(1(2(4)\\
& &(1(2(4)) \\
& &(1(2(4))(3\\
& &(1(2(4))(3)\\
& &(1(2(4))(3))\\
\end{align*}
$$



### [MrHuang](https://leetcode-cn.com/problems/construct-string-from-binary-tree/solution/cgao-xiao-di-cun-fang-fa-by-mrhuang-3/)

- 用`stringstream`代替`string`
- 用`lambda`表达式代替`函数`

```C++
class Solution {
public:
    string tree2str(TreeNode* t) {
        if(t==nullptr)
            return "";
        stringstream ss;
        function<void(TreeNode*)> helper = [&ss, &helper](TreeNode* t){
            ss<<t->val;
            if(t->left==nullptr){
                if(t->right!=nullptr){
                    ss<<"()(";
                    helper(t->right);
                    ss<<')';
                }
            }
            else if(t->right==nullptr){
                ss<<'(';
                helper(t->left);
                ss<<')';
            }
            else{
                ss<<'(';
                helper(t->left);
                ss<<")(";
                helper(t->right);
                ss<<')';
            }
        };
        helper(t);
        string s;
        ss>>s;
        return s;
    }
};
```



## References

https://leetcode-cn.com/problems/construct-string-from-binary-tree/solution/gen-ju-er-cha-shu-chuang-jian-zi-fu-chuan-by-leetc/

https://leetcode-cn.com/problems/construct-string-from-binary-tree/solution/cgao-xiao-di-cun-fang-fa-by-mrhuang-3/

https://stackoverflow.com/questions/1701067/how-to-check-that-an-element-is-in-a-stdset

