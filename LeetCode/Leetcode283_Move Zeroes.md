//Given an array nums, write a function to move all 0 's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

Approach #1: 
从前往后扫描并将0与后面找到第一个非零值交换
```
//  27 ms
class Solution {
public:
    void moveZeroes(vector<int>& nums) 
    {
        int n = nums.size();
        int finish = 0;
        for(int i = 0;i < n && finish == 0;i++)
        {
            if(nums[i] == 0)
            {
                SweepToEnd(nums,i,n,finish);
            }
        }
    }
private:
    void SweepToEnd(vector<int>& nums,int start, int end, int & finish)
    {   // 找到第一个非零值与nums[start] 交换
        int j=start+1;
        for(;j<end && nums[j] == 0;j++);
        if(j == end)  // 如果start之后都是零，说明Move Zeros结束
        {
            finish = 1;
            return;
        }
        int temp = nums[j];
        nums[j] = nums[start];
        nums[start] = temp;
    }
};
```


Approach #2: 类似于冒泡，在Approach #1的基础上，
将扫描到的0不断与后面第一个非零值交换，
之后这个0被交换到数组尾部
```
// 32 ms
// 不科学！居然比Approach #1慢
class Solution {
public:
    void moveZeroes(vector<int>& nums) 
    {
        int n = nums.size();
        int finish = 0;
        for(int i = 0;i < n && finish == 0;i++)
        {
            if(nums[i] == 0)
            {
                BubbSweepToEnd(nums,i,n--,finish);
            }
        }
    }
private:
    void BubbSweepToEnd(vector<int>& nums,int start, int end, int & finish)
    {   // 将找到的0元交换到数组尾部
        int pos = start;
        int j = pos + 1;
        for(; j < end; j++)
        {
            if(nums[j] != 0)
            {
                int temp = nums[pos];
                nums[pos] = nums[j];
                nums[j] = 0;
                pos = j;
            }
        }
        if(pos == start)
            finish = 1;
    }
};
```