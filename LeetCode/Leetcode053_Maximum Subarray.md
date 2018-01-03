Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

```
// DP
/* 关键：寻找子问题
   子问题: maxSubArray(int A[], int i)，表示数组A[0:i]中包含的最大和，并且最大和子序列包含A[i]
   子问题与原问题的关系：
       maxSubArray(A, i) = A[i] + maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0 ; 
*/
class Solution {
public:
    int maxSubArray(vector<int>& nums) 
    {
    	int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];
        int maxm = dp[0];
        for(int i=1;i<n;i++)
        {
        	dp[i] = nums[i] + (dp[i-1]>0? dp[i-1]:0);
        	maxm = max(maxm,dp[i]);
        }
        return max;
    }
};
```