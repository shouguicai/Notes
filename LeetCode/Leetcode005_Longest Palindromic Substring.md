Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:

Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
Example:

Input: "cbbd"

Output: "bb"

```
// 时间复杂度O(n^2)，朴素的思路，以某个位置为中心左右扩展，直到不相等为止
class Solution {
public:
    string longestPalindrome(string s) 
    {
        if(s.size()==1)
            return s;
        int len = 1,start,end;
        for(int i=0;i<s.size();i++)
        {
            int len1 = expandAroundCenter(s,i,i); //  字母为中心
            int len2 = expandAroundCenter(s,i,i+1);// 空格为中心
            int max_len = max(len1,len2);
            if(max_len>len)
            {
                len = max_len;
                start = i-(len-1)/2;
                end = i+len/2;
            }
        }
        return s.substr(start,len);
    }
private:
    int expandAroundCenter(string s,int left, int right)
    {
        int pos_l = left; 
        int pos_r = right;
        while(pos_l >=0 && pos_r < s.size() && s[pos_l] == s[pos_r])
        {
            pos_l --;
            pos_r ++;
        }
        return pos_r - pos_l -1;
    }
};
```