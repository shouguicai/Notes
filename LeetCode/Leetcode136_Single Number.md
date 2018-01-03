Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

使用multimap，注意insert,count,map.begin()->first的用法
```
// 26 ms
class Solution {
public:
    int singleNumber(vector<int>& nums)
    {
        unordered_multimap<int,int> mymap;
        for(int i = 0;i<nums.size();i++)
        {
          mymap.insert({nums[i],i});
          if (mymap.count(nums[i]) == 2)
            mymap.erase(nums[i]);
        }
        return mymap.begin()->first;
    }
};
```

Approach #2 Bit Manipulation 按位异或
```
/*
16 ms
O(1) space, O(n) time
0^a = a;
a^a = 0;
a^b^a = (a^a)^b = b
*/
class Solution {
public:
    int singleNumber(vector<int>& nums)
    {
        int result = 0;
        for(int i = 0;i<nums.size();i++)
          result ^= nums[i];
        return result;
    }
};
```