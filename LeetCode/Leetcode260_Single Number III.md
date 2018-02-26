Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once. 
For example: 
Given nums = [1, 2, 1, 3, 2, 5], return [3, 5]. 

[牛客网](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

按位异或

1. 0^a = a; a^a = 0; a^b^a = (a^a)^b = b

2. 两个不相等的元素在位级表示上必定会有一位存在不同，

3. 将数组的所有元素异或得到的结果为不存在重复的两个元素异或的结果，

4. 据异或的结果1所在的最低位，把数字分成两半，每一半里都还有一个出现一次的数据和其他成对出现的数据,
  问题就转化为了两个独立的子问题“数组中只有一个数出现一次，其他数都出现了2次，找出这个数字”。

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) 
    {
      int n = nums.size();
      int diff = 0;
      int flag = 1;
      vector<int> ans(2,0);
      for(int i = 0;i < n;++i)
        diff ^= nums[i];
      // == 的优先级大于 &
      while((diff & flag) == 0) flag <<= 1;
      for(int i = 0;i < n;++i)
      {
        // == 的优先级大于 &
        if((nums[i] & flag) == 0)
          ans[0] ^= nums[i];
        else
          ans[1] ^= nums[i];
      }
      return ans;
    }
};
```