Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

Input: 123
Output:  321
Example 2:

Input: -123
Output: -321
Example 3:

Input: 120
Output: 21

```
class Solution {
public:
    int reverse(int x) 
    {
        int result = 0;
        int temp = 0;
        do
        {
            temp = temp*10 + x%10;
            if (temp/10!=result)
                return 0;
            result = temp;
            x = x/10;
        }while(x!=0);

        return result;
    }
};
```
