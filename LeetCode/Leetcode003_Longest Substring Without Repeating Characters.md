Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

```
// 用start,pos指示位置，加上got位置判断
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int start = 0;
        int pos = 0;
        int max = 0;
        unordered_map<char, int> hash;
        while(pos < s.size())
        {
            char element = s[pos];
            unordered_map<char, int>::iterator got = hash.find(element);
            if (got!= hash.end()&&start<=(*got).second&&(*got).second<=pos) // got位置判断
            {
                if ((pos-start)>max)
                {
                    max = pos-start;
                }
                start = hash[element]+1;
            }
            hash[element] = pos;
            pos ++;
        } 
        if ((pos-start)>max)
        {
            max = pos-start;
        }
        return max;
    }
};
```