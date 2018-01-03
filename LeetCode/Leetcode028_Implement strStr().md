Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

Input: haystack = "hello", needle = "ll"
Output: 2
Example 2:

Input: haystack = "aaaaa", needle = "bba"
Output: -1

```
class Solution {
public:
    int strStr(string haystack, string needle) 
    {
        if(needle.empty())
            return 0;
        for(int i=0;i<haystack.length();)
        {
            if(haystack[i]==needle[0])
            {
                int j=1;
                for(;haystack[i+j] == needle[j] && j<needle.length();j++);
                if(j == needle.length())
                    return i;
            }
            i++;
        }
        return -1;
    }
};
```