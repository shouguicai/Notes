//Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

```
// 3 ms
class Solution {
public:
    vector<vector<int>> generate(int numRows) 
    {
        vector<vector<int>> ans;
        vector<vector<int>>::iterator it;
        vector<int>::iterator inner_it;
        int line = 1;
        while(line <= numRows)
        {
            vector<int> temp(line,1);
            for(int i = 1; i < line - 1; i++)
                temp[i] = *(inner_it + i - 1) + *(inner_it + i);
            ans.push_back(temp);
            it = ans.begin();
            inner_it = (it + line -1)->begin();
            line ++;
        }
        return ans;
    }
};
```

```
//  3 ms
class Solution {
public:
    vector<vector<int>> generate(int numRows) 
    {
        vector<vector<int>> ans;
        int line = 1;
        while(line <= numRows)
        {
            ans.push_back(vector<int> (line,1));
            for(int i = 1; i < line - 1; i++)
                ans[line - 1][i] = ans[line - 2][i -1] + ans[line - 2][i];
            line ++;
        }
        return ans;
    }
};
```


