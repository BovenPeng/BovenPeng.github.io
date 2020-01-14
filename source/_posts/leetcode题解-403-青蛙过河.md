---
title: leetcode题解-403-青蛙过河
date: 2019-11-14 21:15:10
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- DFS
- 数组
- 特殊DP数组
- ×
---



## [题目](https://leetcode-cn.com/problems/frog-jump/)



## My way

### 1.暴力解



```C++
class Solution {
public:
    bool canCross(vector<int>& stones) {
        return helper(stones, 0, 0);
    }

    bool helper(const vector<int>& stones, int curPos, int jumpSize)
    {
        for (int nextPos = curPos+1; nextPos < stones.size(); ++nextPos)
        {
        	// gap值表示curPos位置 跳到 i位置 的跳跃距离
            int gap = stones[nextPos] - stones[curPos];
            if (gap >= jumpSize - 1 && gap <= jumpSize + 1)
            {
            	// 能跳到i位置，则递归下去
                if (helper(stones, nextPos, gap))
                {
                    return true;
                }
            }
        }

        return curPos == stones.size() - 1;
    }
};
```



### 2. 记忆化搜索

利用暴力解的递归关系，使用一个数组提前存储好对应的值，然后查询的时候遇到已有的结果则提前跳出。

```c++
class Solution {

    bool helper(const vector<int>& stones, int curPos, int jumpSize, vector<vector<int>>& mem)
    {
        // 已有的计算结果则提前跳出
        if (mem[curPos][jumpSize] >= 0)
        {
            return mem[curPos][jumpSize];
        }

        for (int nextPos = curPos+1; i < stones.size(); ++i)
        {
            // gap值表示curPos位置 跳到 i位置 的跳跃距离
            int gap = stones[nextPos] - stones[curPos];
            if (gap >= jumpSize - 1 && gap <= jumpSize + 1)
            {
                // 能从curPos位置 跳到 i位置上，则记录为1
                if (helper(stones, nextPos, gap, mem) == 1)
                {
                    mem[nextPos][gap] = 1;
                    return 1;
                }
            }
        }
		
        // 假如能跳到最后一个位置上，则记录下，反之同理
        mem[curPos][jumpSize] = (curPos == stones.size()-1) ? 1 : 0;
        return mem[curPos][jumpSize];
    }
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        vector<vector<int>> mem(n, vector<int>(n, -1));

        return (helper(stones, 0, 0, mem) == 1);
    }
};
```



### 3.~~DP~~

在思考怎么通过已有的递归中的依赖关系 <=> dp的状态转移方程上去。

发现假如是使用2维DP表的话，是不可成立的，因为一个i值需要对应多个j值，所以无从下手。





## 官方解

### 1.记忆化搜索 + 二分查找

在检索位置`curPos` + `jumpSize`时候，使用二分搜索来查找位置上是否有石头

```java
public class Solution {
    public boolean canCross(int[] stones) {
        int[][] memo = new int[stones.length][stones.length];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return can_Cross(stones, 0, 0, memo) == 1;
    }
    public int can_Cross(int[] stones, int ind, int jumpsize, int[][] memo) {
        if (memo[ind][jumpsize] >= 0) {
            return memo[ind][jumpsize];
        }
        int ind1 = Arrays.binarySearch(stones, ind + 1, stones.length, stones[ind] + jumpsize);
        if (ind1 >= 0 && can_Cross(stones, ind1, jumpsize, memo) == 1) {
            memo[ind][jumpsize] = 1;
            return 1;
        }
        int ind2 = Arrays.binarySearch(stones, ind + 1, stones.length, stones[ind] + jumpsize - 1);
        if (ind2 >= 0 && can_Cross(stones, ind2, jumpsize - 1, memo) == 1) {
            memo[ind][jumpsize - 1] = 1;
            return 1;
        }
        int ind3 = Arrays.binarySearch(stones, ind + 1, stones.length, stones[ind] + jumpsize + 1);
        if (ind3 >= 0 && can_Cross(stones, ind3, jumpsize + 1, memo) == 1) {
            memo[ind][jumpsize + 1] = 1;
            return 1;
        }
        memo[ind][jumpsize] = ((ind == stones.length - 1) ? 1 : 0);
        return memo[ind][jumpsize];
    }
}
```



### 2.DP

在动态规划方法中，我们会利用散列表 mapmap，对于散列表中的 key:valuekey:value，keykey 表示当前石头的位置，valuevalue 是一个包含 jumpsizejumpsize 的集合，其中每个 jumpsizejumpsize 代表可以通过大小为 jumpysizejumpysize 的一跳到达当前位置。

首先我们对散列表初始化，keykey 为所有石头的位置，除了位置 0 对应的 valuevalue 为包含一个值 0 的集合以外，其余都初始化为空集。接下来，依次遍历每个位置上的石头。对于每个 currentPositioncurrentPosition，遍历 valuevalue 中每个 jumpsizejumpsize，判断位置 currentPosition + newjumpsizecurrentPosition+newjumpsize 是否存在于 mapmap 中，对于每个 jumpsizejumpsize，newjumpsizenewjumpsize 分别为 jumpsize-1jumpsize−1，jumpsizejumpsize，jumpsize+1jumpsize+1。如果找到了，就在对应的 valuevalue 集合里新增 newjumpsizenewjumpsize。重复这个过程直到结束。如果在结束的时候，最后一个位置对应的集合非空，那也就意味着我们可以到达终点，如果还是空集那就意味着不能到达终点。

```java
public class Solution {
    public boolean canCross(int[] stones) {
        HashMap<Integer, Set<Integer>> map = new HashMap<>();
        for (int i = 0; i < stones.length; i++) {
            map.put(stones[i], new HashSet<Integer>());
        }
        map.get(0).add(0);
        for (int i = 0; i < stones.length; i++) {
            for (int k : map.get(stones[i])) {
                for (int step = k - 1; step <= k + 1; step++) {
                    if (step > 0 && map.containsKey(stones[i] + step)) {
                        map.get(stones[i] + step).add(step);
                    }
                }
            }
        }
        return map.get(stones[stones.length - 1]).size() > 0;
    }
}
```



## 总结

- 暴力递归解总是有的，不过要找出一个靠谱的递归实现，然后可以通过其优化为 用一个数组存好结果的记忆化搜索的方式；
- dp数组可以是 **hash散列** 这种存储方式，不一定是 数组 的存储方式。



## Reference

[官方解](https://leetcode-cn.com/problems/frog-jump/solution/qing-wa-guo-he-by-leetcode/)