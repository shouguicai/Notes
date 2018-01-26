---
title: 最大积子数组
date: 2018-01-26
mathjax: false
tags: 动态规划
---

问题叙述:求解给定数组的最大积连续子数组。

Examples:
1. 输入 [2,3,-2,4] 输出 6 解释 最大积的连续子数组是[2,3]

## Naive解法

原始的思路是枚举所有的连续子数组，并寻找最大积。

```
//  Time Limit Exceeded 
class Solution {
public:
    int maxProduct(vector<int>& nums) 
    {
        int max_product = nums[0];
        for(int i = 0; i < nums.size(); i++)
        {
            int product = nums[i];
            max_product = max(max_product,product);
            for(int j = i + 1; j < nums.size(); j++)
            {
                product *= nums[j];
                max_product = max(max_product,product);
            }
        }
        return max_product;
    }
};
```
时间复杂度O(n^2);

## 动态规划

用dp_max[i],dp_min[i]分别记录nums[i:end]中包含nums[i]的最大最小积。

对于每一步待处理的nums[i]，考虑```dp_min[i+1] < dp_max[i+1]```


- 若```nums[i] > 0```，```dp_min[i+1] * nums[i] < dp_max[i+1] * nums[i]```

        dp_max[i] = max(nums[i],nums[i] * dp_max[i+1]);
        dp_min[i] = min(nums[i],nums[i] * dp_min[i+1]);


- 否则```nums[i] < 0```，不等号方向改变，```dp_min[i+1] * nums[i] > dp_max[i+1] * nums[i]```

        dp_max[i] = max(nums[i],nums[i] * dp_min[i+1]);
        dp_min[i] = min(nums[i],nums[i] * dp_max[i+1]);

取max和min是因为考虑到 dp_min[i+1] , dp_max[i+1]和nums[i]异号的可能，代码如下，


```
// 6ms

class Solution {
public:
    int maxProduct(vector<int>& nums) 
    {
        int n = nums.size();
        int max_product = nums[n-1];
        vector<int> dp_max(n);
        vector<int> dp_min(n);
        dp_max[n-1] = nums[n-1];
        dp_min[n-1] = nums[n-1];
        for(int i = n -2; i >= 0; i--)
        {   
            if(nums[i] < 0)
            {
                dp_max[i] = max(nums[i],nums[i] * dp_min[i+1]);
                dp_min[i] = min(nums[i],nums[i] * dp_max[i+1]);
            }
            else
            {
                dp_max[i] = max(nums[i],nums[i] * dp_max[i+1]);
                dp_min[i] = min(nums[i],nums[i] * dp_min[i+1]);
            }
            max_product = max(max_product,dp_max[i]);
        }
        return max_product;
    }
};
```

算法时间复杂度O(n)。


