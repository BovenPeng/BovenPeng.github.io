---
title: leetcode题解-303-区域和检索 - 数组不可变
date: 2019-11-03 20:23:56
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 数组
- 2维dp
- hash
- 插入1个虚拟0 
- 缓存
- ×
---

## [题目](https://leetcode-cn.com/problems/range-sum-query-immutable/)

```C++
class NumArray {
public:
    NumArray(vector<int>& nums) {
        
    }
    
    int sumRange(int i, int j) {
        
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```



## My way

### 1. 暴力法

```C++
class NumArray {
private:
    vector<int> dp;
    

public:
    NumArray(vector<int>& nums) {
        dp = nums;
    }
    
    int sumRange(int i, int j) {
        int sum = 0;
        for (int k = i; k <= j; ++k)
        {
            sum += dp[k];
        }

        return sum;
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```

结果，时间复杂度极低，仅超过10%的解。

很明显是要先用dp数组存入各个索引$i$, $j$对应的值。



### 2. 2维DP

#### 状态转移方程：

$$
dp[i][j] = dp[i][j-1] + nums[j-1]
$$



```c++
class NumArray {
private:
    vector<vector<int>> dp;
    

public:
    NumArray(vector<int>& nums):dp(nums.size()+1, vector<int>(nums.size()+1))
    {
        for (int i = 1; i <= nums.size(); ++i)
        {
            for (int j = i; j <= nums.size(); ++j)
            {
                dp[i][j] = dp[i][j-1] + nums[j-1];
            }
        }
    }
    
    int sumRange(int i, int j) 
    {
        return dp[i+1][j+1];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```

结果更慢了，仅超过5%的解。。。

应该是哪里思路出了问题...

```C++
class NumArray {
private:
    int **dp;
    int m;

public:
    NumArray(vector<int>& nums)
    {
        m = nums.size() + 1;
        dp = new int*[m];
        for(int i = 0; i < m; ++i)
        {
            dp[i] = new int[m];
        }

        for (int i = 1; i <= nums.size(); ++i)
        {
            int sum = 0;
            for (int j = i; j <= nums.size(); ++j)
            {
                sum += nums[j-1];
                dp[i][j] = sum;
            }
        }
    }
    
    ~NumArray()
    {
        for (int i = 0; i < m; ++i)
        {
            delete []dp[i];
        }

        delete []dp;
    }
    
    int sumRange(int i, int j) 
    {
        return dp[i+1][j+1];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```

换成原生数组，时间复杂度仅超过7%，仍然很糟糕。。。

假如直接静态数组`    int dp[INT_MAX][INT_MAX];`，内存会炸掉。。。



## 官方way

### 2. 缓存

将计算结果提前存入到 **哈希表** 中。

PS：而我用的是`vector<vector<int>> dp, int **dp`, 感觉没区别，应该是`resize()/动态分配内存`那耗费了不少时间。

```java
private Map<Pair<Integer, Integer>, Integer> map = new HashMap<>();

public NumArray(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        int sum = 0;
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
            map.put(Pair.create(i, j), sum);
        }
    }
}

public int sumRange(int i, int j) {
    return map.get(Pair.create(i, j));
}
```



### 3. 缓存

优化空间复杂度从$O(n^2) = > O(n)$.
$$
sum[k] = 
\begin{cases}
& \sum_{i=0}^{k-1} nums[i] & , k > 0 \\
& 0                        & , k == 0\\
\end{cases}
$$
优化为:
$$
sumrange(i,\ j)= sum[j+1] - sum[i]
$$
java:

```java
private int[] sum;

public NumArray(int[] nums) {
    sum = new int[nums.length + 1];
    for (int i = 0; i < nums.length; i++) {
        sum[i + 1] = sum[i] + nums[i];
    }
}

public int sumRange(int i, int j) {
    return sum[j + 1] - sum[i];
}
```

C++: use array

```C++
class NumArray {
private:
    int *sum;

public:
    NumArray(vector<int>& nums) {
        sum = new int [nums.size() + 1];

        for (int i = 0; i < nums.size(); ++i)
        {
            sum[i+1] = sum[i] + nums[i];
        }
    }
    ~NumArray()
    {
        delete []sum;
    }
    
    int sumRange(int i, int j) {
        return sum[j + 1] - sum[i];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```

但这也只超过了 78.45%...

C++: user `vector<int>`

```c++
class NumArray {
    vector<int> dp;
public:
    NumArray(vector<int>& nums) {
        int sum=0;
        dp.push_back(0);
        for(int i=0;i<nums.size();++i)
        {
            sum+=nums[i];
            dp.push_back(sum);
        }
    }
    
    int sumRange(int i, int j) {
        return dp[j+1]-dp[i];
    }
};

```

超过了96.23%，代码类似 [陈乐乐解2](https://leetcode-cn.com/problems/range-sum-query-immutable/solution/qu-yu-he-jian-suo-shu-zu-bu-ke-bian-by-gpe3dbjds1/).

但仔细看她代码，你会发现她在C++编码时，有个很不好的习惯，我在这纠正了，就不指出来了。



## 总结

- 傻了，老想着把计算结果存起来，到时候直接取，居然忘记了
  - $sumrange(i,\ j)= sum[j+1] - sum[i]$
- 这里也用到了插入了一个虚拟 0 作为 sum 数组中的第一个元素。
  - 这个技巧可以避免在 sumrange 函数中进行额外的条件检查，对于base case, corner case等等
  -  PS：尽管，我其实喜欢加上对于base case, corner case的检查，但却是实现起来有些冗余





## Reference

[官方解](https://leetcode-cn.com/problems/range-sum-query-immutable/solution/qu-yu-he-jian-suo-shu-zu-bu-ke-bian-by-leetcode/)

[陈乐乐解](https://leetcode-cn.com/problems/range-sum-query-immutable/solution/qu-yu-he-jian-suo-shu-zu-bu-ke-bian-by-leetcode/)

### extended

[How do I declare a 2d array in C++ using new?](https://stackoverflow.com/questions/936687/how-do-i-declare-a-2d-array-in-c-using-new)