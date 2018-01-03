Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character

```
// 动态规划
// ref: https://web.stanford.edu/class/cs124/lec/med.pdf
class Solution {
public:
    int minDistance(string word1, string word2) 
    {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> Dis(m+1,vector<int>(n+1));
        for(int i = 0; i <= m; i++)
            Dis[i][0] = i;
        for(int j =1; j <= n; j++)
            Dis[0][j] = j;
        for(int i = 1; i <= m; i++)
            for(int j = 1; j <= n; j++)
                Dis[i][j] = min(min(Dis[i-1][j],Dis[i][j-1])+1,
                                Dis[i-1][j-1] + (word1[i-1] == word2[j-1]? 0 : 1));
        return Dis[m][n];
    }
};
```