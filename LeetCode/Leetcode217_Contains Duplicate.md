Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) 
    {
        unordered_set<int> myset;
        for(int i = 0;i < nums.size(); i++)
        {
            if(myset.find(nums[i]) != myset.end())
                return true;
            else
                myset.insert(nums[i]);
        }
        return false;
    }
};
```