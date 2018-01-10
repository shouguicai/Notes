On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

Example 1:
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
Example 2:
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
Note:
cost will have a length in the range [2, 1000].
Every cost[i] will be an integer in the range [0, 999].

 
递归  Runtime Error 
nextStep = minCostToEnd[curStep+1] + cost[curStep+1] 
        >= minCostToEnd[curStep+2] + cost[curStep+2] 
        ? curStep+2 : curStep+1;
```
// 重复计算太多
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) 
    {
        int curStep = -1;
        int sumCost = 0;
        // vector.size() 是 unsigned int，需要先转成signed int 再比较
        while(curStep < (int)cost.size()-1)
        {
        	curStep = minCostStep(cost,curStep);
        	sumCost += cost[curStep];
        }
        return sumCost;
    }
private:
	int minCostStep(vector<int>& cost, int curStep)
	{
        int nextStep;
		if (curStep == cost.size()-2)
			nextStep = cost.size() - 1;
		else
			nextStep = minCostToEnd(cost,curStep+1) + cost[curStep+1] 
        			>= minCostToEnd(cost,curStep+2) + cost[curStep+2] 
        			? curStep+2 : curStep+1;
		return nextStep;
	}
	int minCostToEnd(vector<int>& cost, int Step)
	{
		int nextStep = minCostStep(cost,Step);
		int CostToEnd = cost[nextStep] + minCostToEnd(cost,nextStep);
		return CostToEnd;
	}
};
```

DP PASSED
```
/* minCostToEnd[i], 从i到end的最小cost
// minCostToEnd[i] = min( minCostToEnd[i+1] + cost[i+1] ,
                          minCostToEnd[i+2] + cost[i+2] );
*/
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) 
    {
    	int n = cost.size();
    	vector<int> minCostToEnd(n);
    	minCostToEnd[n-1] = 0;
    	minCostToEnd[n-2] = 0;
    	for(int i = n-3; i >= 0;i--)
    	{
    		minCostToEnd[i] = min(cost[i+1]+minCostToEnd[i+1],cost[i+2]+minCostToEnd[i+2]);
    	}
    	return min(cost[0]+minCostToEnd[0],cost[1]+minCostToEnd[1]);
    }
};
```
