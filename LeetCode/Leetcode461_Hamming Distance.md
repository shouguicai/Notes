The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

Note:
0 ≤ x, y < 231.

Example:

Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.

```
// 按位异或
// 然后计算结果中1的个数
class Solution {
public:
    int hammingDistance(int x, int y) 
    {
        return count(x^y);
    }
private:
    int count(int v)
    {
        unsigned int num = 0;
    
        while(v)
        {
            num += v & 0x01;
            v >>= 1;
        }
        return num;
    }
};
```