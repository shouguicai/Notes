Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

Note: The length of path between two nodes is represented by the number of edges between them.

Example 1:

Input:

              5
             / \
            4   5
           / \   \
          1   1   5
Output:

2
Example 2:

Input:

              1
             / \
            4   5
           / \   \
          4   4   5
Output:

2
Note: The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.

该题求的是最长相同值路径长度，路径可以不经过root
宽度优先+深度优先，类似于No.437
```
// 93ms
// 自己的方法好像比较复杂且慢，看下Discusion的方法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
/** 算法思路：
*   1.宽度优先遍历树,计算以每个节点为根的子树的UnivaluePath，最后取最大值。
*   2.每个根子树UnivaluePath = 左子树值相同的连续节点数 + 右子树值相同的连续节点数
*   3.值相同的连续节点数 = 1 + max (左子树值相同的连续节点数,右子树值相同的连续节点数)
*   
*/
class Solution {
public:
    int longestUnivaluePath(TreeNode* root) 
    {
        if(root == NULL)
          return 0;
        queue<TreeNode*> myqueue;
        TreeNode* p;
        int length = 0;
        myqueue.push(root);    // 队列完成宽度优先遍历树
        while(!myqueue.empty())
        {
          p = myqueue.front();
          myqueue.pop();       // 计算以p为根的子树的UnivaluePath
          length = max(length, // // 左右两侧相加
                             lenghtsameValuePath(p -> left,p -> val)  
                             + lenghtsameValuePath(p -> right,p -> val));

          if(p -> left != NULL)
            myqueue.push(p -> left);
          if(p -> right != NULL)
            myqueue.push(p -> right);
        }
        return length;
    }
private:
    int lenghtsameValuePath(TreeNode* root, int val)
    {
      if(root == NULL || root -> val != val)
        return 0;
      else
        return 1 + max (lenghtsameValuePath(root -> left, val)   // 取左右两边大的那侧
                        ,lenghtsameValuePath(root -> right,val));
    }
};
```