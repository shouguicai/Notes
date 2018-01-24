---
title: 跨越一定距离的路径总数
date: 2018-01-24
mathjax: false
tags: 动态规划
---

问题叙述：给定一个距离'dist'，求可能的跨越方案总数。每次可以选择跨越1，2或者3步。

Examples:

1. 输入 n=3 输出：4 四种方案为{1 + 1 + 1, 1 + 2, 2 + 1, 3}
2. 输入 n=4 输出：7

## 递归解

```
// Returns count of ways to cover 'dist'
int printCountRec(int dist)
{
    // Base cases
    if (dist<0)    return 0;
    if (dist==0)  return 1;
 
    // Recur for all previous 3 and add the results
    return printCountRec(dist-1) +
           printCountRec(dist-2) +
           printCountRec(dist-3);
}
```

时间复杂度是指数型的，比较接近的上限是$O(3^n)$。

## 动态规划

容易看出递归调用里面存在重复，
举个例子，假设```dist = 6```，
可以通过连续2次```printCountRec(dist-1)```到达4，
或者通过一次```printCountRec(dist-2)```到达4，
这边单是子问题4就被调用了2次。
该问题存在重复子问题，可以动态规划解决。
自下而上的程序如下，
```
int printCountDP(int dist)
{
    int count[dist+1];
 
    // Initialize base values. There is one way to cover 0 and 1
    // distances and two ways to cover 2 distance
    count[0]  = 1,  count[1] = 1,  count[2] = 2;
 
    // Fill the count array in bottom up manner
    for (int i=3; i<=dist; i++)
       count[i] = count[i-1] + count[i-2] + count[i-3];
 
    return count[dist];
}
```



