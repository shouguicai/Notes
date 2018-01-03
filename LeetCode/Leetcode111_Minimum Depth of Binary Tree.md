Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.


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
// 需要注意左右子树一颗空一颗非空的情况
class Solution {
public:
    int minDepth(TreeNode* root) 
    {
        return Depth(root);   
    }
private:
    int Depth(TreeNode* t)
    {
       if(t == NULL)
          return 0;
       if((t->left != NULL) != (t->right != NULL))  // t的左右子树一颗空一颗非空的情况
          return 1 + max(Depth(t->left),Depth(t->right));
       return 1 + min(Depth(t->left),Depth(t->right));
    }
};
```