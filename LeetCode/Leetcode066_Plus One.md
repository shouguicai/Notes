Given a non-negative integer represented as a non-empty array of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.


```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) 
    {
        int flag = 1;
        int i = digits.size() - 1;
        while(flag && i >= 0)
        {
            flag = (digits[i] + 1)/10;
            digits[i] = (digits[i] + 1)%10;
            i --;
        }
        if(flag == 1)
            digits.insert(digits.begin(),1);
        return digits;
    }
};
```