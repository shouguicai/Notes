Given an array of integers, every element appears three times except for one, which appears exactly once. Find that single one. 

统计所有数对应二进制位上1的总数，如果某些数出现了k次，那么这些数对应二进制为1的位上
1的出现次数为k的整数倍。

[牛客网](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```
class Solution {
public:
    int singleNumber(vector<int>& nums) 
    {
        vector<int> bits(32,0);              
        int n = nums.size();   
        int ans = 0;      
        for(int i = 0; i < n; i++)      
            for(int j = 0; j < 32; j++)              
                bits[j] += ((nums[i]>>j) & 1);                       

        for(int i = 0; i < 32; i++)        
            if(bits[i] % k != 0) // k次         
                ans |= (1 << i);    
        return ans;                
    }        
};
```