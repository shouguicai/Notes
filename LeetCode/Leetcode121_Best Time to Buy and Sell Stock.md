Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.

```
// initial idel
// 两指针 + DP
// O(n) time , O(1) space
/*
  指针i : 寻找最低买入时间
  指针j ：寻找最高卖出时间
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) 
    {
        if(prices.size() < 2)
            return 0;
        int i = 0;
        int j = prices.size() - 1;
        int day_buy = 0;
        int day_sell = j;
        int profit = 0;
        int limit_left ;
        int limit_right;
        while(i <= j)
        {
            profit = max(profit,  
                     max(prices[i] - prices[day_buy], prices[day_sell] - prices[j]));   // 考虑i天卖出和j天买入时的利润
            limit_right = day_sell;
            while(prices[i+1] <= prices[i] && i < limit_right-1) i++;     // 两指针分别从数组两端向中间(i非递增，j非递减)移动寻找局部最低买入时间，最高卖出时间
            if(prices[day_buy] > prices[i] && i < day_sell)               // 停止时，day_buy是0~i天的最低买入时间
                day_buy = i;                                              //         day_sell是j~n天的最高卖出时间
            limit_left = day_buy;
            while(prices[j-1] >= prices[j] && limit_left< j-1 ) j--;
            if(prices[day_sell] < prices[j] && j > day_buy)
                day_sell = j;
            profit = max(max(profit,prices[day_sell] - prices[day_buy]) ,        // 停止时，计算可能的最大利润，a.prices[day_sell] - prices[day_buy]
                          max(prices[i+1] - prices[i],prices[j] - prices[j-1])); //                             b.指针i，j停止处
            limit_left = i+1;
            limit_right = j-1;
            i+=2;j-=2;                                                  // 跳过i递增，j递减部分，在区间i+2~j-2中继续寻找最低买入时间和最高卖出时间，
        }                                                               // 并考虑i天卖出和j天买入时的利润
        return profit;                                                  // limit_right，limit_left用来限制i,j的搜索范围
    }
};
```