Given an integer, write a function to determine if it is a power of two.

Approach #1
```
class Solution {
public:
    bool isPowerOfTwo(int n) 
    {
        int temp;
        if(n == 0)
            return false;
        while(n)
        {
            if(n == 1)
                return true;
            temp = n / 2 ; // 除以2
            if(temp*2 != n)
                return false;
            n = n / 2;
        }
    }
};
```

Approach #2
```
class Solution {
public:
    bool isPowerOfTwo(int n) 
    {
        unsigned int num = 0;
        while(n)
        {
            num +=  (n & 0x01);
            n >>= 1;
        }
        return num == 1;
    }
};
```