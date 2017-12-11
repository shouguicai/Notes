Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8, 
A solution set is: 
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

```
// Each number in C may only be used once in the combination.
// 递归解
// 相比于39题，要求每个元素最多使用一次
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) 
    {
        vector<vector<int>> ans;
        vector<int> path;
        int cur = 0;
        sort(candidates.begin(),candidates.end());
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
        // 因为一个节点最多访问一次，cur+1
        combinationSum(candidates, target - candidates[cur], path, ans, cur + 1);
        path.pop_back();
        // 跳过重复解
        while(cur < candidates.size()-1 && candidates[cur] == candidates[cur+1]) cur++;
        combinationSum(candidates, target, path, ans, cur+1);
    }
};
```