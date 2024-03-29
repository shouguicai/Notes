Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

/* 百度百科
罗马数字是阿拉伯数字传入之前使用的一种数码。罗马数字采用七个罗马字母作数字、即Ⅰ（1）、X（10）、C（100）、M（1000）、V（5）、L（50）、D（500）。
记数的方法：
1.相同的数字连写，所表示的数等于这些数字相加得到的数，如 Ⅲ=3；
2.小的数字在大的数字的右边，所表示的数等于这些数字相加得到的数，如 Ⅷ=8、Ⅻ=12；
3.小的数字（限于 Ⅰ、X 和 C）在大的数字的左边，所表示的数等于大数减小数得到的数，如 Ⅳ=4、Ⅸ=9；
4.在一个数的上面画一条横线，表示这个数增值 1,000 倍
*/

```
class Solution {
public:
    string intToRoman(int num) 
    {
        string Roman[4][10] = {{"", "M", "MM", "MMM"}, // 1000,2000,3000
                               {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"}, // 100，200，300，...,900
                               {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"}, // 10,20,...,90
                               {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"}}; //1,2,...,9
        int digit[] = {1000,100,10,1};
        string ans;
        int i = 0;
        while (num!=0)
        {
            if (num >= digit[i])
            {
                ans.insert(ans.size(),Roman[i][num/digit[i]]);
                num %= digit[i];
            }
            else
             {
                 i ++;
             }
        }
        return ans;
    }
};
```