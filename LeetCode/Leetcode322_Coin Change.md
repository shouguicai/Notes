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

时间复杂度不详。

## 动态规划






