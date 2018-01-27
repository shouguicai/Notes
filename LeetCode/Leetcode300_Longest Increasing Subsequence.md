---
title: 最长递增子序列
date: 2018-01-27
mathjax: false
tags: 动态规划
---

问题叙述:要求找出给定序列中最长的递增子序列的长度，递增子序列中的元素满足递增性质，不要求元素相邻。

Examples:
1. 输入 [10, 9, 2, 5, 3, 7, 101, 18] 输出 4 解释 LIS是[2, 3, 7, 101]

## 递归解

记输入序列为nums，用L[i]表示以nums[i]为最后字符的LIS长度。对输入序列从左往右处理，
对每一个待处理的字符nums[k]，L[k]满足，

- L[k] = 1 + max(L[j]) , 对任意j，满足 nums[j] < nums[k]
- 否则，不存在满足条件的j，L[k] = 1

程序为，
```
// Time Limit Exceeded
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) 
    {
        int max = 1;
        int n = nums.size();
        _lis(nums,n-1,max);
        return n == 0 ? 0 :max;
    }
private:
    int _lis(vector<int> nums, int k, int& max)
    {
        // base case
        if(k == 0)
            return 1;
        /*
        递归解所有以nums[0],nums[1],...,nums[index-2]结尾的LIS长度。
        如果nums[j]<nums[k]，则更新L[k]。
        max_ending_here 是以nums[k]为末字符的LIS长度
        */
        int res, max_ending_here = 1;

        for(int j = 0; j < k; j++)
        {
            res = _lis(nums,j,max);
            if(nums[j] < nums[k] && res + 1 > max_ending_here)
                max_ending_here = res + 1;
        }
        if(max < max_ending_here)
            max = max_ending_here;
        //返回以nums[k]结尾的LIS长度
        return max_ending_here;
    }
};
```

时间复杂度O(n^3)。

## 动态规划

上面的递归程序存在重复调用的子问题，以nums长度为4为例子，递归调用树如下，

```
              lis(4)
        /        |     
      lis(3)    lis(2)   lis(1)
     /           /
   lis(2) lis(1) lis(1)
   /
lis(1)
```

Bottom up manner的程序为，

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) 
    {
        int n = nums.size();
        int max = 0;
        vector<int> dp(n,1);

        // bottom up
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < i; j++)
                if(nums[i] > nums[j] && dp[i] < dp[j] + 1)
                    dp[i] = dp[j] + 1;
            max = max(max,dp[i]);
        }
        return max;

    }
};
```

时间复杂度O(n^2)。


GeeksforGeeks[同题](https://github.com/shouguicai/Notes/blob/master/GeeksforGeeks/Longest%20Increasing%20Subsequence.md)。









