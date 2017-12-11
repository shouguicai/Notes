Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]

```
// 递归求解
// left,right 分别记录需要被添加的左括号和右括号数量
// left > 0 --> 插入一个左括号，left-1,同时right需要+1
// right >0 --> 插入一个右括号
// 递归停止的条件时 left == 0 and right == 0
class Solution {
public:
    vector<string> generateParenthesis(int n) 
    {
        vector<string> ans;
        addingpar(ans,"",n,0);
        return ans;
    }
private:
    // ans 要传引用
    void addingpar(vector<string>& ans, string str, int left, int right)
    {
        if (left == 0 && right ==0)
        {
            ans.push_back(str);
            return ;
        }
        if (left >0) addingpar(ans,str+"(",left-1,right+1);
        if (right>0) addingpar(ans,str+")",left,right-1);
    }
};
```