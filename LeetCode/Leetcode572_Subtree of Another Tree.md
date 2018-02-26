//Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

Example 1:
Given tree s:

     3
    / \
   4   5
  / \
 1   2
Given tree t:
   4 
  / \
 1   2
Return true, because t has the same structure and node values with a subtree of s.
Example 2:
Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0
Given tree t:
   4
  / \
 1   2
Return false.

树A（根为root）中包含树B的三种情况，

1. 以root为根的一个子树和树B完全相同，
2. root的左子树包含树B，
3. 或者，root的右子树包含树B。

两根树相同的判断

1. 两棵树都是空树，
2. 根节点值相同，并且左右子树对应为相同树，
3. 否则，两棵树不同。

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
    bool isSubtree(TreeNode* s, TreeNode* t) 
    {  
        return isSame(s,t) || (s != NULL && (isSubtree(s -> left,t) || isSubtree(s -> right,t)));
    }
private:
    bool isSame(TreeNode* s, TreeNode* t)
    {
        if(s == NULL && t == NULL)
          return true;
        if(s != NULL && t != NULL && t -> val == s -> val)
          return isSame(s -> left,t -> left) && isSame(s -> right,t -> right);
        return false;
    }
};
```