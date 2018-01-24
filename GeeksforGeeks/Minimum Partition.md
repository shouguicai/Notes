---
title: 最小分割
date: 2018-01-23
mathjax: false
tags: 动态规划
---

问题叙述：给定一个整数的集合，任务是把它分成两个子集S1，S2，要求两集合元素之和的差绝对值最小。如果给定的集合S中有n个元素，假设子集S1中有m个元素，子集S2中则有n-m个元素，题目要求输出abs(sum(S1) - sum(S2))，对应的是最小的集合划分方法。

Examples:
输入 arr[] = {1,6,11,5}
输出 1
解释
S1 = {1，5，6}，sum(S1) = 12
S2 = {11}，sum(S2) = 11

## 递归解

递归方法的思路是生成所有可能分割组合的*sum*，并检查哪一个是最优的。

对于数组arr中的第i个元素，我们有两种可选的操作，分到集合S1，或者不分到S1（即分到S2），递归程序返回两选择中abs(sum(S1) - sum(S2))小的那个。

子集元素之和的关系，sum(S2) = sum(S) - sum(S1)，因此算好了sum(S1)，sum(S2)很容易得到

```
// Function to find the minimum sum
// sumTotal = sum(S) 
// sumCalculated = sum(S1)
// i start form n to 0
int findMinRec(int arr[], int i, int sumCalculated, int sumTotal)
{
    // If we have reached last element.  Sum of one
    // subset is sumCalculated, sum of other subset is
    // sumTotal-sumCalculated.  Return absolute difference
    // of two sums.
    if (i==0)
        return abs((sumTotal-sumCalculated) - sumCalculated);
 
 
    // For every item arr[i], we have two choices
    // (1) We do not include it first set
    // (2) We include it in first set
    // We return minimum of two choices
    return min(findMinRec(arr, i-1, sumCalculated+arr[i-1], sumTotal),
               findMinRec(arr, i-1, sumCalculated, sumTotal));
}
```

程序对集合S中每个元素有两种可能的操作，集合S共n个元素，因此时间复杂度为O(2^n)。

## 动态规划

当集合元素之和不是很大时，最小分割问题可以使用动态规划求解。创建一个二维数组
```dp[n+1][sum(arr)+1]```，然后自下而上的方式求解。

```
dp[i][j] = true  如果 存在 集合{S的第一个到第i个元素} 的某个子集的和等于 j;
           false 其他
其中，i from {1,...,n}，j from {0,...,sum(arr)}，j = sum(S1)
```

### 递归关系

```dp[i][j]```，表示 集合```arr[0:i-1]``` 是否存在某个子集的和等于 j，```dp[][]```递推关系如下，

- 子集S1不包含第i个元素arr[i-1]（excluded）

    ```dp[i][j] = dp[i-1][j]```

- S1包含第i个元素（included）

    ```dp[i][j] = dp[i-1][j-arr[i-1]]```

### 递归程序

表```dp[][]```更新完之后，算法需要在所有满足```dp[n][j] == true```的 j 中找到使
```abs(sum(S2) - sum(S1))```, i.e. ```abs(sum(arr) - 2*j)```最小的j，
满足条件的j应该尽可能接近```sum(arr)/2```。

```
// Returns the minimum value of the difference of the two sets.
int findMin(int arr[], int n)
{
    // Calculate sum of all elements
    int sum = 0; 
    for (int i = 0; i < n; i++)
        sum += arr[i];
 
    // Create an array to store results of subproblems
    bool dp[n+1][sum+1];
 
    // Initialize first column as true. 0 sum is possible 
    // with all elements.
    for (int i = 0; i <= n; i++)
        dp[i][0] = true;
 
    // Initialize top row, except dp[0][0], as false. With
    // 0 elements, no other sum except 0 is possible
    for (int i = 1; i <= sum; i++)
        dp[0][i] = false;
 
    // Fill the partition table in bottom up manner
    for (int i=1; i<=n; i++)
    {
        for (int j=1; j<=sum; j++)
        {
            // If i'th element is excluded
            dp[i][j] = dp[i-1][j];
 
            // If i'th element is included
            if (arr[i-1] <= j)
                dp[i][j] |= dp[i-1][j-arr[i-1]];
        }
    }
  
    // Initialize difference of two sums. 
    int diff = INT_MAX;
     
    // Find the largest j such that dp[n][j]
    // is true where j loops from sum/2 t0 0
    for (int j=sum/2; j>=0; j--)
    {
        // Find the 
        if (dp[n][j] == true)
        {
            diff = sum-2*j;
            break;
        }
    }
    return diff;
}
```

其中，```dp[i][j] |= dp[i-1][j-arr[i-1]];```是为了不改变S1不包含```arr[i-1]```时```dp[i][j]```为true的情况。```dp[i][j] == false```时，```dp[i][j] = dp[i-1][j-arr[i-1]]```。

算法的时间复杂度为O(n*sum(S))，为多项式时间。