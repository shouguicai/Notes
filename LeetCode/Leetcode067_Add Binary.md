Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100".


```
class Solution {
public:
    string addBinary(string a, string b) 
    {
        int flag = 0;
        int n_a = a.length();
        int n_b = b.length();
        int k = max(n_a,n_b);
        char c[k+2];
        c[0] = '1'; c[k+1] = '\0';
        while(n_a > 0 || n_b > 0)
        {
            int num_a = n_a > 0 ? a[n_a-- -1] - '0' : 0;
            int num_b = n_b > 0 ? b[n_b-- -1] - '0' : 0;
            c[k--] = (num_a + num_b + flag) % 2 + '0';
            flag = (num_a + num_b + flag) / 2;
        }
        if(flag == 1)
            return &c[0];
        else
            return &c[1];
    }
};
```
