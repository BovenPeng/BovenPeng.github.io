---
title: leetcode题解-647-回文子串
date: 2019-11-01 16:35:48
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- 动态规划
- 字符串
- 数组对角遍历
- 中心扩散法
- ×
---



## [题目](https://leetcode-cn.com/problems/palindromic-substrings/)



## ~~My way~~

#### 状态定义：

$$
\begin{align*}
& s:\ 字符串s\\
& i:\ 字符串s第i位\\
& j:\ 字符串s第j位\\
& s[i:j]:\ 字符串s第i位到第j位的子串, s[i,j]\\
& dp[i][j]:\ dp数组在s[i,j]上是否为回文串的记录。
\end{align*}
$$

#### Base case：

$$
\begin{align*}
&s[i][j] = 1, &\text{if $i==j$}\ 
\end{align*}
$$



#### 状态转移方程：

$$
s[i-1][j+1] = 
\begin{cases}
& 1,\ \text{if $s[i] == s[j]$}\\
& 0,\ otherwise
\end{cases}
$$



### Error:

在如何实现数组遍历上面，出现了问题。

套用了 **glamour**的逆向对角遍历思路：

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int m = s.size();
        int cnt = 0;
        vector<vector<bool>> dp(m, vector<bool>(m, false));


        // base case
        for (int i = 0; i < m; ++i)
        {
            dp[i][i] = true;
            ++cnt;
        }


        for (int i = 1; i < m; ++i)
        {
            for (int j = 0; j <= m - i - 1; ++j)
            {
                int row = j;
                int column = i + j;
                bool cur = s[row] == s[column];
                if (cur && (i == 1 || dp[row+1][column-1]))
                {
                    dp[row][column] = true;
                    ++cnt;
                }
            }
        }

        return cnt;
    }
};
```



## Glamour way

### 1.DP + 逆向对角遍历

#### 逆向对角遍历思路：

{% asset_img dp1.png pic1 %}

上图中同一条直线上的值有前者依赖于后者的关系，因此我们应该以对角线方向遍历

{% asset_img dp2.png pic2 %}

> 但是本方法在發現一字符串不是回文串后，仍然對包含其的字符串進行回文判斷。
>
> 此方法包含了**冗餘的判斷，因此還有優化的地方。**

```java
 public int countSubstrings(String s) {
        int len = s.length();
        if(len <= 1) return len;
        boolean[][] dp = new boolean[len][len];
        int i;
        int j;
        int row, column;
        boolean current;
        int count = 0;
        for(i = 0; i < len; i++){
            dp[i][i] = true;
            count++;
        }
        for(i = 1; i < len; i++){
            for(j = 0; j <= len - i - 1; j++){
                row = j;
                column = i + j;
                current = s.charAt(row) == s.charAt(column);
                if(current && (i == 1 || dp[row + 1][column - 1])){
                    dp[row][column] = true;
                    count++;
                }
            }
           
        }
        return count;
    }

```



### 2. 分治法 + 中心扩散法

> **分治法**，对以不同字符为中心的回文分而治之，同时又将回文的长度分为奇数和偶数，奇数的中心有一个，而偶数的中心有两个.
>
> 有一個規律：
>
> - 回文串左右兩邊同時去掉一個字符仍然是回文串；反之一個字符串不是回文串，
> - 那麽他左右兩邊不論加上什麽字符都不可能是回文串.

```java
 public int countSubstrings(String s) {
        int count = 0;
        int i;
        for(i = 0; i < s.length(); i++){
            count += countPalindrome(s, i, i);
            count += countPalindrome(s, i, i + 1);
        }
         return count;
    }
    public int countPalindrome (String s, int left, int right){
        int count = 0;
        while(left >= 0 && right < s.length() && s.charAt(left--) == s.charAt(right++)){
            count++;
        }
        return count;
    }
```

C++实现

```C++
class Solution {
public:
    int countSubstrings(string s) {
        int m = s.size();
        int cnt = 0;

        for (int i = 0; i < m; ++i)
        {
            cnt += countPalindrome(s, i, i);    // even length of string
            cnt += countPalindrome(s, i, i+1);  // old length of string
        }

        return cnt;
    }

    int countPalindrome(const string& s, int l, int r)
    {
        int cnt = 0;
        while (l >= 0 && r < s.size() && s[l--] == s[r++])
        {
            ++cnt;
        }

        return cnt;
    }
};
```



## Reference

[glamour解法](https://leetcode-cn.com/problems/palindromic-substrings/solution/dong-tai-gui-hua-yun-xing-shi-jian-11mszhong-xin-k/)