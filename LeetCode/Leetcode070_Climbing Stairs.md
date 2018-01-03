You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.


Example 1:

Input: 2
Output:  2
Explanation:  There are two ways to climb to the top.

1. 1 step + 1 step
2. 2 steps
Example 2:

Input: 3
Output:  3
Explanation:  There are three ways to climb to the top.

1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

参考62题 递归形式解有重复计算的部分，
DP，用一个数组保存中间计算
```
// 3 ms
class Solution {
public:
    int climbStairs(int n) 
    {
        vector<int> path(n);
        path[0] = 1;
        path[1] = 1;
        for(int i = 2;i < n;i++)
            path[i] = path[i-1] + path[i-2];
        return (path[n-1] + path[n-2]);
    }
};
```
