Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.


```
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) 
    {
        unordered_map<int,int> mymap;
        for(int i = 0;i < nums.size();i++)
        {
            if(mymap.find(nums[i]) != mymap.end() && abs(i - mymap[nums[i]]) <= k)
                return true;
            else
                mymap[nums[i]] = i;
        }
        return false;
    }
};
```