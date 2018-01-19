//Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

```
// 3 ms Beat 0.55%
class Solution {
public:
    vector<int> getRow(int rowIndex) 
    {
        vector<int> ans(1,1);
        for(int line = 1; line <= rowIndex; line++)
        {
            for(int i = 0;i < ans.size() -1; i++)
                ans[i] += ans[i + 1];
            ans.insert(ans.begin(),1);
        }
        return ans;
    }
};
```