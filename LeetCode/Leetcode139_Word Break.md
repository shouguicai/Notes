---
title: 断字
date: 2018-01-25
mathjax: false
tags: 动态规划
---

问题叙述:给定一个非空字符串s和一个非空词典wordDict，问字符串s能否被分割为字典wordDict中的一个或多个单词。假设字典中没有重复的单词。

Examples:
1. 输入 s = "leetcode" dict = ["leet","code"] 输出 true 
解释： leetcode可以分割为leet和code.

## 递归解

最原始的思路是枚举字符串s的所有分割组合，并检查其中是否存在符合题意条件的分割。

用start_pos标记起始位置，len标记当前选取的单词的长度，并在字典中检查（Hash）单词是否存在，

- 若存在，则递归检查right之后剩余部分字符串；
- 若不存在，则```right++```

一旦发现符合条件的分割组合，```break```并返回 ```true```，否则，遍历完所有组合后，返回```false```。

```特别地```，需要注意字典中的单词存在包含关系的情况，比如```["aaa","aaaa"]```

```
//  Time Limit Exceeded
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict)
    {
        unordered_set<string> dict;
        for(int i = 0;i < wordDict.size(); i++)
            dict.insert(wordDict[i]);
        return check(s,0,dict);

    }
private:
    bool check(string s, int start_pos, unordered_set<string> dict)
    {
        if(start_pos >= s.length())
            return true;
        for(int len = 1; len <= s.length() - start_pos; len++)
        {
            if(dict.find(s.substr(start_pos,len)) != dict.end() 
               && check(s,start_pos + len,dict))
                return true;
        }
        return false;
    }
};
```

最坏时间复杂度是O(n!)，n是输入字符串的长度。需要检查完$n!$个组合才能确定不存在。

## 动态规划

显然存在重复计算的子问题，对于一个```false```的测例，或者```true```组合出现较晚的例子，
重复计算会很多。

Bottom up manner的思路，从右往左处理字符串，用一个大小等于字符串长度的数组```dp[]```，存储子问题的解。

```dp[i]```表示s的子串s[i:end]是否符合条件。对于每个待处理的字符，

- 若存在```j, i < j < n```，```dp[j] && dict.finded(s[i:j]) == true```,
    
    ```dp[i] = true;```

- 否则，

    ```dp[i] = fasle;```

最后返回```dp[0]```，算法最坏时间复杂度是O(n^2)，代码如下，

```
// 10ms
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict)
    {
        int n = s.length();
        unordered_set<string> dict;
        vector<bool> dp(n);
        for(int i = 0;i < wordDict.size(); i++)
            dict.insert(wordDict[i]);
        dp[n-1] = (dict.find(s.substr(n-1,1)) != dict.end());
        for(int i = n-2; i >= 0; i--)
            dp[i] = checkgo(s,i,dict,dp);
        return dp[0];

    }
private:
    bool checkgo(string s, int i, unordered_set<string> dict, vector<bool> dp)
    {
        for(int j = i; j < s.length()-1;j++)
            if(dp[j + 1] && dict.find(s.substr(i,j-i+1)) != dict.end())
                return true;
        return dict.find(s.substr(i,s.length()-i+1)) != dict.end();
    }  
};
```

