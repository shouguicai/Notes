Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Example 1:

Input: [1,3,5,6], 5
Output: 2
Example 2:

Input: [1,3,5,6], 2
Output: 1
Example 3:

Input: [1,3,5,6], 7
Output: 4
Example 1:

Input: [1,3,5,6], 0
Output: 0

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) 
    {
        int n = nums.size();
        int i = 0;
        int j = n-1;
        if(target<= nums[0])
            return 0;
        if(target> nums[n-1])
            return n;
        for(;i<j;)
        {
            int mid = (i + j)/2;
            if (nums[mid] < target)
            {
                i = mid;
            }
            else if(nums[mid]>target)
            {
                j = mid;
            }
            else
            {
                return mid;
            }
           if(i == j -1)
             return i+1;
        }
    }
};
```