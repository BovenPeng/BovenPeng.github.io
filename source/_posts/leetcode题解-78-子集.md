---
title: leetcode题解-78-子集
mathjax: true
date: 2020-01-28 09:49:14
categories:
tags:
- 回溯法
- 数组
---

## [题目](https://leetcode-cn.com/problems/subsets/)

### 回溯法

递归实现

额外空间：$O(n)$

时间：$O(2^n)$

```C++
class Solution {
private:
    vector<vector<int>> sub;
public:
    void findSub(vector<int>& nums, vector<int>& tmp, const int pos)
    {
        if (pos >= nums.size())
        {
            sub.emplace_back(tmp);
            return ;
        }

        // add this element
        tmp.emplace_back(nums[pos]);
        findSub(nums, tmp, pos + 1);

        // not add this element
        tmp.pop_back();
        findSub(nums, tmp, pos + 1);
    }
    
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        vector<int> tmp{};
        findSub(nums, tmp, 0);
        return sub;
    }
};
```



## 总结

- 使用`emplace_back()` 而不是 `push_back()`

- 我的理解：带有条件判断的DFS

  回溯法=>递归实现=>自动调栈<=> DFS



## References

[push_back vs emplace_back](https://stackoverflow.com/questions/4303513/push-back-vs-emplace-back)

[Examples where std::vector::emplace_back is slower than std::vector::push_back?](https://stackoverflow.com/questions/32941695/examples-where-stdvectoremplace-back-is-slower-than-stdvectorpush-back)

[push_back vs emplace_back to a std::vector](https://stackoverflow.com/questions/43596430/push-back-vs-emplace-back-to-a-stdvectorstdstring)

