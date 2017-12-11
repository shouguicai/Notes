Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 
Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

```
//只能得到局部解 结果不对
class Solution {
public:
    int trap(vector<int>& height) 
    {
        int left,right,sum=0;
        //寻找合适点
        for (int i = 1;i < height.size() - 1;)
        {
            if (isLegalMiddle(height,i))
            {
                left = i - 1;
                right = i + 1;
                // 向两边扩展
                ExpBounds(height,left,right);
                // 计算积水面积
                sum += CaluTrap(height,left,right);
                i = right + 1;
            }
            else
                i++;
        }
        return sum;
    }

private:
    bool isLegalMiddle(vector<int>& height, int middle)
    {
        return (height(middle-1) > height[middle] && height[middle+1] > height[middle]);
    } 

    void ExpBounds(vector<int>& height, int& left, int& right)
    {
        if (left > 0 && height[left - 1] > height[left])
            left = left -1;
        if (right < height.size() -1 && height[right + 1] > height[right])
            right = right +1;
        //调整两侧
        int min = min(height[left],height[right]);
        while(height[left + 1] >= min) left ++;
        while(height[right - 1] >= min) right --;
    }

    int CaluTrap(vector<int>& height, int left, int right)
    {
        int area = min(height[left],height[right])*(right - left -1);
        for (int i = left + 1; i < right ; i ++)
            area -= height[i];
        return area; 
    }
};
```

```
// 答案
class Solution {
public:
    int trap(vector<int>& height)
    {
        int left = 0, right = height.size() - 1;
        int ans = 0;
        int left_max = 0, right_max = 0;
        while (left < right)
        {
            if (height[left] < height[right])
            {
              height[left] >= left_max ? (left_max = height[left]) : ans += (left_max - height[left]);
              ++left;
             }
            else 
            {
                height[right] >= right_max ? (right_max = height[right]) : ans += (right_max - height[right]);
                --right;
            }
        }
    return ans;
    }
};
```