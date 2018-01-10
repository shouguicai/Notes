Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

// dfs 递归
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
// 9ms
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) 
    {
        if(root == NULL) // 空树返回FALSE
            return false;
        if(root -> left == NULL && root -> right == NULL) // 到达叶子节点处
        {                                                 // 判断是都满足条件
            if(sum == root -> val)
                return true;
            else
                return false;
        }
        return hasPathSum(root -> left,sum - root -> val)
                ||hasPathSum(root -> right,sum - root -> val);
    }
};
```

// 非递归(比递归慢很多)，栈
```
// 113 / 114
// Time Limit Exceeded
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) 
    {
        if(root == NULL)            // 空树返回FALSE
            return false;
        stack<TreeNode*> mystack;  // DFS
        set<TreeNode*> myset;     //记录访问过的结点
        TreeNode* p;
        mystack.push(root);
        while(!mystack.empty())
        {
            p = mystack.top();
            if(!isVisited(p,myset))
            {
                sum -= p -> val;
                myset.insert(p);
            }       
            if(isLeafNode(p) && sum == 0)
                return true;
            if(     (p -> left == NULL || isVisited(p -> left,myset))
                 && (p -> right == NULL || isVisited(p -> right,myset))  )
            {
                sum += p -> val;
                mystack.pop();
            }
            else
            {
                if(p -> left != NULL && !isVisited(p -> left,myset))
                    mystack.push(p -> left);
                if(p -> right != NULL && !isVisited(p -> right,myset))
                    mystack.push(p -> right);
            }
        }
        return false;
    }
private:
    bool isLeafNode(TreeNode* root)
    {
        return ( root -> left == NULL && root -> right == NULL );
    }
    bool isVisited(TreeNode* root, set<TreeNode*> myset)
    {
        return ( myset.find(root) != myset.end() );
    }
};
```