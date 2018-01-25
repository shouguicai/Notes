---
title: 解码方式
date: 2018-01-24
mathjax: false
tags: 动态规划
---

问题叙述:一条含有字母```A-Z```的信息按照下面的规则被编码为数字串，
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
问对于给定一个数字串，共有多少种解码可能。

Examples:
1. 输入 "12" 输出 2，解释：输入可以被解码为"AB" (1 2) 或 "L" (12)

## 递归解

可以被解码的数字是1（A）~ 26（Z）。对输入字符串从左往右逐字符处理，
对于每一步要处理的字符存在两种可能的情况，

1. 当前字符和下一位字符构成的数字超过解码范围26，那么只需要忽略该字符，求解剩余部分字符串的解码方式数；
2. 当前字符和下一位字符构成的数字在解码范围26以内，则存在两种解码方式，
    
    - 两个数字单独解码，
    - 两个数字作为两位数一起解码。

递归计算这两种操作后的解码方式数，并求和。
要注意```'0'```不能单独解码。
简单的递归实现如下，

```
// 224/259
// test 225 length:100
// 超时
class Solution {
public:
    int numDecodings(string s) 
    {
        if(s.empty())
            return 0;
        return numDecodingWays(s, 0);
    }
private:
    int flag = 1;
    int numDecodingWays(string s, int cur)
    {
        if(s[cur] == '0' && cur < s.length())
        {
            flag = 0;
            return 0;
        }
        if(cur >= s.length()-1)
            return 1;

        if( (s[cur] - '0')*10 + (s[cur + 1] - '0') <= 26)
        {
            if(s[cur + 1] == '0')
            {
                int count = numDecodingWays(s, cur + 2) * flag;
                if(flag == 0) flag = 1;
                return count;
            }

            else
            {
                int count1 = numDecodingWays(s, cur + 1) * flag;
                if(flag == 0) flag = 1;
                int count2 = numDecodingWays(s, cur + 2) * flag;
                if(flag == 0) flag = 1;
                return count1 + count2;
            }
        }
        else
        {
            int count = numDecodingWays(s, cur + 1) * flag;
            if(flag == 0) flag = 1;
            return count; 
        }
    }
};
```

其中，```flag = 0```用来标记遇到非法的```0```，遵循用完立即复原```if(flag == 0) flag = 1```，避免不必要的错误。


## 动态规划

以输入 s = "12120" 为例，递归调用树如下，

```
                                                (s,0)
                                            /            \
                                   (s,1)                      (s,2)
                               /         \                 /       \
                          (s,2)          (s,3)          (s,3)      (s,4)
                          /    \         /              /            |
                    (s,3)     (s,4)  (s,5)           (s,5)          err
                    /           |
                (s,5)          err 
```

可以看到，```numDecodingWays(s,2)```，```numDecodingWays(s,3)```等都被多次调用。递归程序的的比较接近的上限时间复杂度是O(2^n)，对于一个长度为100的输入，最坏情况下需要```2^100```的递归调用。重复计算太多，遂动态规划。
以bottom up manner为例，从右往左处理输入字符串，对于每一步要处理的字符,

- 如果```s[cur] == '0'```，dp[cur] = 0；

- 否则，分两中情况讨论，
    - 如果```(s[cur] - '0')*10 + (s[cur + 1] - '0') <= 26```，
    
        ```dp[cur] = dp[cur + 1] + dp[cur + 2] ```

    - 否则，```dp[cur] = dp[cur + 1]```

程序实现如下，

```
// PASSED 49ms
class Solution {
public:
    int numDecodings(string s) 
    {
        int n = s.length();
        if(n < 2)
            return (n == 1 && s[0] != '0')? 1 : 0;
        vector<int> dp(n);
        dp[n-1] = s[n-1] == '0'? 0 : 1;
        dp[n-2] = s[n-2] == '0'? 0 : dp[n-1] +
                  ((s[n-2] - '0')*10 + (s[n-1] - '0') <= 26 ?
                  1 : 0);
        for(int cur = n-3; cur >= 0; cur--)
        {
            if(s[cur] == '0')
                dp[cur] = 0;
            else
                dp[cur] = dp[cur + 1] +
                          (((s[cur] - '0')*10 + (s[cur + 1] - '0') <= 26) ?
                          dp[cur + 2] : 0);
            cout<<"cur: "<<cur<<" dp: "<<dp[cur]<<endl;
        }
        return dp[0];
    }
};
```