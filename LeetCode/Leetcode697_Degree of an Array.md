Given a non-empty array of non-negative integers nums, the degree of this array is defined as the maximum frequency of any one of its elements.

Your task is to find the smallest possible length of a (contiguous) subarray of nums, that has the same degree as nums.

Example 1:
Input: [1, 2, 2, 3, 1]
Output: 2
Explanation: 
The input array has a degree of 2 because both elements 1 and 2 appear twice.
Of the subarrays that have the same degree:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
The shortest length is 2. So return 2.
Example 2:
Input: [1,2,2,3,1,4,2]
Output: 6
Note:

nums.length will be between 1 and 50,000.
nums[i] will be an integer between 0 and 49,999.

```
// 使用两个map分别记录每个元素出现的次数，第一次出现的位置
// 并不断比较最大频次和最短长度
// https://leetcode.com/problems/degree-of-an-array/discuss/108674
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) 
    {
        unordered_map<int,int> startIndex,count;
        int len = nums.size();
        int fre = 0;
        for(int i = 0;i < nums.size();i++)
        {
        	if(startIndex.count(nums[i]) == 0) // 检查nums[i]是否以及记录过了
        		startIndex[nums[i]] = i;
        	count[nums[i]] ++;

        	if(count[nums[i]] == fre)  //如果两个元素出现次数相同
        		len = min(i- startIndex[nums[i]]+1,len);
                                     // 则比较他们对应同度最短子序列的长度
        	if(count[nums[i]] > fre) //更新fre,len
        	{
        		len = i - startIndex[nums[i]] + 1;
        		fre = count[nums[i]];
        	}
        }
        return len;
    }
};
```