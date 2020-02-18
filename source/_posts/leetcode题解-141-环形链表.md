---
title: leetcode题解-141-环形链表
mathjax: true
date: 2020-02-19 01:15:33
categories:
tags:
- 链表
- 双指针
---

## [题目](https://leetcode-cn.com/problems/linked-list-cycle/)

以下2种题解，时间均是$O(1)$

### 哈希表

额外空间: $O(n)$

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
    bool hasCycle(ListNode *head) 
    {
        auto p = head;
        set<ListNode*> hash;

        while (p)
        {
            if (!hash.count(p))
            {
                hash.insert(p);
            }
            else
            {
                return true;
            }
            p = p->next;
        }   
        
        return false;
    }
};
```



### 双指针法

额外空间：$O(1)$

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
    bool hasCycle(ListNode *head) 
    {
        if (!head || !head->next)   return false;
        ListNode* slowP = head;
        ListNode* fastP = head->next;

        while (slowP != fastP)
        {
            // CUZ fastP 快些，所以fastP/ fastP->next为null之后，就说明 无环了
            if (!fastP || !fastP->next) return false;
            
            slowP = slowP->next;
            fastP = fastP->next->next;        
        }

        return true;
    }
};
```



## 总结

- **双指针法**

  - 需要注意，初始化 `fastP`, `slowP`快慢指针的时候的判别条件。

    ```C++
    if (!head || !head->next)   return false;
    ListNode* slowP = head;
    ListNode* fastP = head->next;
    ```

  - **判断是否有环**  <=> **判断是否指针指向`nullptr`处**. 判断指针是否指向`nullptr`处的常数项系数更小。

    注意此时的循环/条件语句的判别条件。

    ```C++
            while (slowP != fastP)
            {
                // CUZ fastP 快些，所以fastP/ fastP->next为null之后，就说明 无环了
                if (!fastP || !fastP->next) return false;
            }
    ```

    