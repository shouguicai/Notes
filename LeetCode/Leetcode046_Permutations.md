Given a collection of distinct numbers, return all possible permutations.

For example,
[1,2,3] have the following permutations:

```
// 递归解
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) 
    {
        vector<vector<int>> ans;
        vector<int> path();
        vector<bool> flag(nums.size(),false);
        int count = 0;
        permute(nums, path, ans, flag, count);
        return ans;
    }
private:
    void permute(vector<int>& nums, vector<int>& path, vector<vector<int>>& ans, vector<bool>& flag, int count)
    {
        if(count == nums.size())
        {
            ans.push_back(path);
            return;
        }

        for(int i = 0; i< nums.size(); i++)
        {
            if(!flag[i])
            {
                path.push_back(nums[i]);
                flag[i] = true;
                permute(nums, path, ans, flag, count + 1);
                flag[i] = false;
                path.pop_back();
            }
        }
    }
};
```