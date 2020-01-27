---
title: leetcode题解-237-删除链表中的节点
mathjax: true
date: 2020-01-27 21:45:37
categories:
tags:
---

## [题目](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

### [Leetcode官方 way](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/solution/shan-chu-lian-biao-zhong-de-jie-dian-by-leetcode/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) 
    {
        node->val = node->next->val;
        node->next = node->next->next;    
    }
};
```



## Reference

https://leetcode-cn.com/problems/delete-node-in-a-linked-list/solution/shan-chu-lian-biao-zhong-de-jie-dian-by-leetcode/

## 总结

- 提供了一个新的删除节点的思路：
  - **在不考虑末尾节点的时候！！**
  - 跟下一个节点交换值之后，然后调整`next`指针为`next->next`