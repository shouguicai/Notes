You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

DP
```
/*
  对房间k,存在两种选择，偷(include)或者不偷(exclude)
  1. 偷(include)
      dp_include[k] = num[k] + dp_exclude[k-1]; (房间k的钱+房间k-1之前获得的所有钱并且不偷房间k-1)
  2. 不偷(exclude)
      dp_exclude[k] = max(dp_include[k-1],dp_exclude[k-1]);取偷或者不偷房间k-1中的最大值

  最后返回 max(dp_include[end],dp_exclude[end])
*/

class Solution {
public:
    int rob(vector<int>& nums) 
    {
      int n = nums.size();
      if (n == 0)
        return NULL;
      vector<int> dp_include(n);
      vector<int> dp_exclude(n);
      dp_include[0] = nums[0];
      dp_exclude[0] = 0;
      for(int i = 1;i < n;i++)
      {
        dp_include[i] = nums[i] + dp_exclude[i-1];
        dp_exclude[i] = max(dp_include[i-1],dp_exclude[i-1]);
      }
      return max(dp_exclude[n-1],dp_include[n-1]);
    }
};
```