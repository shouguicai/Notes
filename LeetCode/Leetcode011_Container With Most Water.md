Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

```
// 初始化为两个边界容纳的空间
// 矮的那头往中间推进
class Solution {
public:
    int maxArea(vector<int>& height) 
    {
        int start = 0;
        int end = height.size()-1;
        int ans = min(height[start],height[end])*(end-start);
        while(start<end)
        {
            if (height[start] < height[end])
                start ++;
            else
                end --;
            ans = max(ans,min(height[start],height[end])*(end-start));
        }
        return ans;
    }
};
```