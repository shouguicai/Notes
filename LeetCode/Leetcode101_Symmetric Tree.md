Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following [1,2,2,null,3,null,3] is not:
    1
   / \
  2   2
   \   \
   3    3
Note:
Bonus points if you could solve it both recursively and iteratively.

Approach  #1 递归解
```
//递归解 9 ms
/*
	一个镜像对称的树，它的左子树和右子树关于轴对称；
	左子树的左(右)儿子等于右子树的右(左)儿子；
*/
class Solution {
public:
    bool isSymmetric(TreeNode* root) 
    {
        return isMirror(root,root);
    }
private:
	bool isMirror(TreeNode* t1, TreeNode* t2)
	{   // t1 - left subtree, t2 - right subtree
		return    (t1 == NULL && t2 == NULL)
		       || (   (t1 != NULL && t2 != NULL)
                   && (t1->val == t2->val)
		           && isMirror(t1->left,t2->right)
		           && isMirror(t1->right,t2->left)  );
	}
};
```

Approach  #1 迭代解(借助队列)
```
// 6 ms
class Solution {
public:
    bool isSymmetric(TreeNode* root) 
    {
    	queue<TreeNode*> myqueue;
    	TreeNode* t1,t2;
    	myqueue.push(root);
    	myqueue.push(root);
    	while(!myqueue.empty())
    	{
    		t1 = myqueue.front();myqueue.pop();
    		t2 = myqueue.front();myqueue.pop();
    		if (( (t1 != NULL) != (t2 !=NULL) ) || (t1 != NULL && t2 != NULL && t1->val != t2->val))
    		                                                   // 返回false的几个情况
    			return false;                                  // a. t1,t2非空，值不等
                                                              //  b. t1,t2一个空，一个非空 c++ 没有逻辑异或 用(t1 != NULL) != (t2 !=NULL)代替
    		if(t1 != NULL && t2 != NULL)
    		{
    			myqueue.push(t1.left);
    			myqueue.push(t2.right);
    			myqueue.push(t1.right);
    			myqueue.push(t2.left);
    		}
    	}
    return true;
    }
};
```