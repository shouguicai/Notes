Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

```
// 1. 邻近的"()"一定是配对的
// 2. 不断删除"()"
// 3. 算法停止时，返回最大连续删除长度
// time :22 ms O(n^2)
// space : O(n)
// ? 可能还不如用堆
class Solution {
public:
    int longestValidParentheses(string s) 
    {
        if (s.size() < 2)
            return 0;
        vector<int> mark(s.size(),0);
        while(existpair(s,mark))
            deletepairs(s,mark);
        return calcumaxlen(mark);
    }

private:
    bool existpair(string s, vector<int>& mark)
    {
        for (int i = 0,j;i < mark.size()-1;)
        {
            for (;i < mark.size()-1 && mark[i] == 1; i++);
            for (j = i+1;j < mark.size() && mark[j] == 1;j++);  
            
            if (s[i] == '(' && s[j] == ')')
                return true;
            i = j;
        }
        return false;
    }

    void deletepairs(string s,vector<int>& mark)
    {
        for (int i = 0,j;i < mark.size()-1;)
        {
            for (;i < mark.size()-1 && mark[i] == 1; i++);
            for (j = i+1;j < mark.size() && mark[j] == 1;j++);  
            if (s[i] == '(' && s[j] == ')')
            {
                mark[i] = 1;
                mark[j] = 1;
            }
            i = j; 
        }
    }

    int calcumaxlen(vector<int>& mark)
    {
        for (int i=0;i<mark.size();i++)
        {
            mark[i] = (mark[i] + mark[i-1])*mark[i]; 
        }
        return *(max_element(mark.begin(),mark.end()));
    }
};
```

```
// 用栈 O(n) space 
//  9 ms
// 思路：用一个栈存多于的右括号以及还没有匹配的左括号的位置
// 输入')'，则找栈顶是不是'('，如果是，则弹出栈顶，剩下的即为上一个没匹配的位置last，i-last即为当前括号表达式的长度
class Solution {
public:
    int longestValidParentheses(string s) 
    {
        int maxlen = 0;
        stack<int> mystack;
        mystack.push(-1); // 
        for (int i = 0 ;i < s.size(); i++)
        {
            int t = mystack.top();
            if (t != -1 && s[t] == '(' && s[i] == ')')
            {
                mystack.pop();  // 弹出对应的'('的位置，剩下的是最后一个不合法的')'的位置
                maxlen = max(maxlen,i-mystack.top()); 
            }
            else
                mystack.push(i);        
        }
        return maxlen;
    }
};
```