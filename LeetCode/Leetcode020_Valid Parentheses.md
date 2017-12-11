
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

```
class Solution {
public:
    bool isValid(string s) 
    {
        if (s.size() % 2 == 1)
            return false;
        stack<char> mystack;
        for(int i=0;i<s.size();i++)
        {
            if (s[i] == ')' || s[i] == ']' || s[i] == '}')
            {
                if (mystack.empty())
                    return false;
                char ch = mystack.top();
                if  ( s[i] == ')' && ch != '('
                   || s[i] == ']' && ch != '['
                   || s[i] == '}' && ch != '{')
                    return false;
                mystack.pop();
            }
            else
            {
                mystack.push(s[i]);
            }
        }
        if (!mystack.empty())
            return false;
        return true;
    }
};
```