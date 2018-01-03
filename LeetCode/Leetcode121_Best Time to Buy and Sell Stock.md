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
// my initial idel : 6ms
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

Approach #2 (One Pass) 
很巧妙的思路,更新最低买入价格的同时更新最大利润
```
// 6 ms
class Solution{
public:
     int maxProfit(vector<int>& prices) 
      {
        int minprice = INT_MAX;
        int maxprofit = 0;
        for (int i = 0; i < prices.size(); i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
};
```

Approach #3 (转换为No.53 Maximum Subarray问题) 

假想每天都以当天的价格进行卖出买入(第一天只有买入，最后一天只有卖出)；
第k天，卖出价与前一天的买入价之差就是当天的利润 profit_k = sell_k- buy_{k-1}，这样问题就成为了寻找最大和的相邻子序列问题；
因为当天的卖出与买入价时相同的，相邻子序列i~j累加相消\sum_{k=i}^{j} profit_k = \sum_{k=i+1}^{j} sell_k- buy_{k-1} = sell_j- buy_{k},
等于j天卖出，i天买入的利润。

算法分为两部
- 计算原数组相邻元素的差
```
   profit[i] = prices[i] - prices[i-1] (define: profit[0] = 0)
```
- 同No.53寻找Maximum Subarray
```
    dp(profit, i) = profit[i] + dp(profit, i - 1) > 0 ? dp(profit, i - 1) : 0 ; 
    maxm = max(maxm,dp[i]);
```

两步可以共用一个for循环，
```
// 13ms
// maxCur = current maximum value
// maxSoFar = maximum value found so far
class Solution {
public:
    int maxProfit(vector<int>& prices) 
    {
        int maxCur  = 0; // 相当于dp[i]
        int maxSoFar  = 0; // 相当于maxm
        for(int i=1;i<prices.size();i++)
        {
            maxCur = max(0,maxCur) + prices[i] - prices[i-1];
            maxSoFar = max(maxSoFar,maxCur);
        }
        return maxSoFar;
    }
};
```