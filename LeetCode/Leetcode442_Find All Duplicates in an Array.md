Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?

Example:
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]

类似No.448题，
```
/** 通过改变符号来标记元素是否出现
*   1. 第一次出现时用负号标记
*   2. 用abs操作保持元素值不受负号影响
*   3. 第二次出现则直接输出答案
*/
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) 
    {
        int n = nums.size();
        int m;
        vector<int> ans;
        for(int i = 0; i < n;i++)
        {
            m = abs(nums[i]) - 1;
            if(nums[m] < 0)
                ans.push_back(m + 1);
            else
                nums[m] = - nums[m];
        }
        return ans;
    }
};
```