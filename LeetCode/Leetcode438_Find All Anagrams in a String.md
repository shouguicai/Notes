//Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

Example 1:

Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
Example 2:

Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

找相同字母异序词
```
/** 两指针(滑窗) + hash
*   Ref:
*    1. https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92015/
*    2. https://discuss.leetcode.com/topic/30941/here-is-a-10-line-template-that-can-solve-most-substring-problems?page=1
*/
class Solution {
public:
    vector<int> findAnagrams(string s, string p) 
    {
        vector<int> map(128,0); // 因为小写字符最大'z'ASCII码为122
        vector<int> ans;     // 存储找到的位置
        int count = p.length(); int left = 0; int right = 0;
        for(int i = 0; i< p.length(); i++) map[p[i]]++; //初始化map，这里用vector就够了，而且比unordered_map快很多
                                  //初始化后，map[c:p]为c在p中的次数(大于0的整数)
        while(right < s.length())
        {                                      // 后--，先执行其他操作，后做自减
            if(map[s[right++]]-- >= 1) count--; // 如果当前字符s[right]在p中存在，count-1
                                               // s中的每个字符在map中的统计数-1
                                               // 如果当前字符在p中不存在，count将是负的
            if(count == 0)  // count == 0 说明找到了
            {
                ans.push_back(left);
            }

            if(right - left == p.length() && map[s[left++]]++ >= 0) count++;
            // 如果没找到，移动窗口，重新开始寻找
            /** 这句话等价为：
            *   if(right - left == p.length()) // 没找到
            *   {
            *       if(map[s[left]] >= 0)     // map[s[left]]非负，说明s[left]在p中存在，
            *           count++;              // 那么count需要+1，补上count在上面的程序中因为s[left]执行的--
            *       map[s[left]]++;           // ++将map[s[left]]重置为之前的值
            *       left ++;                  // left++ 移动窗口
            *    }
            */
        }
        return ans;
    }
};
```

```
/** Minimum Window Substring template 
* int findSubstring(string s){
*        vector<int> map(128,0);
*        int counter; // check whether the substring is valid
*        int begin=0, end=0; //two pointers, one point to tail and one  head
*        int d; //the length of substring
*
*        for() { initialize the hash map here }
*
*        while(end<s.size()){
*
*            if(map[s[end++]]-- ?){  modify counter here }
*
*            while(counter condition ){ 
*                 
*                  update d here if finding minimum
*
*                increase begin to make it invalid/valid again
*                
*                if(map[s[begin++]]++ ?){ modify counter here}
*            }  
*
*            update d here if finding maximum
*        }
*        return d;
*  }
*   
*
*
*/
```