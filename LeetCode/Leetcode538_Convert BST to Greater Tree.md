Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

Example:

Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13


递归
```
/** 思路
* Step 1: 求递减序列array：13，5，2
* Step 2: 做加法：array[i] += array[i-1];
*/
// 55 ms
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) 
    {
        int front = 0;
        set<TreeNode*> myset;
        convert(root,front,myset);
        return root;
    }
private:
    void convert(TreeNode* root, int& front,set<TreeNode*>& myset)
    {
        if(root != NULL && (isNull(root-> right) || isVisited(root-> right,myset)))
        {
            root -> val += front;
            front = root -> val;
            myset.insert(root);
        }
        if(root != NULL && root -> right != NULL && !isVisited(root -> right,myset))
            convert(root -> right, front,myset);
        if(root != NULL && !isVisited(root,myset))
            convert(root,front,myset);
        if(root != NULL && root -> left != NULL && !isVisited(root -> left,myset))
            convert(root -> left, front,myset);
    }
    bool isVisited(TreeNode* root, set<TreeNode*>& myset)
    {
        return myset.find(root) != myset.end();
    }
    bool isNull(TreeNode* root)
    {
        return root == NULL;
    }
};
```

递归
```
// 42 ms
// 按键值递减序列顺序更新
// sum 记录当前位置之前的所有值的累加和
class Solution {
private:
    int sum = 0; 
public:
    TreeNode* convertBST(TreeNode* root) 
    {
        if(root != NULL)
        {
            convertBST(root -> right); 
            sum += root -> val;        
            root -> val = sum;
            convertBST(root -> left);
        }
        return root;
    }
};
```