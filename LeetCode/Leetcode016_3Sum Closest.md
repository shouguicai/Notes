Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) 
    {
        int ans = nums[0] + nums[1] + nums[2];
        int sum = 0;
        int n = nums.size();
        // 排序
        sort(nums.begin(),nums.end());
        for(int i = 0; i<n-2;i++)
        {
            // 类似 11题
            // start, end 从两头向中间靠拢
            int start = i+1;
            int end = n-1;
            while(start < end)
            {
                sum = nums[i] + nums[start] + nums[end];
                int delta = sum - target;
                if (sum == target)
                {
                    return sum;
                }
                else if(sum < target)
                {
                    if (abs(nums[start+1] - nums[start] + delta) <= abs(delta))
                    {
                        start ++;
                        delta = nums[start+1] -  nums[start] + delta;
                    }
                    else if (abs(nums[start+1] - nums[start] + nums[end-1] - nums[end]+ delta) <= abs(delta))
                    {
                        start ++;
                        end --;
                        delta = nums[start+1] -  nums[start] + nums[end-1] - nums[end] + delta;
                    }
                    else
                    {
                        break;
                    }
                }
                else // sum > target
                {
                    if (abs(nums[end-1] - nums[end] + delta) <= abs(delta))
                    {
                        end --;
                        delta = nums[end-1] - nums[end] + delta;
                    }  
                    else if (abs(nums[start+1] - nums[start] + nums[end-1] - nums[end]+ delta) <= abs(delta))
                    {
                        start ++;
                        end --;
                        delta = nums[start+1] -  nums[start] + nums[end-1] - nums[end] + delta;
                    }
                    else
                    {
                        break;
                    }
                }
            }
            if (abs(sum-target)<abs(ans-target))
                ans = sum;
        }
        return ans;
    }
};
```