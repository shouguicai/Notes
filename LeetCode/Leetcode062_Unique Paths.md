---
title: 唯一路径
date: 2018-01-24
mathjax: false
tags: 动态规划
---

问题叙述：给定一个m*n大小的网格，问从左上角到右下角共有多少条路径。
限制每次只能向下或者向右移动一格。

## 递归解

为了叙述方便，记左上角起点坐标为(m,n)，右下角终点坐标为(1,1)。
递归的思路是，当前位置到终点的路径数等于从右侧网格和下方网格到达终点的路径之和，即
```
uniquePaths(m,n) = uniquePaths(m-1,n) + uniquePaths(m,n-1)
```
原始的递归程序为，
``` 
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
起点到终点的距离是m+n个单位长度，每一步至多有两种选择，时间复杂度是指数型的，
比较接近的上限是O(2^n)。

```
                         uniquePaths(m,n)
                       /                 
         uniquePaths(m-1,n)                      uniquePaths(m,n-1)
         /                                      /               
uniquePaths(m-2,n)   uniquePaths(m-1,n-1)     uniquePaths(m-1,n-1)   uniquePaths(m,n-2)
```
从递归调用树显然可以看出存在重复调用，uniquePaths(m-1,n-1)被调用了2次，对递推关系式迭代一次得到
```
uniquePaths(m,n) = uniquePaths(m-2,n) + uniquePaths(m,n-2) + 2*uniquePaths(m-1,n-1)
```
对应的程序为，
```
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

## 动态规划

虽然对递推关系式一次迭代后的程序能够在459ms内运行，可以想见还是存在重复调用的，
牺牲O(mn)的空间，用一个二维表格记下算好的子问题解，可以有效避免重复计算，bottom up manner的代码如下，
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