Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true

```
// DP
//https://leetcode.com/problems/regular-expression-matching/discuss/5651
class Solution {
public:
    bool isMatch(string s, string p) 
    {
    	if(s.empty() || p.empty())
    	{
    		return false;
    	}
        vector<vector<bool>> state_array(s.length() + 1, vector<bool>(p.length() + 1, false));
    	state_array[0][0] = true;

    	for(int j = 1; j< p.length(); j++)
    	{
    		if(p[j-1] == '*')
    		{
    			if(state_array[0][j-1] || (j > 1 && state_array[0][j-2]))
    			{
    				state_array[0][j] = true;
    			}
    		}	
    	}

        for(int i=1; i<s.length();i++)
        {
        	for(int j=1;j<p.length();j++)
        	{
        		if(s[i-1] == p[j-1] || p[j-1] == '.')
        		{
        			state_array[i][j] = state_array[i-1][j-1];
        		}
        		if(p[j-1] == '*')
        		{
        			if(s[i-1] != p[j-2] && p[j-2] != '.')
        			{
        				state_array[i][j] = state_array[i][j-2];
        			}
        			else
        			{
        				state_array[i][j] =  state_array[i-1][j]
        				                   ||state_array[i][j-1]
        				                   ||state_array[i][j-2];
        			}
        		}
        	}
        }
        return state_array[s.length()][p.length()];
    }
};
```