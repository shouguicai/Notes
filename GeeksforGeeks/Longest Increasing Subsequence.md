---
title: 最长递增子序列问题
date: 2018-01-23
mathjax: false
tags: 动态规划
---

问题叙述：要求找出给定序列中最长的递增子序列的长度，递增子序列中的元素满足递增性质，不要求元素相邻。

Examples:

1. 输入序列{10, 22, 9, 33, 21, 50, 41, 60, 80}的LIS是{10, 22, 33, 50, 60, 80}，长度为6；
2. 输入序列{3, 10, 2, 1, 20}的LIS是{3, 10, 20}，长度为3；
3. 输入序列{3, 2}的LIS是{3}和{2}，长度为1。

## 最优子结构
记输入序列为arr[0...n-1],n是序列的长度。
记L(i)为以元素arr[i]为递增子序列最后一个字符的LIS长度。

L(i)具有如下递归关系，

- if 0 < j < i and arr[j] < arr[i],
    
    L(i) = 1 + max(L(j))

- else // 不存在满足条件的j
    
    L(i) = 1

为了找到给定序列的LIS长度，我们需要得到max(L(i))，其中0 < i < n，
可以看出，LIS问题具有最优子结构，可以动态规划求解。简单的递归程序如下，
```
/* To make use of recursive calls, this function must return
   two things:
   1) Length of LIS ending with element arr[n-1]. We use
      max_ending_here for this purpose
   2) Overall maximum as the LIS may end with an element
      before arr[n-1] max_ref is used this purpose.
   The value of LIS of full array of size n is stored in
   *max_ref which is our final result */
int _lis( int arr[], int n, int *max_ref)
{
    /* Base case */
    if (n == 1)
        return 1;
 
    // 'max_ending_here' is length of LIS ending with arr[n-1]
    int res, max_ending_here = 1; 
 
    /* Recursively get all LIS ending with arr[0], arr[1] ...
       arr[n-2]. If   arr[i-1] is smaller than arr[n-1], and
       max ending with arr[n-1] needs to be updated, then
       update it */
    for (int i = 1; i < n; i++)
    {
        res = _lis(arr, i, max_ref);
        if (arr[i-1] < arr[n-1] && res + 1 > max_ending_here)
            max_ending_here = res + 1;
    }
 
    // Compare max_ending_here with the overall max. And
    // update the overall max if needed
    if (*max_ref < max_ending_here)
       *max_ref = max_ending_here;
 
    // Return length of LIS ending with arr[n-1]
    return max_ending_here;
}

// The wrapper function for _lis()
int lis(int arr[], int n)
{
    // The max variable holds the result
    int max = 1;
 
    // The function _lis() stores its result in max
    _lis( arr, n, &max );
 
    // returns max
    return max;
}
```

## 重复子问题

以array长度为4为例，递归调用树如下，
```
              lis(4)
        /        |     
      lis(3)    lis(2)   lis(1)
     /           /
   lis(2) lis(1) lis(1)
   /
lis(1)
```
明显存在重复计算，自下而上的tabluated实现如下，
```
/* lis() returns the length of the longest increasing
  subsequence in arr[] of size n */
int lis( int arr[], int n )
{
    int *lis, i, j, max = 0;
    lis = (int*) malloc ( sizeof( int ) * n );
 
    /* Initialize LIS values for all indexes */
    for (i = 0; i < n; i++ )
        lis[i] = 1;
 
    /* Compute optimized LIS values in bottom up manner */
    for (i = 1; i < n; i++ )
        for (j = 0; j < i; j++ ) 
            if ( arr[i] > arr[j] && lis[i] < lis[j] + 1)
                lis[i] = lis[j] + 1;
 
    /* Pick maximum of all LIS values */
    for (i = 0; i < n; i++ )
        if (max < lis[i])
            max = lis[i];
 
    /* Free memory to avoid memory leak */
    free(lis);
 
    return max;
}
```
注意上面的程序的时间复杂度是O(n^2)，查看时间复杂度为O(nlogn)的[版本](https://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/)。