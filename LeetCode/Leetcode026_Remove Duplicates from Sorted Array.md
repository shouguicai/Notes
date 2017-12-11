Given a sorted array, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example:

Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the new length.

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        int n = nums.size();
        int count = 0;
        int pos = 0;
        for (int i=0;i<n-1;i++)
        {    
            int begin = i+1;
            int end = n-1;
            int middle;
            while(begin < end)
            {
                middle = (begin + end)/2;
                // 因为数组是非递减排序的，
                //nums[middle]不可能小于nums[i]
                if (nums[i] < nums[middle])
                {
                    end = middle;
                }
                else // nums[i] == nums[middle]
                {
                    count += middle - i;
                    i = middle;
                    break;
                }
            }
            // 跳过重复元素
            while(i < n-1 && nums[i+1] == nums[i])
            {
                i ++;
                count ++;
            }
            // ++pos 先自增再使用
            nums[++pos] = nums[i+1];
        }
        return (n - count);
    }
};
```