---
title: 最长公共子序列问题
date: 2018-01-22 13:13:00
mathjax: true
tags: algorithm,geeksforgeeks
---

问题叙述：给定两个序列，找出两者中都存在的最长子序列的长度。子序列是以相同次序出现的序列，不要求连续。例如，“abc”, “abg”, “bdf”, “aeg”, ‘”acefg”等等都是序列 “abcdefg”的子序列。一个长度为n的序列总共有2^n个不同的子序列。

Examples:

1. 输入序列“ABCDGH” 和 “AEDFHR” 的LCS是 “ADH”，长度为3；
2. 输入序列“ABCDGH” 和 “GXTXAYB” 的LCS是 “GTAB” ，长度为4。

## 最优子结构
记输入序列为X[0...m-1]和Y[0...n-1]，m,n分别是两序列的长度。
记L(X[0...m-1],Y[0...n-1])为序列X，Y的最长公共子序列长度。

L(X[0...m-1],Y[0...n-1])具有如下递归关系，

- if X[m-1] == Y[n-1],
    
    L(X[0..m-1], Y[0..n-1]) = 1 + L(X[0..m-2], Y[0..n-2])

- else // X[m-1] != Y[n-1]
    
    L(X[0..m-1], Y[0..n-1]) = MAX ( L(X[0..m-2], Y[0..n-1]), 
                                    L(X[0..m-1], Y[0..n-2])

可以看出，原问题的解可以通过求解子问题得到，LCS问题具有最优子结构，可以动态规划求解。简单的递归程序如下，
```
/* Returns length of LCS for X[0..m-1], Y[0..n-1] */
int lcs( char *X, char *Y, int m, int n )
{
   if (m == 0 || n == 0)
     return 0;
   if (X[m-1] == Y[n-1])
     return 1 + lcs(X, Y, m-1, n-1);
   else
     return max(lcs(X, Y, m, n-1), lcs(X, Y, m-1, n));
}
```
最坏情况下的时间复杂度是$O(2^n)$，此时X和Y的所有字母都不匹配，LCS的长度为0。

## 重复子问题

下面给出是当输入序列为 “AXYT” 和 “AYZX”时递归调用树的局部。
```
                         lcs("AXYT", "AYZX")
                       /                 
         lcs("AXY", "AYZX")            lcs("AXYT", "AYZ")
         /                              /               
lcs("AX", "AYZX") lcs("AXY", "AYZ")   lcs("AXY", "AYZ") lcs("AXYT", "AY")
```
可以看出， lcs(“AXY”, “AYZ”)被计算了两次，LCS递归程序中有重复计算存在。可以使用Memoization或者Tabulation避免重复计算，下面是LCS问题的tabulated实现。时间复杂度是O(mn)。
```
/* Dynamic Programming C/C++ implementation of LCS problem */
/* Returns length of LCS for X[0..m-1], Y[0..n-1] */
int lcs( char *X, char *Y, int m, int n )
{
   int L[m+1][n+1];
   int i, j;
  
   /* Following steps build L[m+1][n+1] in bottom up fashion. Note 
      that L[i][j] contains length of LCS of X[0..i-1] and Y[0..j-1] */
   for (i=0; i<=m; i++)
   {
     for (j=0; j<=n; j++)
     {
       if (i == 0 || j == 0)
         L[i][j] = 0;
  
       else if (X[i-1] == Y[j-1])
         L[i][j] = L[i-1][j-1] + 1;
  
       else
         L[i][j] = max(L[i-1][j], L[i][j-1]);
     }
   }
    
   /* L[m][n] contains length of LCS for X[0..n-1] and Y[0..m-1] */
   return L[m][n];
}
```
这边的算法只求解了LCS的长度，LCS问题的其他版本有：
[打印LCS](https://www.geeksforgeeks.org/printing-longest-common-subsequence/)，[LCS问题的空间优化版本](https://www.geeksforgeeks.org/space-optimized-solution-lcs/)。