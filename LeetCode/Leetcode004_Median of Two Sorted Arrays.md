There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
Example 2:
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2)
    {
        int N = nums1.size() + nums2.size();
        if (N % 2 == 1) // 奇数
        	return findKthInTwoArray(nums1,nums2,(N+1)/2);
        else          // 偶数
        	return (findKthInTwoArray(nums1,nums2,N/2) + findKthInTwoArray(nums1,nums2,1+N/2))/2.0;
    }
private:
	int findKthInTwoArray(vector<int>& nums1, vector<int>& nums2, int k)
	{
		//pos1,pos2标记即将被考查的元素标号
		//算法停止时，共有k个元素被“加入”排序
		//返回最后一个被“加入”的元素
		int m = nums1.size();
		int n = nums2.size();
		int pos1 = 0;
		int pos2 = 0;
		while (pos1 + pos2 < k)
		{
			// 可以进一步考虑用二分搜索改进两个小while
        	if (m-pos1 == 0 || n-pos2 == 0) // 如果剩余输入为空数组，包括初始出入为空
            	return (m-pos1 == 0 ? nums2[k-m-1] : nums1[k-n-1]);
			else if (nums1[pos1] <= nums2[pos2])
				while(pos1 < m && nums1[++pos1] <= nums2[pos2]);
       		else     // (nums1[pos1] > nums2[pos2])
				while(pos2 < n && nums1[pos1] >= nums2[++pos2]);
		}
		int delta = pos1 + pos2 - k;
        
        if (pos1-delta-1<0 || pos2-delta-1 <0) // 防止出现非法负标号
            return (pos1-delta-1<0 ? nums2[pos2-delta-1]:nums1[pos1-delta-1]);
        else // 取这两者max即可
            return max(nums1[pos1-delta-1],nums2[pos2-delta-1]);
	}
};
```