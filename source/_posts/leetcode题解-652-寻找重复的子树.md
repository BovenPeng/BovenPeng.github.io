---
title: leetcode题解-652-寻找重复的子树
mathjax: true
date: 2019-12-10 16:43:47
categories:
- 刷题
tags: 
- leetcode
- 递归
- 树
- 树的遍历
- 序列化
- hash
- innovative solution
- ×
---



## [题目](https://leetcode-cn.com/problems/find-duplicate-subtrees/)

### My way

#### 序列化 + 重复子序列DP解法 + 反序列化回去时找到对应子树root结点

感觉代价过高，而且在反序列化回去找到对应结点时会很麻烦。



### [yushinan](https://leetcode-cn.com/problems/find-duplicate-subtrees/solution/simple-solution-by-yushinan-2/)

#### 序列化+hash取出重复子树的头结点

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
  vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
    unordered_map<string, int> counts;
    vector<TreeNode*> ans;
    serialize(root, counts, ans);
    return ans;
  }
private:
  string serialize(TreeNode* root, 
    unordered_map<string, int>& counts,
    vector<TreeNode*>& ans) {
    if (!root) return "#";
    string key = to_string(root->val) + "," 
        + serialize(root->left, counts, ans) + "," 
        + serialize(root->right, counts, ans);
    if (++counts[key] == 2)
      ans.push_back(root);
    return key;
  }
};
```



### [*yuguorui](https://leetcode-cn.com/problems/find-duplicate-subtrees/solution/c-xu-lie-hua-jie-fa-by-yuguorui/)

```C++
struct SubTreeCount
{
	TreeNode* root;
	int count;

};

class Solution {
public:
	unordered_map<size_t, SubTreeCount> dict;
	hash<string> hasher;
	vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
		serialize(root);
		vector<TreeNode*> res;
		for(auto const& item: dict) {
			if (item.second.count > 1) {
				res.push_back(item.second.root);
			}
		}
		return res;
	}

	size_t serialize(TreeNode* root) {
		if (root == NULL) return hasher("[]");
		char buf[2048];
		snprintf(buf, 2048, "[%d,%d,%d]", root->val, serialize(root->left), serialize(root->right));
		size_t res = hasher(buf);

		if (dict.count(res) == 0) {
			dict.insert(pair<size_t, SubTreeCount>(res, {root, 1}));
		} else {
			dict[res].count++;
		}
		return res;
	}
};
```

- 利用了`hash<string>`后结果，作为判断这一子序列是否出现过的情况/有重复子串的情况；
- 用`snprintf`的IO流来生成对应序列化结果。



## 总结

- 对整棵树中，每一颗子树都进行序列化，并利用`unordered_map`得到每个子树的序列化结果是否重复
- 对于序列化路径有：**node.path = node.val + node.left.path + node.right.path**
- 这里的序列化，可以不用序列化`null`

## Reference

https://leetcode-cn.com/problems/find-duplicate-subtrees/solution/simple-solution-by-yushinan-2/