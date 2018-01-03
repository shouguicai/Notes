Given an array and a value, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:

Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

```
// 用两个指针完成O(1)空间复杂度的指定元素删除
class Solution {
public:
    int removeElement(vector<int>& nums, int val) 
    {
        int i = 0;
        for(int j=0;j<nums.size();j++)
        {
            if(nums[j] != val)
            {
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
};
```