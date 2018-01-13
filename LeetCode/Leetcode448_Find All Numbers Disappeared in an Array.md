Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

Example:

Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]

```
/** 通过改变符号来标记元素是否出现
*   1. 给定的输入元素为正，用负号标记出现的情况
*   2. 用abs操作保持元素值不受负号影响
*/
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> ans;
        int m;
        for(int i = 0;i < n; i++)
        {
            m = abs(nums[i]) -1; // abs(nums[i])是在数组中出现的元素
                                 // 在数组相应的标号位置(abs(nums[i]) -1)标记负号吗，表示出现
            nums[m] = nums[m] > 0 ? -nums[m] : nums[m]; // 未标记则标记负号
                                                        // 已标记则保持负号

        }
        for(int i = 0;i < n; i++)
        {
            if(nums[i] > 0)
                ans.push_back(i+1);
        }
        return ans;
    }
};
```