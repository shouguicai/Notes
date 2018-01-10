nvert a binary tree.
     4
   /   \
  2     7
 / \   / \
1   3 6   9
to
     4
   /   \
  7     2
 / \   / \
9   6 3   1


Approach  #1 递归解 类似No.101 Symmetric Tree
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
/**
*  翻转一棵树，即把左子树和右子树关于轴做镜像对称；
*  先左右子树各自翻转，然后交换左右子树。
*/
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) 
    {
      if(root != NULL)
      {
        TreeNode* tmp;
        tmp = invertTree(root -> left);
        root -> left = invertTree(root -> right);
        root -> right = tmp;
      }
      return root;
    }
};
```