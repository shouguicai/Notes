Write a function to find the longest common prefix string amongst an array of strings.

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) 
    {
        if (strs.size() == 0)
            return "";
        int minLen = strs[0].size();
        for(int i=1;i<strs.size();i++)
            minLen = min(minLen,(int)strs[i].size());
        int low = 1;
        int high = minLen;
        while(low <= high)
        {
            int middle = (low + high)/2;
            if(isCommonPrefix(strs,middle))
                low = middle + 1;
            else 
                high = middle -1;
        }
        return strs[0].substr(0,(low + high)/2);
    }
private:
    bool isCommonPrefix(vector<string>& strs, int len)
    {
        string str1 = strs[0].substr(0,len);
        for(int i=0;i<strs.size();i++)
        {
            if (strs[i].compare(0,len,str1) != 0) //   0 -- they compare equal
                return false;
        }  
        return true; 
    }
};
```