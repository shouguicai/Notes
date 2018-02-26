---
title: 重建二叉树
date: 2018-02-15
mathjax: true
tags: 树
---

题目描述:输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数。

Example:

输入 preorder = [3,9,20,15,7] inorder = [9,3,15,20,7]

输出:

```
    3
   / \
  9  20
    /  \
   15   7
```

## Naive思路

```
cclass Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) 
    {
        int n = preorder.size();
        if(n == 0)
            return NULL;
        unordered_map<int,int> hash;
        // 中序遍历存在hash,方便查找
        for(int i = 0; i < n; i++)
            hash[inorder[i]] = i;
        return ConstructBinaryTree(preorder,0,inorder,0,n-1,hash);
    }
private:
    TreeNode* ConstructBinaryTree(vector<int> &preorder, int pre_l,vector<int>&inorder, int vin_l, int vin_h,unordered_map<int,int>& hash) 
    {
        int key = preorder[pre_l];
        int mid = hash[key]; // 中序数组中以key为中心，划为左右子树
        int pre_mid = pre_l + mid - vin_l;
        TreeNode* T = new TreeNode(key); // 必须是new, TreeNode T(key)形式T的作用域有限制
        if(mid > vin_l)
            T -> left  = ConstructBinaryTree(preorder,pre_l + 1,inorder,vin_l,mid - 1,hash);
        if(vin_h > mid)
            T -> right = ConstructBinaryTree(preorder,pre_mid + 1,inorder,mid + 1,vin_h,hash);
        return T;
  }
```