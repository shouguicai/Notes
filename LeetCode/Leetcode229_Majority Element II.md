Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times. The algorithm should run in linear time and in O(1) space.

思路：摩尔投票法

1. 出现超过1/k的数最多只有k-11个，而且不一定存在，
2. 每次从序列里“隐含地”删除掉k个不相同的数字，最后剩下的则可能是出现超过1/k的数，
3. 统计最后剩下的数在序列中出现的次数，是否大于n/k。

```
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) 
    {
        int major_m,major_n;
        int cnt_m = 0,cnt_n = 0;
        int n = nums.size();
        vector<int> ans;
        for(int i = 0;i < n;++i)
        {
            if(nums[i] == major_m) //  nums[i] == major_m / major_n的判断要放在cnt_m/cnt_n == 0之前
                ++cnt_m;            // 否则不能保证major_m，major_n是不同的数字
            else if(nums[i] == major_n)// 例如输入为[8 8 7 7 7]
                ++cnt_n;
            else if(cnt_m == 0)
            {
                major_m = nums[i];
                ++cnt_m;
            }
            else if(cnt_n == 0)
            {
                major_n = nums[i];
                ++cnt_n;
            }
            else
            {
                --cnt_m;
                --cnt_n;
            }
            cout<<nums[i]<<" "<<major_m<<" "<<major_n<<" "<<cnt_m<<" "<<cnt_n<<endl;
        }
        cnt_m = 0;
        cnt_n = 0;
        for(int i = 0;i < n;++i)
        {
            if(nums[i] == major_m)
                ++cnt_m;
            else if(nums[i] == major_n)
                ++cnt_n;
        }
        if(cnt_m > n/3) ans.push_back(major_m);
        if(cnt_n > n/3) ans.push_back(major_n);
        return ans;
    }
};
```