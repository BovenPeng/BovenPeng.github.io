---
title: 'leetcode题解: 877.石子游戏'
date: 2019-10-27 21:52:40
mathjax: true
categories:
- 刷题
tags: 
- leetcode
- DP
- 二维dp
- 博弈问题
- ×
---

## [题目](https://leetcode-cn.com/problems/stone-game/)

## 官方解

### 数学way

直接从官方解处摘抄过来，很有意思的数学归纳证明思想。如下：

显然，亚历克斯总是赢得 2 堆时的游戏。 通过一些努力，我们可以获知她总是赢得 4 堆时的游戏。

如果亚历克斯最初获得第一堆，她总是可以拿第三堆。 如果她最初取到第四堆，她总是可以取第二堆。第一 + 第三，第二 + 第四 中的至少一组是更大的，所以她总能获胜。

我们可以将这个想法扩展到 N 堆的情况下。设第一、第三、第五、第七桩是白色的，第二、第四、第六、第八桩是黑色的。 亚历克斯总是可以拿到所有白色桩或所有黑色桩，其中一种颜色具有的石头数量必定大于另一种颜色的。

因此，亚历克斯总能赢得比赛。

```C++
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        return true;
    }
};
```




## 综评最高解

在dp解释上强于官方解，简要摘取部分过程，详细见reference处。

对于$piles=[3, 9, 1, 2]$,

{% asset_img dp_table_1.png dp_table %}



#### 定义**状态**：

$$
dp[i][j].fir\ or\ dp[i][j].sec\\
其中：\\
0 <= i < piles.length, i相当于石子堆最左堆\\
i <= j < piles.length, j相当于石子堆最右堆
$$
其中，
$$
dp[i][j].fir表示，对于piles[i...j]这部分石子，先手能获得的最高分数 \\
dp[i][j].sec表示，对于piles[i...j]这部分石子，后手能获得的最高分数
$$

#### **状态转移方程**为：

$$
dp[i][j].fir = max(piles[i] + dp[i+1][j].sec, piles[j] + dp[i][j-1].sec) \\
$$

$$
\begin{align*}
& if 先手选择左边:
    dp[i][j].sec = dp[i+1][j].fir \\
& if 先手选择右边:
    dp[i][j].sec = dp[i][j-1].fir \\

& 解释：\\

& 我作为后手，要等先手先选择，有两种情况： \\
& 如果先手选择了最左边那堆，给我剩下了 piles[i+1...j] \\
& 此时轮到我，我变成了先手；\\
& 如果先手选择了最右边那堆，给我剩下了 piles[i...j-1] \\ 
& 此时轮到我，我变成了先手。\\
\end{align*}
$$



#### base case

$$
\begin{align*}
& dp[i][j].fir = piles[i] \\
& dp[i][j].sec = 0 \\
& 其中 0 <= i == j < n \\
& 解释：i 和 j 相等就是说面前只有一堆石头 piles[i] \\
& 那么显然先手的得分为 piles[i] \\
& 后手没有石头拿了，得分为 0 \\
\end{align*}
$$

{% asset_img dp_table_2.png dp_table %}

这里需要注意一点，我们发现 $base\ case$是斜着的，而且我们推算 `dp[i][j]` 时需要用到 `dp[i+1][j]` 和 `dp[i][j-1]`：

{% asset_img dp_table_3.png dp_table %}

所以说算法不能简单的一行一行遍历 `dp` 数组，**而要斜着遍历数组**：

{% asset_img dp_table_4.png dp_table %} 







#### 实现

```C++
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();

        // initialize dp array
        vector<vector<pair<int, int>>> dp(n, vector<pair<int, int>>(n, make_pair(0, 0)));

        // fill with base case
        for (int i = 0; i < n; ++i)
        {
            dp[i][i].first = piles[i];
            dp[i][i].second = 0;
        }

        // traverse array in a diagonal way
        for (int l = 2; l <= n; ++l)
        {
            for (int i = 0; i <= n - l; ++i)
            {
                int j = i + l - 1;

                // 先手选择最左边或者最右边的分数
                int left = piles[i] + dp[i+1][j].second;
                int right = piles[j] + dp[i][j-1].second;

                // 套用状态转移方程
                if (left > right)
                {
                    dp[i][j].first = left;
                    dp[i][j].second = dp[i+1][j].first;
                }
                else
                {
                    dp[i][j].first = right;
                    dp[i][j].second = dp[i][j-1].first;
                }
            }
        }
        pair<int, int> res(dp[0][n-1]);

        return res.first - res.second;
    }
};
```

综评Java实现

```java
/* 返回游戏最后先手和后手的得分之差 */
int stoneGame(int[] piles) {
    int n = piles.length;
    // 初始化 dp 数组
    Pair[][] dp = new Pair[n][n];
    for (int i = 0; i < n; i++) 
        for (int j = i; j < n; j++)
            dp[i][j] = new Pair(0, 0);
    // 填入 base case
    for (int i = 0; i < n; i++) {
        dp[i][i].fir = piles[i];
        dp[i][i].sec = 0;
    }
    // 斜着遍历数组
    for (int l = 2; l <= n; l++) {
        for (int i = 0; i <= n - l; i++) {
            int j = l + i - 1;
            // 先手选择最左边或最右边的分数
            int left = piles[i] + dp[i+1][j].sec;
            int right = piles[j] + dp[i][j-1].sec;
            // 套用状态转移方程
            if (left > right) {
                dp[i][j].fir = left;
                dp[i][j].sec = dp[i+1][j].fir;
            } else {
                dp[i][j].fir = right;
                dp[i][j].sec = dp[i][j-1].fir;
            }
        }
    }
    Pair res = dp[0][n-1];
    return res.fir - res.sec;
}
```



## 总结

- 二维DP，也是递推问题，但是给人的没有一维DP那么好处理。

  - **状态定义**，一般涉及到2个状态$i,\ j$。可以用表格法表示，有点类似0-1背包问题。
  - **状态转移方程**，一般涉及到上一状态的2个状态。
  - **base case**, 我的理解是一般类似于 **corner case**的存在，是一个需要考虑到的初始条件。

- **斜着遍历数组**，这个在实现的时候，也不是一件非常容易的事情。

  - ```C++
    // traverse array in a diagonal way
    for (int l = 2; l <= n; ++l)
    {
    	for (int i = 0; i <= n - l; ++i)
        {
        	int j = i + l - 1;
            // ...
        }
    }
    ```

    若$n = 4$, 则依次遍历：
    $$
    \begin{align*}
    & [0,\ 1],\ [1,\ 2], [2,\ 3], [3,\ 4] \\ 
    & [0,\ 2],\ [1,\ 3], [2,\ 4] \\
    & [0,\ 3],\ [1,\ 4] \\
    & [0,\ 4] \\
    \end{align*}
    $$
    

## Reference

[labuladong: leetcode解](https://leetcode-cn.com/problems/stone-game/solution/jie-jue-bo-yi-wen-ti-de-dong-tai-gui-hua-tong-yong/)

[leetcode 官方解](https://leetcode-cn.com/problems/stone-game/solution/shi-zi-you-xi-by-leetcode/)

### extended 

[std::pair](https://en.cppreference.com/w/cpp/utility/pair)

[std::vector](https://en.cppreference.com/w/cpp/container/vector)