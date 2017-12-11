Determine whether an integer is a palindrome. Do this without extra space.

```
class Solution {
public:
    bool isPalindrome(int x) 
    {
        if (x<0)
            return false;
        int result = 0;
        int num = x;
        int temp = 0;
        do
        {
            temp = temp*10 + num%10;
            if (temp/10!=result)
                return 0;
            result = temp;
            num = num/10;
        }while(num!=0);
        return (result == x);
    }
};
```