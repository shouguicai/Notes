Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.


Approach #1 HashMap
用hashmap统计元素出现的次数, No.697 , No.136
```
// 29 ms
class Solution {
public:
    int majorityElement(vector<int>& nums) 
    {
        unordered_map<int,int> count(0);
        for(int i = 0;i < nums.size();i++)
        {
          count[nums[i]] ++;
          if(count[nums[i]] > nums.size()/2)
            return nums[i];
        }
    }
};
```

Approach #2 分而治之
```
/*
  找出左右两部的majorityElement，然后判决全局的majorityElement；

*/
class Solution {
public:
  int majorityElement(vector<int>& nums) 
  {
    return majorityElementPart(nums,0,nums.size()-1);
  }
private:
  int majorityElementPart(vector<int>& nums, int begin, int end)
  {
    if(begin == end)
      return nums[begin];

    int mid = (end - begin)/2 + begin; // 将数组分割为左右两部分
    int left_major = majorityElementPart(nums,begin,mid); // 找出左右两部的majorityElement
    int right_major = majorityElementPart(nums,mid+1,end);
    if(left_major == right_major)  // 如果左右两边的majorityElement相同，则找到答案并返回
      return left_major;
    int left_count = countInRange(nums,left_major,begin,end); // 否则，比较它们在整个数组中出现的次数
    int right_count = countInRange(nums,right_major,begin,end);
    return (left_count > right_count ? left_major : right_major); // 返回出现次数多的那个数
  }
  int countInRange(vector<int>& nums, int target,int begin, int end)
  {
    int count = 0;
    for(int i = begin; i <= end; i++)
    {
      if(nums[i] == target)
        count ++;
    }
    return count;
  }
};
```