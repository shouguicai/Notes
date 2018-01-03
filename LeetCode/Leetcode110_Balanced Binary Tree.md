Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.


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
    bool isBalanced(TreeNode* root) 
    {
      if(root == NULL)
        return true;
      return (NodeBalanced(root) && isBalanced(root->left) && isBalanced(root->right));
    }
private: 
    bool NodeBalanced(TreeNode* root)
    {
      if(root == NULL)
          return true;
      return (abs(Depth(root->left) - Depth(root->right)) <=1);
    }

    int Depth(TreeNode* t)
    {
      if(t == NULL)
        return 0;
      return 1+ max(Depth(t->left),Depth(t->right));
    }
};
```