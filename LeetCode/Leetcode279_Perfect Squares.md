---
title: 完美平方数
date: 2018-01-26
mathjax: false
tags: 动态规划
---

问题叙述:给定一个正整数n，求最小数量的平方数，加起来等于n。

Examples:
1. 输入 n = 12 输出 3 解释 12 = 4 + 4 + 4
2. 输入 n = 13 输出 2 解释 13 = 4 +9


## Naive思路1

从1到n更新每一个数i符合题意的解，用一个数组记录。

- step1: 对n以内的平方数先置1
- step2: 对当前要处理的数i(1~i-1的解已知)
    - 从j = 1~i/2 遍历加法组合，则dp[i] = dp[j] + dp[i - j]
    - 取所有满足条件的j中，dp[i]的最小值
- 返回dp[i]

时间复杂度为O(n^3)，代码为，

```
// Time Limit Exceeded 
// 554 / 586
class Solution {
public:
    int numSquares(int n) 
    {
        vector<int> dp(n + 1);
        for(int i = 1; i <= n; i++) dp[i] = i;
        for(int i = 2; i <=n/2; i++) if(i*i <= n) dp[i*i] = 1;
        for(int i = 5; i <= n; i++)
        {
            if(dp[i] != 1)
                for(int j = 1; j <= i/2; j++)
                    dp[i] = min(dp[i],dp[j] + dp[i - j]);
        }
        return dp[n];
    }
};
```

## Naive思路2 递归解/DFS

对n以内的平方数，遍历所有相加等于n的组合，返回其中数字个数最少的情况。问题转化为了[Leetcode039_Combination Sum](https://github.com/shouguicai/Notes/blob/master/LeetCode/Leetcode039_Combination%20Sum.md)问题。

- step1: 求出n以内的所有平方数，放在数组squares[]中
- step2: 递归求解squares[]中相加等于n的组合，同一个数可以多次使用。

不同的是，这里不要求所有符合情况的组合，只需要知道最小的组合，
所以递归可以提前停止，即
```
if(cur == -1 || target < 0 || numofSquares >= ans) return;
```

代码如下，

```
// 1041 ms
class Solution {
public:
    int numSquares(int n) 
    {
        vector<int> squares;
        for(int i = 1; i <= n/2; i++)
            if( i*i <= n)
                squares.push_back(i*i);
        int numofSquares = 0;
        ans = n;
        combinationSum(squares,n,numofSquares,squares.size()-1);
        return ans;
    }
private:
    int ans;
    void combinationSum(vector<int> candidates, int target, int& numofSquares, int cur)
    {
        if (target == 0)
        {
            ans = min(ans,numofSquares);
            return;
        }

        if(cur == -1 || target < 0 || numofSquares >= ans)
            return;

        numofSquares ++;
        // 因为允许多次使用同一个元素，cur没有+1
        combinationSum(candidates, target - candidates[cur], numofSquares, cur);
        numofSquares --;
        combinationSum(candidates, target, numofSquares, cur - 1);
    }
};
```

最坏时间复杂度O(n^m)，其中，n为目标数字，m为n以内平方数的个数。

## 动态规划 

动态规划问题的关键是找到最优子结构，
用dp[i]表示相加等于i所需的最小平方数的个数，给在平方数数组squares[]下，最优子结构如下，

```
dp[i] = min(dp[i-squares[j]]) + 1 , for 所有 0< j < n-1的，并且i-squares[j] >= 0
```

Bottom up manner代码为，

```
// 112 ms
class Solution {
public:
    int numSquares(int n) 
    {
        vector<int> squares;
        for(int i = 1; i <= n/2 + 1; i++)
            if( i*i <= n)
                squares.push_back(i*i);
        vector<int> dp(n+1);
        int m = squares.size();
        dp[0] = 0;
        for(int i = 1; i <= n ; i++)
        {
            int numofSquares = i;
            for(int j = 0 ; j < m; j++)
                if(i - squares[j] >= 0)
                    numofSquares = min(numofSquares,1 + dp[i - squares[j]]);
            if(numofSquares <= i)
                dp[i] = numofSquares;
        }
        return dp[n];
    }
};
```
算法时间复杂度O(n*m)，空间复杂度O(n)，其中，n为目标数字，m为n以内平方数的个数。





