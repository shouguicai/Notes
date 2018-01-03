Given a set of distinct integers, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:

[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        vector<vector<int>> ans;
        vector<int> path;
        int cur = 0;
        FindAllSubsets(ans,nums,path,cur);
        return ans;
    }

private:
    void FindAllSubsets(vector<vector<int>>& ans, vector<int> nums, vector<int>& path,int cur)
    {
      if (cur == nums.size())
      {
          ans.push_back(path);
          return ;
      }
      path.push_back(nums[cur]);
      FindAllSubsets(ans,nums,path,cur+1);
      path.pop_back();
      FindAllSubsets(ans,nums,path,cur+1);
    }
};
```