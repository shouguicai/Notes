You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11

数的遍历+DFS
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
// 25 ms
class Solution {
public:
    int pathSum(TreeNode* root, int sum) 
    {
        int total_count = 0;
        TreeNode* p;
        queue<TreeNode*> myqueue;
        myqueue.push(root);
        while(!myqueue.empty())  // 层序遍历树
        {
            p = myqueue.front();
            if(p != NULL)
            {
                myqueue.push(p -> left);
                myqueue.push(p -> right);
                total_count += SinglepathSum(p,sum);  // 以某一个节点为根，向下DFS
            }
            myqueue.pop();
        }
        return total_count;
    }
private:
    int SinglepathSum(TreeNode* root, int sum) 
    {  
        if (root == NULL)
            return 0;
        int count = 0;  // count是当前节点为根得到的解
        combinationSum(root,sum,count); 
        return count;
    }

    void combinationSum(TreeNode* root, int sum, int & count)
    {   // DFS，直到遍历到叶子节点停止，统计一路上符合条件的解数
        if(root == NULL)
            return;
        if(sum == root -> val)  // 要注意的是，找到一个解后不应该就停止
            count ++;           // 因为后面的路径上可能还存在和为0的路径
        combinationSum(root -> left, sum - root -> val, count);
        combinationSum(root -> right, sum - root -> val, count);
    }
};
```
