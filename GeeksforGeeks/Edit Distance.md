---
title: 编辑距离
date: 2018-01-23
mathjax: false
tags: 动态规划
---

问题叙述：给定两个字符串str1和str2，编辑距离是将str1转换为str2的最少操作次数，
允许的操作只有以下3种：

1. 插入一个字符，如：ac -> abc
2. 删除一个字符，如：abc -> ac
3. 替换一个字符，如：abc -> adc

上面的三个操作代价相同。

Examples:

1. 输入 str1 = "geek"，str2 = "gesek"，输出：1，在str1中插入一个s可得str2
2. 输入 str1 = "sunday"， str2 = "saturday"，输出：3，因为最后三个字符是一样的，只需要把"un"转变成"atur"即可，第一步把'n'替换成'r'，第二步插入't'，第三步插入'a'

## 子问题分析

基本的思路是从左往后或者从右往左逐字符处理。

以从右往左为例，对于每一步要处理的字符存在两种可能的情况。为叙述方便，记str1的长度为m，str2的长度为n，修改都是在str1上进行。

1. 如果两字符串的末尾字符相同，则什么都不用做，忽略末尾字符，求解剩余部分字符串的编辑距离即可，程序在长度 str1[:m-1] 和 str2[:n-1] 上继续；
2. 否则（末尾字符不同），这时存在3种可能的操作，递归计算这三种操作的编辑距离，并取最小值。
  - 插入，在str1末尾插入str2末尾字符，程序在 str1[:m] 和 str2[:n-1]上继续；
  - 删除，删除str1的末尾字符，程序在 str1[:m-1] 和 str2[:n]上继续；
  - 替换，将str1的末尾字符替换为str2的末尾字符，程序在 str1[:m-1] 和 str2[:n]上继续；

简单的递归实现如下，
```
int editDist(string str1 , string str2 , int m ,int n)
{
    // If first string is empty, the only option is to
    // insert all characters of second string into first
    if (m == 0) return n;
 
    // If second string is empty, the only option is to
    // remove all characters of first string
    if (n == 0) return m;
 
    // If last characters of two strings are same, nothing
    // much to do. Ignore last characters and get count for
    // remaining strings.
    if (str1[m-1] == str2[n-1])
        return editDist(str1, str2, m-1, n-1);
 
    // If last characters are not same, consider all three
    // operations on last character of first string, recursively
    // compute minimum cost for all three operations and take
    // minimum of three values.
    return 1 + min ( editDist(str1,  str2, m, n-1),    // Insert
                     editDist(str1,  str2, m-1, n),   // Remove
                     editDist(str1,  str2, m-1, n-1) // Replace
                   );
}
```
递归程序的时间复杂度是指数的，在最差的情况下（两个字符串没有相同字符），会需要$O(3^m)$次操作。

## 重复子问题

举个最坏情况的例子，输入str1="abc"，str2="xyz"，递归调用树如下，

```
                                 eD(3,3)
            /                       |                 \         
         eD(3,2)                 eD(2,3)           eD(2,2)
      /                             |                       \
eD(3,1) eD(2,2) eD(2,1)   eD(2,2) eD(1,3) eD(1,2)  eD(2,1) eD(1,2) eD(1,1)
      ...                        ...                  ... 
```

可以看出递归调用树中存在重复调用，eD(2,2)被调用了3次，eD(1,2)，eD(2,1)被调用了2次，编辑距离问题同时具有动态规划问题的两个属性（最优子结构和重复子问题），同其他DP问题一样，重复计算可以通过构建一个表得到避免。

自底向上的代码如下，
```
int editDistDP(string str1, string str2, int m, int n)
{
    // Create a table to store results of subproblems
    int dp[m+1][n+1];
 
    // Fill d[][] in bottom up manner
    for (int i=0; i<=m; i++)
    {
        for (int j=0; j<=n; j++)
        {
            // If first string is empty, only option is to
            // isnert all characters of second string
            if (i==0)
                dp[i][j] = j;  // Min. operations = j
 
            // If second string is empty, only option is to
            // remove all characters of second string
            else if (j==0)
                dp[i][j] = i; // Min. operations = i
 
            // If last characters are same, ignore last char
            // and recur for remaining string
            else if (str1[i-1] == str2[j-1])
                dp[i][j] = dp[i-1][j-1];
 
            // If last character are different, consider all
            // possibilities and find minimum
            else
                dp[i][j] = 1 + min(dp[i][j-1],  // Insert
                                   dp[i-1][j],  // Remove
                                   dp[i-1][j-1]); // Replace
        }
    }
 
    return dp[m][n];
}
```

时间复杂度是O(mn)，额外需要的空间是O(mn)。

同题见[LeetCode072](https://github.com/shouguicai/Notes/blob/master/LeetCode/Leetcode072_Edit%20Distance.md)
