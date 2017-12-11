Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) 
    {
        vector<vector<int>> ans;
        vector<int> solution(3);
        int n = nums.size();
        // 排序
        sort(nums.begin(),nums.end());
        for(int i = 0; i<n-2;i++)
        {
            // 有点类似 11题
            // start, end 从两头向中间靠拢
            int start = i+1;
            int end = n-1;
            while(start < end)
            {
                int sum = nums[i] + nums[start] + nums[end];
                if (sum == 0)
                {
                    solution[0] = nums[i];
                    solution[1] = nums[start];
                    solution[2] = nums[end];
                    ans.push_back(solution);
                    // 跳过相同的值，避免重复解
                    while(start<end && nums[start] == solution[1]) start ++;
                    while(start<end && nums[end] == solution[2]) end --;
                }
                // nums 是非递减排序的
                // sum < 0 , 向增方向
                else if (sum < 0)
                {
                    start ++;
                }
                // sum > 0 , 向减方向
                else 
                {
                    end --;
                }
            }
            // 避免重复求解
            // 如果nums[i] == nums[i+1]，那么nums[i]时的解已经包含了nums[i+1]时的解
            while(i < n-2 && nums[i] == nums[i+1])
                i++;
        }
        return ans;
    }
};
```