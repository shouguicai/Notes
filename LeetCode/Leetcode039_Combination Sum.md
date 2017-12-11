Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [2, 3, 6, 7] and target 7, 
A solution set is: 
[
  [7],
  [2, 2, 3]
]

```
// The same repeated number may be chosen from C unlimited number of times.
// 递归解
// 算法过程类似DFS
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) 
    {
        vector<vector<int>> ans;
        vector<int> path;
        int cur = 0;
        combinationSum(candidates, target, path, ans, cur);
        return ans;
    }
private:
    void combinationSum(vector<int>& candidates, int target, vector<int>& path, vector<vector<int>>& ans , int cur)
    {
        if (target == 0)
        {
            ans.push_back(path);
            return;
        }

        if(cur == candidates.size() || target < 0)
            return;

        path.push_back(candidates[cur]);
        // 因为允许多次使用同一个元素，cur没有+1
        combinationSum(candidates, target - candidates[cur], path, ans, cur);
        path.pop_back();
        combinationSum(candidates, target, path, ans, cur + 1);
    }
};
```