Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

```
// 这题要求检查输入合法性并做溢出处理
// 遇到非数字字符，则停止，直接返回到现在为止的数
class Solution {
public:
    int myAtoi(string str) 
    {
        int ans = 0;
        int sign = 1;
        int flag_overflow = 0;
        string str1 = skipspace(str);
        for (int i=0;i<str1.size();i++)
        {
            if (!isValid(str1[i],i) || flag_overflow)
                   break;
            update(str1[i],i,ans,sign,flag_overflow);
        }
        return ans*sign;
    }
private:
    bool isValid(char ch,int i)
    {
        if (i == 0)
            return ('0' <= ch && ch<= '9' || ch == '+' || ch == '-'); 
        else
            return ('0' <= ch && ch<= '9'); 
    }

    int flag(char ch)
    {
        int sign = 1;
        if (ch == '-')
            sign = -1;
        return sign;
    }

    void update(char ch,int i,int & ans,int & sign,int &flag_overflow)
    {
        int temp;
        if (i == 0 && (ch == '+' || ch == '-'))
            sign = flag(ch);       
        else
        {
            temp = ans*10 + ch - '0';
            flag_overflow = isoverflow(temp,ans);
            if(!flag_overflow)
                ans = temp;
            else
                ans = (sign == -1 ? 2147483648 : 2147483647);
        }
    }

    bool isoverflow(int temp,int ans)
    {
       return (temp/10 != ans);
    }
    string skipspace(string str)
    {
        int i = 0;
        while(str[i]==' '||str[i] == '0')
        {
            i ++;
        }
        return &str[i];
    }
};
```