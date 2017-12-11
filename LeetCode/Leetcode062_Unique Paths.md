A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

``` 
Solution 1
// 递归
// 递推关系：
// f(m,n) = f(m-1,n) + f(m,n-1)
// Time Limit Exceeded
class Solution {
public:
    int uniquePaths(int m, int n) 
    {
        if (m == 1 || n == 1)
            return 1;
        return uniquePaths(m-1,n) + uniquePaths(m,n-1);
    }
};
```

```
Solution 2
// 递归形式 f(m,n) = f(m-1,n) + f(m,n-1)存在重复计算
// 如 f(m-1,n) ，f(m,n-1) 都会计算一遍 f(m-1,n-1)
// 减少重复计算，递推关系修改为：f(m,n) = f(m-2,n) + f(m,n-2) +2*f(m-1,n-1)
// PASSED 459ms
class Solution {
public:
    int uniquePaths(int m, int n) 
    {
        if (m == 1 || n == 1)
            return 1;
        if (m < 1 || n < 1)
            return 0;
        return uniquePaths(m-2,n) + uniquePaths(m,n-2) + 2*uniquePaths(m-1,n-1);
    }
};
```

```
Solution 3
// 用一个数组保存每一个点的解
// PASSED 0 ms
class Solution
{
public:
    int uniquePaths(int m, int n) 
    {
        vector<vector<int>> path(m,vector<int>(n,1));
        for (int i=1;i<m;i++)
            for(int j=1;j<n;j++)
                path[i][j] = path[i-1][j] + path[i][j-1];
        return path[i-1][j-1];
    }
};
```