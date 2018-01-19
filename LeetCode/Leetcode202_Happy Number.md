Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

```
// 3ms
class Solution {
public:
    bool isHappy(int n) 
    {
        unordered_set<int> hasOccured;
        int integer = n;
        while(integer != 1)
        {
            integer = digitSquareSum(integer);
            if(hasOccured.find(integer) != hasOccured.end())
                return false;
            else
                hasOccured.insert(integer);
        }
        return true;
    }
private:
    int digitSquareSum(int num)
    {
        int sum = 0;
        while(num != 0)
        {
            sum += pow(num % 10,2);
            num /= 10;
        }
        return sum;
    }
};
```