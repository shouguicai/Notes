Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

递归解
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
class Solution {
public:
    int maxDepth(TreeNode* root) 
    {
        return Depth(root);
    }
private:
    int Depth(TreeNode* t)
    {
       if(t == NULL)
          return 0;
       return 1 + max(Depth(t->left),Depth(t->right));
    }
};
```