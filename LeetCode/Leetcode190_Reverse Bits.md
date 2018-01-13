Reverse bits of a given 32 bits unsigned integer.

For example, given input 43261596 (represented in binary as 00000010100101000001111010011100), return 964176192 (represented in binary as 00111001011110000010100101000000).

Follow up:
If this function is called many times, how would you optimize it?

```
class Solution {
public:
    uint32_t reverseBits(uint32_t n) 
    {
        unsigned int num = 0;
        int count = 32;
        while(count--)
        {
            num = num * 2 + (n & 0x01);
            n >>= 1;
        }
        return num;
    }
};
```