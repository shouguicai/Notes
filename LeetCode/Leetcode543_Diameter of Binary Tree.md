Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example:
Given a binary tree 
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

Note: The length of path between two nodes is represented by the number of edges between them.

Approach #1 :宽度优先 + 递归求树高
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
private:
    int diameter = 0;
public:
    int diameterOfBinaryTree(TreeNode* root) 
    {
        if(root == NULL)
            return 0;
         queue<TreeNode*> myqueue;
         TreeNode* p;
         myqueue.push(root);
         while(!myqueue.empty())
         {
            p = myqueue.front();
            myqueue.pop();
            diameter = max(diameter,Height(p -> left) + Height(p -> right));
            if(p -> left != NULL)
                myqueue.push(p -> left);
            if(p -> right != NULL)
                myqueue.push(p -> right);
         }
        return diameter; 
    }
private:
    int Height(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        return 1 + max(Height(root -> left),Height(root -> right));
    }
};
```

Approach #2 :Approach #1用宽度优先的目的是遍历每个节点；
事实上，Height求树高的过程中已经包含对节点的遍历，遍历和求高可以合并。

```
class Solution {
private:
    int diameter;
public:
    int diameterOfBinaryTree(TreeNode* root) 
    {
        diameter = 0;
        Height(root);
        return diameter; 
    }
private:
    int Height(TreeNode* root)
    {
        if(root == NULL)
            return 0;
        int L = Height(root -> left);
        int R = Height(root -> right);
        diameter = max(diameter,L+R);
        return 1 + max(L,R);
    }
};
```