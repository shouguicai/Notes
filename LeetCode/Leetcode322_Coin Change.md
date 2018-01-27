---
title: 找零钱
date: 2018-01-27
mathjax: false
tags: 动态规划
---

问题叙述:给定可供找零的硬币类型以及需要找零的数额，求最少需要的硬币数，无法成功找零则返回-1。

Examples:
1. 输入 coins=[1,2,5],amount=11 输出 3 解释 11 = 5 + 5 + 1
2. 输入 coins=[2],amount=3 输出 -1

## 递归解

思路类似于[Leetcode279_Perfect Squares](https://github.com/shouguicai/Notes/blob/master/LeetCode/Leetcode279_Perfect%20Squares.md)和[Leetcode039_Combination Sum](https://github.com/shouguicai/Notes/blob/master/LeetCode/Leetcode039_Combination%20Sum.md)，递归遍历所有可能的找零组合并返回其中硬币数量最小的情况。

同样的，这里不要求所有符合情况的组合，只需要知道最小的组合， 所以递归可以提前停止。

```
// Time Limit Exceeded
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        int numofcoins = 0;
        ans = amount + 1;
        sort(coins.begin(),coins.end());
        combinationSum(coins,amount,numofcoins,coins.size()-1);
        return ans == amount + 1 ? -1 : ans;
    }
private:
    int ans;
    void combinationSum(vector<int> candidates, int target, int& numofcoins, int cur)
    {
        if (target == 0)
        {
            ans = min(ans,numofcoins);
            return;
        }

        if(cur == -1 || target < 0 || numofcoins >= ans)
            return;

        numofcoins ++;
        // 因为允许多次使用同一个元素，cur没有+1
        combinationSum(candidates, target - candidates[cur], numofcoins, cur);
        numofcoins --;
        combinationSum(candidates, target, numofcoins, cur - 1);
    }
};
```

最坏时间复杂度O(S^n)，其中，S为目标找零，n为硬币种类数。

## 动态规划

动态规划问题的关键是找到最优子结构，
用dp[i]表示找零i元所需的最小硬币数目，给在coins下，最优子结构如下，

```
dp[i] = min(dp[i-C[j]]) + 1 , for 所有 0< j < n-1的，并且i-C[j] >= 0
```

Bottom up manner代码为，

```
//  26 ms
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        vector<int> dp(amount + 1);
        int n = coins.size();
        dp[0] = 0;
        for(int i = 1; i <= amount; i++)
        {
            int numofcoins = i + 1;
            for(int j = 0; j < n; j++)
                if(i - coins[j] >= 0 && dp[i - coins[j]] != -1)
                    numofcoins = min(numofcoins,dp[i - coins[j]] + 1);
            dp[i] = numofcoins == i + 1 ? -1 : numofcoins;
        }
        return dp[amount]; 
    }
};
```

算法时间复杂度O(S*n)，空间复杂度O(S)，其中，S为目标找零，n为硬币种类数。



