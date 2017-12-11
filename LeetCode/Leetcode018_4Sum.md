Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]

```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) 
    {
        vector<vector<int>> ans;
        vector<int> solution(4);
        int n = nums.size();
        // 排序
        sort(nums.begin(),nums.end());
        for(int i = 0; i<n-3;i++)
        {
            for (int j = i+1;j<n-2;j++)
            {
            // start, end 从两头向中间靠拢
            int start = j+1;
            int end = n-1;
            while(start < end)
            {
                int sum = nums[i] + nums[j] + nums[start] + nums[end];
                if (sum == target)
                {
                    solution[0] = nums[i];
                    solution[1] = nums[j];
                    solution[2] = nums[start];
                    solution[3] = nums[end];
                    ans.push_back(solution);
                    // 跳过相同的值，避免重复解
                    while(start < end && nums[start] == solution[2]) start ++;
                    while(start < end && nums[end] == solution[3]) end --;
                }
                // nums 是非递减排序的
                // sum < target , 向增方向
                else if (sum < target)
                {
                    start ++;
                }
                // sum > target , 向减方向
                else 
                {
                    end --;
                }
            }
            // 避免重复求解
            // 如果nums[j] == nums[j+1]，那么nums[j]时的解已经包含了nums[j+1]时的解
            while(j < n-2 && nums[j] == nums[j+1])
                j++;
            }
            while(i < n-2 && nums[i] == nums[i+1])
                i++;
        }
        return ans;    
    }
};
```