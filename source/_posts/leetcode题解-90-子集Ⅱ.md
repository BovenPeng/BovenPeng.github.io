---
title: leetcode题解-90-子集Ⅱ
mathjax: true
date: 2020-02-19 01:40:52
categories:
tags:
-数组
-回溯法
---

## [题目](https://leetcode-cn.com/problems/subsets-ii/)

```C++
class Solution {
private:
    vector<vector<int>> sub;
    void findSub(vector<int>& nums, vector<int>& tmp, int pos)
    {
        if (pos >= nums.size())
        {
            sub.emplace_back(tmp);
            return ;
        }
        // add this element
        tmp.emplace_back(nums[pos]);
        findSub(nums, tmp, pos+1);

        // skip the duplicated 
        while (pos + 1 < nums.size() && nums[pos] == nums[pos+1])
        {
            ++pos;
        }
        // not add this element
        tmp.pop_back();
        findSub(nums, tmp, pos+1);
    }

public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        vector<int> tmp{};

        findSub(nums, tmp, 0);    
    
        return sub;
    }
};
```

