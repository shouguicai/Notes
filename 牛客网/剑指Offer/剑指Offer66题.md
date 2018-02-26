---
title: 剑指Offer
date: 2018-02-15
mathjax: true
tags: 经典算法题
---

<!--more-->

## 二维数组中的查找

问题叙述：在一个二维数组中，每一行都按照从左到右递增的顺序排序，
每一列都按照从上到下递增的顺序排序。
请完成一个函数，输入这样的一个二维数组和一个整数，
判断数组中是否含有该整数。

考点：数组

[牛客网](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


考虑对输入矩阵逐行二分搜索。如果在当前行找到目标元素则返回```true```,
否则在下一行继续搜索。假设搜索停止时，两指针停止在left和right位置，
考虑到array向右和向下的递增关系，下一行只需要在[0:right]之间搜索。

```
/***
* 运行时间：10ms
* 占用内存：1404k
*/
class Solution {
public:
    bool Find(int target, vector<vector<int>> array) 
    {
      int right = array[0].size()-1;
      for(int i = 0; i < array.size(); i++)
      {
        int left = 0;
        right = array[i].size()-1 < right ? array[i].size()-1 : right;
        for(;left <= right;)
        {
          int mid = (left + right)/2;
          if(array[i][mid] == target)
            return true;
          else if(array[i][mid] > target)
            right = mid - 1;
          else // array[i][mid] < target
            left = mid + 1;
        }
      }
      return false;
    }
};
```

## 替换空格 

问题叙述：请实现一个函数，将一个字符串中的空格替换成“%20”。
例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

考点：字符串

[牛客网](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

考察数组的插入操作。

先遍历一遍统计空格数，然后从后往前处理字符串。注意字符串以```\0```结尾，空格为```' '```。

```
/***
* 运行时间：5ms
* 占用内存：632k
*/
class Solution {
public:
  void replaceSpace(char *str,int length) 
  {
    int numofSpace = 0;
    // 统计空格数
    for(int i = 0; i < length; i++)  
      if(str[i] == ' ')
          numofSpace ++;

    int newlength = length + numofSpace*2;
    int j = newlength;
    // 从后往前处理字符串
    for(int i = length; i >= 0; i--) 
    {
      if(str[i] == ' ')
      {
        str[j--] = '0';
        str[j--] = '2';
        str[j--] = '%';
      }
      else
        str[j--] = str[i];
    }
  }
};
```

## 二叉树的镜像

问题叙述：操作给定的二叉树，将其变换为源二叉树的镜像。

考点：面试思路

[Leetcode Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

[牛客网](https://www.nowcoder.com/practice/564f4c26aa584921bc75623e48ca3011?tpId=13&tqId=11171&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/***
* 运行时间：3ms
* 占用内存：400k
*/
class Solution {
public:
    void Mirror(TreeNode *pRoot) 
    {
      if(pRoot != NULL)
      {
        // mirror left and right tree first
        Mirror(pRoot -> left);
        Mirror(pRoot -> right);  
        // then swap left tree and right tree
        TreeNode *tmp = pRoot -> left;
        pRoot -> left = pRoot -> right;
        pRoot -> right = tmp;
      }
    }
};
```

## 从尾到头打印链表 

题目描述:输入一个链表，从尾到头打印链表每个节点的值。

考点：链表

[牛客网](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?tpId=13&tqId=11156&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：400k
*/
class Solution {
public:
    vector printListFromTailToHead(ListNode* head) 
    {
        vector v;
        // 链表元素逐个插入vector尾部
        while(head != NULL)
        {
          v.push_back(head -> val);
          head = head -> next;
        }
        // 将vector倒序
        int n = v.size();
        for(int i = 0; i < n/2; i++)
        {
          int tmp = v[i];
          v[i] = v[n-i-1];
          v[n-i-1] = tmp;
        }
        return v;
    }
};

```

## 重建二叉树

题目描述:输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

考点：树 

[牛客网](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

[Leetcode 105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

递归思路：

1. 前序遍历的第一个元素key初始化为根节点T的val，
2. 在中序遍历中查找key的位置，其左边的为T的左子树元素，右边的为T的右子树元素，
3. 递归构建T的左子树和右子树。

```
/***
* 运行时间：4ms
* 占用内存：636k
*/
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) 
    {
      int n = pre.size();
      if(n == 0)
        return NULL;
      unordered_map<int,int> hash;
      // 中序遍历存在hash,方便查找
      for(int i = 0; i < n; i++)
        hash[vin[i]] = i;
      return ConstructBinaryTree(pre,0,vin,0,n-1,hash);
    }
private:
  TreeNode* ConstructBinaryTree(vector<int> &pre, int pre_l,vector<int>&vin, int vin_l, int vin_h,unordered_map<int,int>& hash) 
  {
    int key = pre[pre_l];
    int mid = hash[key]; // 中序数组中以key为中心，划为左右子树
    int pre_mid = pre_l + mid - vin_l;
    TreeNode* T = new TreeNode(key); // new
    if(mid > vin_l)
      T -> left  = ConstructBinaryTree(pre,pre_l + 1,vin,vin_l,mid - 1,hash);
    if(vin_h > mid)
      T -> right = ConstructBinaryTree(pre,pre_mid + 1,vin,mid + 1,vin_h,hash);
    return T;
  }
};
```

## 用两个栈实现队列

题目描述：
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

考点：栈，队列

[牛客网](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=13&tqId=11158&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tPage=1)

队列先进先出，栈先进后出。

参考推荐答案

入队：将元素进stack1 

出队：判断stack2是否为空，如果为空，则将stack1中所有元素pop，并push进stack2，stack2再pop； 
如果不为空，stack2直接pop。

```
/***
* 运行时间：3ms
* 占用内存：512k
*/
class Solution
{
public:
    void push(int node) 
    {
      stack1.push(node);
    }

    int pop() 
    {
      if(stack2.empty())
      {
        while(!stack1.empty())
        {
          stack2.push(stack1.top());
          stack1.pop();
        }
      }

      int num = stack2.top();
      stack2.pop();
      return num;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

引申：用两个队列实现一个栈的功能?

入栈：将元素进队列A

出栈：判断队列A中元素的个数是否为1，如果等于1，则出队列，否则将队列A中的元素
以此出队列并放入队列B，直到队列A中的元素留下一个，然后队列A出队列，
再把队列B中的元素出队列以此放入队列A中。

## 旋转数组中的最小数字

题目描述：
把一个数组最开始的若干个元素搬到数组的末尾，
我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，
输出旋转数组的最小元素。 

考点：查找和排序

Example:

例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，
该数组的最小值为1。

NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

[牛客网](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&tqId=11159&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tPage=1)

### 遍历一下

```
/***
* 运行时间：34ms
* 占用内存：640k
*/
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) 
    {
        int n = rotateArray.size();
        int ans = 0;
        for(int i = 1; i < n; i++)
          if(rotateArray[i] < rotateArray[i-1])
            ans = rotateArray[i];
        return ans;
    }
};
```

时间复杂度O(n)，空间复杂度O(1)。

### 二分查找

查找时分两种情况：    

1、array[m] > array[r]：说明旋转后最小值在右区间，

2、否则，说明旋转后最小值在左区间。  

```
/**
* 运行时间：32ms
* 占用内存：640k
*
*/
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) 
    {
        int n = rotateArray.size();
        if(n == 0) return 0;
        int l = 0;
        int r = n - 1;
        for(;l < r;)
        {
          m = l + (r - l)/2;
          if(rotateArray[m] > rotateArray[r])
            l = m + 1;
          else
            r = m; // 非递减 <=
        }
        return rotateArray[l];
    }
};
```

时间复杂度O(logn)，空间复杂度O(1)。

## 斐波那契数列

题目描述：
大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。
n<=39

考点：递归和循环

[牛客网](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?tpId=13&tqId=11160&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```
/*
* 运行时间：4ms
* 占用内存：396k
*/
class Solution {
public:
    int Fibonacci(int n) 
    {
      vector<int> v(n + 1);
      v[0] = 0;
      v[1] = 1;
      for(int i = 2;i <= n;i++)
        v[i] = v[i-1] + v[i-2];
      return v[n];
    }
};
```
## 跳台阶

题目描述：
一只青蛙一次可以跳上1级台阶，也可以跳上2级。
求该青蛙跳上一个n级的台阶总共有多少种跳法。

考点：递归和循环

[牛客网](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=13&tqId=11161&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/*
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  int jumpFloor(int number) 
  {
    vector<int> v(number + 1);
    v[0] = 1;
    v[1] = 1;
    for(int i = 2; i <= number; i++)
      v[i] = v[i - 1] + v[i - 2];
    return v[number];      
  }
};
```

## 变态跳台阶

题目描述：
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。
求该青蛙跳上一个n级的台阶总共有多少种跳法。

考点：递归和循环

[牛客网](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=13&tqId=11162&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

可以选择先跳j个台阶（共有n个选择），
每种选择对应的方案数是跳n - j级台阶的跳法，
总的，跳n个的方案数为n种选择的累加。

$$F(n) = \sum_{j=1}^{n}{F(j)}$$

```
/***
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  int jumpFloorII(int number)
  {
    vector<int> v(number + 1);
    v[0] = 1;
    for(int i = 1; i <= number; i++)
      for(int j = 1; j <= i; j++)
        v[i] += v[i-j];
    return v[number];      
  }
};
```

## 矩形覆盖

题目描述
我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。
请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，
总共有多少种方法？

考点：递归和循环

[牛客网](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6?tpId=13&tqId=11163&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

首先，只存在两种铺法，横着铺一条或者竖着铺两条。
问题就转化为了青蛙跳台阶问题，只是初始条件略有不同。

```
/**
* 运行时间：4ms
* 占用内存：508k
*/
class Solution {
public:
    int rectCover(int number) 
    {
      vector<int> v(number + 1);
      v[1] = 1;
      v[2] = 2;
      for(int i = 3;i <= number; i++)
         v[i] = v[i-1] + v[i-2];
      return v[number];
    }
};
```

## 二进制中1的个数

题目描述：
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

考点：位运算 

[牛客网](https://www.nowcoder.com/practice/8ee967e43c2c4ec193b040ea7fbb10b8?tpId=13&tqId=11164&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

计算机中的数的表示形式为补码。```int```为四字节整形，为32位。

### 无符号整数时的情况

[Leetcode 191](https://leetcode.com/problems/number-of-1-bits/description/)
是统计无符号整数中1 bit的个数，代码如下

```
class Solution {
public:
     int  NumberOf1(int n) 
     {
        int count = 0;
        while(n)
        {
          count += n & 0x01;
          n >>= 1;
        }
        return count;
     }
};
```

这个解法在这里可能会陷入死循环，如果输入是负数，
```>>```运算符在最高位补得是1,那么会有无数个1了。   

### 去掉负数的符号位 

因此需要一点点特殊操作,用n & 0x7FFFFFFF，
将最高位的符号位1变成0，把负数转化成正数。

```
/*
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
     int  NumberOf1(int n) 
     {
        int count = 0;
        if(n < 0)
        {
          n &= 0x7FFFFFFF;
          count ++;
        }
        while(n)
        {
          count += n & 1;
          n >>= 1;
        }
        return count;
     }
};
```

### 奇特的 n = n & (n - 1)

参考推荐答案，```n = n & (n - 1)```效果是把n二进制表示最右边一个1变成0。

```
/*
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
     int  NumberOf1(int n) 
     {
        int count = 0;
        while(n)
        {
          ++ count;// 据说++count 比 count++ 更高效
          n &= n - 1;
        }
        return count;
     }
};
```

## 数值的整数次方 

题目描述：
给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

考点：代码的完整性，位运算

[牛客网](https://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?tpId=13&tqId=11165&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)


1. 要用到指数的二进制表达。举例:10^1101 = 10^0001*10^0100*10^1000。

2. 通过```&1```和```>>1```来逐位读取exp，为1时将该位代表的乘数累乘到最终结果。

3. 如果指数为负，则先计算正指数，然后去倒数即可。


```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    double Power(double base, int exponent) 
    {
      double ans = 1;
      int exp = abs(exponent);
      while(exp)
      {
        if(exp & 1) ans *= base;
        base *= base;
        exp >>= 1;
      }
      return exponent < 0 ? 1/ans : ans;
    }
};
```

- 一个需要注意的点：```int```为四字节整形，32位，范围是 -2^31 ~ 2^31 -1，
  那么当指数exponent = INT_MIN时，```abs(exponent)```会溢出，正确的做法是
  拆出一个平方到底数上，即求```Power(base*base,INT_MIN/2)```。


## 调整数组顺序使奇数位于偶数前面 

题目描述:
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，
使得所有的奇数位于数组的前半部分，
所有的偶数位于位于数组的后半部分，并保证奇数和奇数，
偶数和偶数之间的相对位置不变。

考点：代码的完整性，两指针

[牛客网](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?tpId=13&tqId=11166&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

## Naive思路

1. 整体的想法是遍历数组不断把遇到奇数移到数组的前部。
2. 用两个指针，```odd```和```even```指示需要交换的位置。
3. 先指针```even```遇到偶数停下，指针```even```从```odd```
  右侧开始，遇到第一个奇数停下，逐次交换两指针之间的元素。
4. 指针```even++```。

```
/**
* 运行时间：3ms
* 占用内存：396k
*/
class Solution {
public:
    void reOrderArray(vector<int> &array) 
    {
      int n = array.size();
      int odd = 0;
      int even = 0;
      int tmp;
      for(;odd < n;)
      {
        for(;even < n && array[even] % 2 != 0;even++); // 第一个偶数停下
        for(odd = even + 1;odd < n && array[odd] % 2 == 0;odd++); // 第一个奇数停下
        if(odd < n)
        {
          tmp = array[odd];
          for(int i = odd;i > even;--i) array[i] = array[i-1]; // 逐次交换两指针之间的元素
          array[even++] = tmp;
        }
      }
    }
};
```

## 链表中倒数第k个结点

题目描述：
输入一个链表，输出该链表中倒数第k个结点。

考点：代码的鲁棒性，两指针

[牛客网](https://www.nowcoder.com/practice/529d3ae5a407492994ad2a246518148a?tpId=13&tqId=11167&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

1. 两指针，一快一慢，快的比慢的快k个节点，
2. 当快指针停止时，慢指针所指即为倒数第k个结点，
3. 快指针先跑k个节点后，慢再开始跑。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) 
    {
      int count = 0;
      ListNode* p = pListHead; // 快指针
      ListNode* q = pListHead; // 慢指针
      for(;p != NULL && count < k; ++count) p = p -> next;// 快指针先跑k个节点
      for(;p != NULL;p = p -> next,q = q -> next);
      return count < k ? NULL : q; // 节点总数小于k输出NULL，鲁棒性
    }
};
```

## 反转链表 

题目描述：
输入一个链表，反转链表后，输出链表的所有元素。

考点：代码的鲁棒性，递归

[牛客网](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=13&tqId=11168&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

思路：递归

1. 假设第二个到最后一个节点已经反转。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) 
    {
      if(pHead == NULL || pHead -> next == NULL) return pHead;
      ListNode* p = ReverseList(pHead -> next);
      (pHead -> next) -> next = pHead;
      pHead -> next = NULL;
      return p;
    }
};
```

## 合并两个排序的链表 

题目描述：
输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

考点：代码的鲁棒性，指针

[牛客网](https://www.nowcoder.com/practice/d8b6b4358f774294a89de2a6ac4d9337?tpId=13&tqId=11169&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

思路：两指针

1. 两指针分别在两链表中遍历，
2. 将两指针处较小的节点插入新链表head。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL || pHead2 == NULL)
          return pHead1 == NULL ? pHead2: pHead1;
        ListNode* head;
        if(pHead1 -> val < pHead2 -> val) 
        {
            head = pHead1;
            pHead1 = pHead1 -> next;
        }
        else
        {
            head = pHead2;
            pHead2 = pHead2 -> next; 
        }
        ListNode* p = head;
       
        while(pHead1 != NULL || pHead2 != NULL)
        {
          if(pHead1 == NULL || pHead2 == NULL)
          {
            p -> next = pHead1 == NULL ? pHead2: pHead1;
            break;
          }
          else
          {
            if(pHead1 -> val < pHead2 -> val)
            {
              p -> next = pHead1;
              pHead1 = pHead1 -> next;
              p = p -> next;
            }
            else //pHead1 -> val >= pHead2 -> val
            {
              p -> next = pHead2;
              pHead2 = pHead2 -> next;
              p = p -> next;
            }
          }
        }
        return head;
    }
};
```

## 树的子结构 

题目描述:
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

考点：代码的鲁棒性


类似[Leetcode 572](https://leetcode.com/problems/subtree-of-another-tree/description/)，
只不过这边的要求比弱一些，只要A的某部分和B相同就行，不需要考虑all descendants。

思路：递归

### 树B是树A（根为root）的子结构的三种情况，

1.  以root为根的一个子结构和树B完全相同，
2.  树B是root的左子树的子结构，
3.  或者，树B是root的右子树的子结构。

### 两根树相同的判断

1.  两棵树都是空树，
2.  根节点值相同，并且左右子树对应为相同树，
3.  否则，两棵树不同。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
  {
    // 空树不是任意一个树的子结构
    if(pRoot2 == NULL) 
      return false;
    return (isSame(pRoot1,pRoot2)  
           || (pRoot1 != NULL && (HasSubtree(pRoot1 -> left,pRoot2) 
           || HasSubtree(pRoot1 -> right, pRoot2))));
  }
private:
  bool isSame(TreeNode* pRoot1, TreeNode* pRoot2)
  {
    // 因为不要求考虑pRoot1的all descendants，
    // pRoot2 为 NULL 时判真
    if(pRoot2 == NULL) 
      return true;
    if(pRoot1 != NULL && pRoot2 != NULL 
                      && pRoot1 ->val == pRoot2 -> val)
      return isSame(pRoot1 -> left, pRoot2 -> left) 
          && isSame(pRoot1 -> right, pRoot2 -> right);
    return false;
  }
};
```

## 顺时针打印矩阵 

题目描述:
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，
例如，如果输入如下矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 
则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

考点：画图让抽象形象化，数组，递归

[牛客网](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?tpId=13&tqId=11172&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

1.一圈一圈的打印，用左上角和右下角左边确定矩形圈，
2.先打印上边缘，再右边缘，接着下边缘，最后左边缘。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  vector<int> printMatrix(vector<vector<int> > matrix) 
  {
    vector<int> v;
    int m = matrix.size();
    int n = matrix[0].size();
    int left_top_x = 0; int left_top_y = 0;
    int right_down_x = m-1; int right_down_y = n-1;
    for(;left_top_x <= right_down_x && left_top_y <= right_down_y;)
      printSquareMargin(matrix,v,left_top_x++,left_top_y++,right_down_x--,right_down_y--);
    return v;
  }
private:
  void printSquareMargin(vector<vector<int>>& matrix,vector<int>& v,int left_top_x,int left_top_y,int right_down_x,int right_down_y)
  {
    for(int i = left_top_y;i <= right_down_y;i++) //上边缘
      v.push_back(matrix[left_top_x][i]);
    for(int i = left_top_x + 1;i <= right_down_x;i++) //右边缘
      v.push_back(matrix[i][right_down_y]);
    if(right_down_x != left_top_x) // 避免matrix为1*n时,打印两遍
      for(int i = right_down_y -1 ;i >= left_top_y;i--) //下边缘
        v.push_back(matrix[right_down_x][i]);
    if(left_top_y != right_down_y) // 避免matrix为m*1时,打印两遍
      for(int i = right_down_x - 1;i > left_top_x;i--) //左边缘
        v.push_back(matrix[i][left_top_y]);
  }
};
```

## 包含min函数的栈 

题目描述:
定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

考点：举例让抽象具体化 ，栈

[牛客网](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=13&tqId=11173&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

[Leetcode 155](https://leetcode.com/problems/min-stack/description/)

使用两个栈

1. 一个正常push,pop,top;  mystack1
2. 另一个用来保持最小元素 mystack2

```
/**
* 运行时间：3ms
* 占用内存：396k
*/
class Solution {
public:
    void push(int value) 
    {
        mystack1.push(value);
        if(mystack2.empty() || value < min())
          mystack2.push(value);
    }
    void pop() 
    {
      if(top() == min())
        mystack2.pop();
      mystack1.pop();
    }
    int top() 
    {
        return mystack1.top();
    }
    int min() 
    {
        return mystack2.top();
    }
private:
  stack<int> mystack1;
  stack<int> mystack2;
};
```

## 栈的压入、弹出序列 

题目描述:
输入两个整数序列，第一个序列表示栈的压入顺序，
请判断第二个序列是否为该栈的弹出顺序。
假设压入栈的所有数字均不相等。
例如序列1,2,3,4,5是某栈的压入顺序，
序列4，5,3,2,1是该压栈序列对应的一个弹出序列，
但4,3,5,1,2就不可能是该压栈序列的弹出序列。
（注意：这两个序列的长度是相等的）

考点：举例让抽象具体化

[牛客网](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&tqId=11174&tPage=2&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) 
    {
        stack<int> mystack;
        int n = pushV.size();
        int j = 0;
        for(int i = 0;i < n;i++)
        {
          if(pushV[i] != popV[j])
            mystack.push(pushV[i]);
          else // pushV[i] == popV[j]
            j++;

        }

        while(!mystack.empty())
        {
          if(mystack.top() != popV[j++])
            return false;
          mystack.pop();
        }
        return true;
    }
};
```

## 从上往下打印二叉树 

题目描述:
从上往下打印出二叉树的每个节点，同层节点从左至右打印。

考点：举例让抽象具体化，宽度优先搜索

[牛客网](https://www.nowcoder.com/practice/7fe2212963db4790b57431d9ed259701?tpId=13&tqId=11175&tPage=2&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) 
    {
      TreeNode* p;
      queue<TreeNode*> myqueue;
      vector<int> v;
      if(root == NULL) return v;
      myqueue.push(root);
      while(!myqueue.empty())
      {
        p = myqueue.front();
        if(p -> left != NULL)
          myqueue.push(p -> left);
        if(p -> right != NULL)
          myqueue.push(p -> right);
        v.push_back(p -> val);
        myqueue.pop();
      }
      return v;
    }
};
```

## 二叉搜索树的后序遍历序列 

题目描述:
输入一个整数数组，
判断该数组是不是某二叉搜索树的后序遍历的结果。
如果是则输出Yes,否则输出No。
假设输入的数组的任意两个数字都互不相同。

考点：举例让抽象具体化，二叉搜索树的定义

[牛客网](https://www.nowcoder.com/practice/a861533d45854474ac791d90e447bafd?tpId=13&tqId=11176&tPage=2&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

二叉查找树（二叉排序树）：左子树上所有结点的值均小于它的根结点的值；
树上所有结点的值均大于它的根结点的值。它的左、右子树也分别为二叉查找树。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  bool VerifySquenceOfBST(vector<int> sequence) 
  {
    if(sequence.empty()) return false;
    return isBST(sequence,0,sequence.size()-1);
  }
private:
  bool isBST(vector<int>& v, int l, int h)
  {
    if(l >= h)
      return true;
    int root = v[h];
    int i = l;
    for(;i < h && v[i] < root;i++); // l ~ i-1 为左子树，i ~ h-1为右子树
    for(int j = i;j < h;j++)       // 检查右子树结点的值是否均大于根结点值
      if(v[j] < root)
        return false;
    return isBST(v,l,i-1) && isBST(v,i,h-1);// 检查左右子树是否为BST
  }
};
```
## 二叉树中和为某一值的路径 

题目描述:
输入一颗二叉树和一个整数，
打印出二叉树中结点值的和为输入整数的所有路径。
路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

考点：举例让抽象具体化

[牛客网](https://www.nowcoder.com/practice/b736e784e3e34731af99065031301bca?tpId=13&tqId=11177&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

[Leetcode 112](https://leetcode.com/problems/path-sum/description/)
和 [Leetcode 437](https://leetcode.com/problems/path-sum-iii/description/)

递归遍历二叉树路径

```
/**
* 运行时间：3ms
* 占用内存：393k
*/
class Solution {
public:
  vector<vector<int>> FindPath(TreeNode* root,int expectNumber) 
  {
    vector<int> path;
    vector<vector<int>> ans;
    findPathSum(root,expectNumber,ans,path);
    return ans;
  }
private:
  void findPathSum(TreeNode* root,int sum,vector<vector<int>>& ans,vector<int> path)
  { // path 传值，不要传引用
    if(root == NULL)
      return;
    path.push_back(root -> val);
    if(root -> left == NULL  // 到达叶子节点
        && root -> right == NULL && root -> val == sum)
      ans.push_back(path); // 找到答案
    //find左子树
    findPathSum(root -> left,sum - root -> val,ans,path,count);
    //find右子树
    findPathSum(root -> right,sum - root -> val,ans,path,count);
    path.pop_back();
  }
};
```

## 复杂链表的复制

题目描述:
输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，
另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。
（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

考点：分解让复杂问题简单 

[牛客网](https://www.nowcoder.com/practice/f836b2c43afc4b35ad6adc41ec941dba?tpId=13&tqId=11178&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

参考推荐答案

1. 第一趟遍历链表并把复制的结点A1放在原节点A后面，得到AA1BB1CC1DD1形式的链表，
2. 第二趟，逐个更新新节点的random，```A1->random = A->random->next```，比如说```A->random = &B```，
  有第一趟的结果，将有```A1->random = &B1```，这些就完成了原链表的复制，
3. 第三趟拆分新旧链表。

```
/**
* 运行时间：3ms
* 占用内存：384k
*/
class Solution {
public:
    /*       
    1、复制每个节点，如：复制节点A得到A1，将A1插入节点A后面        
    2、遍历链表，A1->random = A->random->next;        
    3、将链表拆分成原链表和复制后的链表    
    */
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead == NULL) return NULL;
        RandomListNode* oriNode = pHead;
        RandomListNode* cloneNode;
        // 第一趟复制链表,并连成AA1BB1...的形式
        while(oriNode)
        {
          cloneNode = new RandomListNode(oriNode -> label);
          cloneNode -> next = oriNode -> next;
          oriNode -> next = cloneNode;
          oriNode = cloneNode -> next;
        }
        //第二趟，更新新新节点的random
        oriNode = pHead; // 指向旧结点
        while(oriNode)
        {
          cloneNode = oriNode -> next; // 指向复制的新结点
          if(oriNode -> random)
            cloneNode -> random = oriNode -> random -> next;
          oriNode = cloneNode -> next;

        }

        //第三趟，拆分新旧链表
        RandomListNode* pcloneHead = pHead -> next;
        RandomListNode* currNode = pHead;
        RandomListNode* tmp;
        while(currNode -> next)
        {
          tmp = currNode -> next;
          currNode -> next = tmp -> next;
          currNode = tmp;
        }
        return pcloneHead;
    }
};
```

## 二叉搜索树与双向链表 

题目描述:
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

考点：分解让复杂问题简单

[牛客网](https://www.nowcoder.com/practice/947f6eb80d944a84850b0538bf0ec3a5?tpId=13&tqId=11179&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

类似中序遍历，```Convert2Dlist```函数返回双向链表的首尾结点指针。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        vector<TreeNode*> v = Convert2Dlist(pRootOfTree);
        return v[0];
        
    }
 private:
    vector<TreeNode*> Convert2Dlist(TreeNode* root)
    {
      vector<TreeNode*> v(2,root);
      vector<TreeNode*> left,right;
      if(root != NULL && root -> left != NULL)
      {
          left = Convert2Dlist(root -> left);
          left[1] -> right = root;
          root -> left = left[1];
          v[0] = left[0]; // v[0] 首结点指针
      }
      if(root != NULL && root -> right != NULL)
      {   right = Convert2Dlist(root -> right);
          right[0] -> left = root;
          root -> right = right[0];
          v[1] = right[1]; // v[1] 尾结点指针
      }
       return v;
    }
};
```

## 字符串的排列 

题目描述:
输入一个字符串,按字典序打印出该字符串中字符的所有排列。
例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

输入描述:
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

考点：分解让复杂问题简单

[牛客网](https://www.nowcoder.com/practice/fe6b651b66ae47d7acce78ffdd9a96c7?tpId=13&tqId=11180&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

### 回溯法

参考 [CSDN](http://blog.csdn.net/u013490101/article/details/50587997)。

思路：先确定某一部step的选择并置状态为1，以表示某个资源已被使用；
然后把“选择”和当前状态传到到step+1，直到第N+1步，才结束。
然后回溯回去的时候，要重新置状态为0，这一点是回溯法的重要标志！


```
/**
* 运行时间：5ms
* 占用内存：512k
*/
class Solution {
public:
  vector<string> Permutation(string str) 
  {
    vector<string> ans;
    if(!str.empty())
      PermutationHelper(ans,0,str);
    sort(ans.begin(),ans.end()); // 排序
    return ans;
  }
private:
  void PermutationHelper(vector<string>& ans,int i,string str)
  {
    if(i == str.size()-1)
      ans.push_back(str);
    else
    {
      for(int j = i;j < str.size();++j)
      {
        if(j != i && str[i] == str[j]) // 跳过重复解
          continue;
        swap(str,i,j);
        PermutationHelper(ans,i+1,str);
        swap(str,i,j);
      }
    }
  }
  void swap(string &str,int i,int j)
  {
    char tmp = str[i];
    str[i] = str[j];
    str[j] = tmp;
  }
};
```

## 数组中出现次数超过一半的数字 

考点：时间效率 

[牛客网](https://www.nowcoder.com/practice/e8a1b01a2df14cb2b228b30ee6a92163?tpId=13&tqId=11181&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

[Leetcode 169](https://leetcode.com/problems/majority-element/description/) 和 
[Leetcode 229](https://leetcode.com/problems/majority-element-ii/description/)

思路：摩尔投票法

1. 出现超过1/k的数最多只有k-11个，而且不一定存在，
2. 每次从序列里“隐含地”删除掉k个不相同的数字，最后剩下的则可能是出现超过1/k的数，
3. 统计最后剩下的数在序列中出现的次数，是否大于n/k。

1/3的[例子](https://www.cnblogs.com/YaoDD/p/5276958.html)。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) 
    {
      int major; // 候选数字
      int count = 0; // major出现的次数
      int n = numbers.size();
      for(int i = 0;i < n;++i)
      { // major为空，加入，count置1
        if(count == 0){major = numbers[i];++count;}
        // numbers[i]与major不同，一起“删去”，否则count++
        else(numbers[i] == major) ? ++count : --count;
      }
      // 统计候选数字major出现的次数
      count = 0;
      for(int i = 0;i < n;++i)
        if(numbers[i] == major)
          ++count;
      // 出现次数大于一半则输出，否则答案不存在
      return count > n/2 ? major :0;
    }
};
```

## 最小的K个数 

题目描述:
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

考点：时间效率

[牛客网](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=13&tqId=11182&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

参考高票答案

### 全排序

时间复杂度O（nlogn）

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) 
    {
      vector<int> ans;
      int n = input.size();
      if(n==0 ||k > n||k <=0) return ans;
      sort(input.begin(),input.end());
      for(int i = 0;i < k;i++)
        ans.push_back(input[i]);
      return ans;
    }
};
```

### 最大堆

维持一个大小为 k 的最大堆。时间复杂度O（nlogk）。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) 
    {
        vector<int> ans;
        int n = input.size();
        if(n==0 ||k > n||k <=0) return ans;

        for(int i = 0; i < k; i++)
          ans.push_back(input[i]);
        //最大元素在第一个位置
        make_heap(ans.begin(),ans.end());

        for(int i = k; i < n;i++) // k < 0 会造成访问越界
          if(ans[0] > input[i])
          {
            //先pop,然后在容器中删除
            pop_heap(ans.begin(),ans.end());
            ans.pop_back();
            //先在容器中加入，再push
            ans.push_back(input[i]);
            push_heap(ans.begin(),ans.end());
          }
          //使其从小到大排序
          sort_heap(ans.begin(),ans.end());
          return ans;
    }
};
```

### 分割思想

1. 利用快速排序的获取分割点位置函数getPartition，

2. 调整之后，位于分割点左边的数字都比分割点小，右边的则比分割点大（数字不一定是排序的），

3. 当分割点位置为k-1时，则数组左边的k个数是最小k个数。

分割方法：两指针i，j

1. i右移，移过小于枢纽元的元素，j左移动，移过大于枢纽元的元素，
2. 当i,j停止过时，i指向一个大元素，j指向一个小元素，
3. 如果i在j的左边，则元素互换，
4. 重复1~3直到i,j交错，
5. 最后一步是交换枢纽元与i指向的元素。i即为分割点的位置。

时间复杂度 O(N)

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
  vector<int> GetLeastNumbers_Solution(vector<int> input, int k) 
  {
    vector<int> ans;
    int n = input.size();
    if(n == 0 || k > n || k <= 0) return ans;
    int start = 0;
    int end = n - 1;
    int index = getPartition(input,start,end); // 获取分割点的位置
    while(index != (k-1))
    {
      if(index < k-1) // 期望的分割点在右边
        start = index + 1;
      else // index > k-1，则期望的分割点在左边
        end = index - 1;
      index = getPartition(input,start,end);
    }
    for(int i = 0;i < k;i++)
      ans.push_back(input[i]);
    return ans;
  }
private:
  int getPartition(vector<int>& nums,int start,int end)
  {
    int i = start;
    int j = end;
    int Pivot = nums[end]; // 取最后一个元素为枢纽元
    for(;;)
    {
      // i右移，移过小于枢纽元的元素，j左移动，移过大于枢纽元的元素，
      // 当i,j停止过时，i指向一个大元素，j指向一个小元素
      while(i < j && nums[i] <= Pivot) ++i; // 如果不加i < j就会报越界访问，不知道为啥
      while(i < j && nums[j] >= Pivot) --j; 
      if(i < j)  // 如果i在j的左边，则元素互换，
        swap(nums[i],nums[j]); 
      else
        break;
    }
    swap(nums[i],nums[end]); // 交换枢纽元与i指向的元素
    return i; // i即为分割点的位置
  }
  void swap(int& key_1,int& key_2)
  {
    int tmp = key_1;
    key_1 = key_2;
    key_2 = tmp;
  }
};
```

## 连续子数组的最大和 

题目描述:
HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。
今天测试组开完会后,他又发话了:在古老的一维模式识别中,
常常需要计算连续子向量的最大和,当向量全为正数的时候,
问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,
并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},
连续子向量的最大和为8(从第0个开始,到第3个为止)。
你会不会被他忽悠住？(子向量的长度至少是1)

考点：时间效率

[牛客网](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?tpId=13&tqId=11183&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```
/**
* 运行时间：3ms
* 占用内存：396k
*/
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) 
    {
      int sum = 0;
      int max = INT_MIN;
      for(int i = 0; i < array.size();++i)
      {
        if(sum < 0) sum = array[i];
        else sum += array[i];
        max = sum > max ? sum : max;
      }
      return max;
    }
};
```

## 整数中1出现的次数（从1到n整数中1出现的次数） 

题目描述:
求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？
为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,
但是对于后面问题他就没辙了。ACMer希望你们帮帮他,
并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。

考点：时间效率

[牛客网](https://www.nowcoder.com/practice/bd7f978302044eee894445e244c7eee6?tpId=13&tqId=11184&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

[Leetcode 233](https://leetcode.com/problems/number-of-digit-one/description/)

参考推荐答案：按数位分析

对于整数，右起数位依次为个位，十位，百位，等等。
1~n内的所有整数可以通过在各个数位上取不同的数字得到，
并且取值上界为n对应数位上的数字。所以，可以先固定某个数位上为1，
然后求出其他数位的总的取值可能性，就可以得到该数位为1时对应的整数个数。

1. 从右往左逐个数位考虑，
2. 如果当前数位上的数字```>= 2```，那么该数位为1对应的整数个数为
  (a/10+1)*m，左边的数位可以取0~a/10，右边的数位可以取0~m-1，
3. 如果当前数位上的数字```== 1```，分为两部分

    - (a/10)*m，左边的数位可以取0~a/10-1，右边的数位可以取0~m-1，

    - b + 1，左边的数位取a/10，右边数位取0~b,

4. 如果当前数位上的数字```== 0```，那么该数位为1对应的整数个数为
  (a/10)*m，边的数位可以取0~a/10-1，右边的数位可以取0~m-1。
5. 小技巧 ```(a + 8) / 10 ```

    - ```= a/10```      if ```a/10 <= 1```
    - ```= a/10 + 1```  if ```a/10 >= 2```

 
```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    int NumberOf1Between1AndN_Solution(int n)
    {
      int cnt = 0;
      for(int m = 1;m <= n;m *=10)
      {
        int a = n/m, b = n % m;
        cnt += (a + 8) / 10 * m + (a % 10 == 1 ? b + 1 : 0);
      }
      return cnt;
    }
};
```

## 把数组排成最小的数 

题目描述
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，
打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，
则打印出这三个数字能排成的最小数字为321323。

考点：时间效率

[牛客网](https://www.nowcoder.com/practice/8fecd3f8ba334add803bf2a06af1b993?tpId=13&tqId=11185&tPage=2&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

参考牛客网

解题思路： 
先将整型数组转换成String数组，然后将String数组排序，最后将排好序的字符串数组拼接出来。
关键就是制定排序规则。

排序规则如下：

- 若ab > ba 则 a > b， 
- 若ab < ba 则 a < b， 
- 若ab = ba 则 a = b； 

解释说明： 比如 "3" < "31"但是 "331" > "313"，所以要将二者拼接起来进行比较。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) 
    {
        int n = numbers.size();
        vector<string> nums(n);
        string ans = "";
        for(int i = 0;i < n;++i)
          nums[i] = to_string(numbers[i]);
        sort(nums.begin(),nums.end(),comp);
        for(int i = 0;i < n;++i)
          ans += nums[i];
        return ans;
    }
private:
    // comp函数要加static，否则会报非静态错误。
    static bool comp(string s1, string s2)
    {
      return s1 + s2 < s2 + s1;
    }
};
```

## 丑数 

题目描述:
把只包含因子2、3和5的数称作丑数（Ugly Number）。
例如6、8都是丑数，但14不是，因为它包含因子7。 
习惯上我们把1当做是第一个丑数。
求按从小到大的顺序的第N个丑数。

考点：时间空间效率的平衡 

[牛客网](https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?tpId=13&tqId=11186&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

思路：动态规划

后面的丑数是有前一个丑数乘以2，3，5中的一个得来。

```
/**
* 运行时间：3ms
* 占用内存：508k
*/
class Solution {
public:
    int GetUglyNumber_Solution(int index) 
    {
     if(index < 1)
         return 0;
      int i2 = 0, i3 = 0, i5 = 0, cnt = 1;
      vector<int> dp(index);
      dp[0] = 1;
      while(cnt < index)
      {
        int n2 = dp[i2]*2, n3 = dp[i3]*3, n5 = dp[i5]*5;
        int tmp = min(n2,min(n3,n5));
        dp[cnt++] = tmp;
        if(tmp == n2) i2++;
        if(tmp == n3) i3++;
        if(tmp == n5) i5++;
      }
      return dp[index-1];
    }
};
```

## 第一个只出现一次的字符位置 

题目描述:
在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

考点：时间空间效率的平衡

[牛客网](https://www.nowcoder.com/practice/1c82e8cf713b4bbeb2a5b31cf5b0417c?tpId=13&tqId=11187&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：4ms
* 占用内存：396k
*/
class Solution {
public:
    int FirstNotRepeatingChar(string str) 
    {
      vector<int> cnt(256,0);
      for(int i = 0;i < str.size();++i) 
        cnt[str[i]]++;   
      for(int i = 0;i < str.size();++i) 
        if(cnt[str[i]] == 1)
          return i;
      return -1;
    }
};
```

## 数组中的逆序对 

题目描述:
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

输入描述:
题目保证输入的数组中没有的相同的数字
数据范围：
  对于%50的数据,size<=10^4
  对于%75的数据,size<=10^5
  对于%100的数据,size<=2*10^5

示例1 

输入
1,2,3,4,5,6,7,0

输出
7

考点：时间空间效率的平衡

[牛客网](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=13&tqId=11188&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

思路：归并排序

时间复杂度O(nlogn),空间复杂度O(n)。

1. 归并排序的基本操作是合并两个已经排序的表，这个时间是线性的，因为最多进行了N-1次比较。

2. 合并算法Merge，是取两个输入数组A和B，一个输出数组C，以及三个计数器Aptr,Bptr,Cptr,
他们初始置于对应数组的末端。算法不断将A[Aptr]，B[Bptr]中的较大者拷贝到C中的下一个位置，
相关计数器向左推进一步。当两个表有一个用完的时候，则将另一个表中剩余部分拷贝到C中。

3. 算法在每次合并的时候计算逆序对数。如果左半子数组中的数字A[Aptr]大于右半数组中的数字B[Bptr]，则构成逆序对，
并且逆序对的数目等于B子数组中剩余数字的个数。

```
/**
* 运行时间：191ms
* 占用内存：4336k
*/
class Solution {
private:
    long long cnt;
public:
    int InversePairs(vector<int> data) 
    {
        cnt = 0;  
        vector<int> TmpArray(data.size());
        Msort(data,TmpArray,0,data.size()-1);
        return cnt % 1000000007;
    }
private:
    void Msort(vector<int>& data,vector<int>& TmpArray,int left, int right)
    {
      int center;
      if(left < right)
      {
        center = (left + right)/2;
        Msort(data,TmpArray,left,center);
        Msort(data,TmpArray,center+1,right);
        // 任意时刻只需要一个临时数组TmpArray活动
        Merge(data,TmpArray,left,center+1,right);
      }
    }
    void Merge(vector<int>& data,vector<int>& TmpArray,int Lpos,int Rpos,int RightEnd)
    { // 合并算法
      int i,LeftEnd,NumElements,TmpPos;
      LeftEnd = Rpos - 1;
      TmpPos = RightEnd;
      NumElements = RightEnd - Lpos + 1;
      /* 主循环*/
      while(LeftEnd >= Lpos && RightEnd >= Rpos)
      {
        if(data[LeftEnd] > data[RightEnd])
        {
          TmpArray[TmpPos--] = data[LeftEnd--];
          /*计算逆序对数*/
          cnt += RightEnd - Rpos + 1;
        }
        else //data[LeftEnd] <= data[RightEnd]
          TmpArray[TmpPos--] = data[RightEnd--];
      }
      while(LeftEnd >= Lpos) // 拷贝左半剩余部分
        TmpArray[TmpPos--] = data[LeftEnd--];
      while(RightEnd >= Rpos) // 拷贝右半剩余部分
        TmpArray[TmpPos--] = data[RightEnd--];

      /* 拷贝回原数组*/
      for(int i = 0;i < NumElements;i++,Lpos++)
        data[Lpos] = TmpArray[Lpos];

    }
};
```

## 两个链表的第一个公共结点 

考点：时间空间效率的平衡

[牛客网](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=13&tqId=11189&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

[Leetcode 160](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

### hash

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) 
    {
        ListNode *p = pHead1;
        unordered_set<ListNode*> myset;
        while(p != NULL)
        {
            myset.insert(p);
            p = p-> next;
        }

        if(myset.empty())
            return NULL;

        p = pHead2;
        while(p != NULL)
        {
            if(myset.find(p) != myset.end())
                return p;
            p = p -> next;
        }
        return NULL;
    }
};
```

### 8字型方法

把list1的尾部和list2的头相连，构成8的下半部分o，
把list2的尾部和list1的头相连，构成8的上半部分o，
用两指针在上下两个圈里面循环，直到相遇，相遇点即为
两个链表的公共结点 。

```
/**
* 运行时间：3ms
* 占用内存：396k
*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) 
    {
        ListNode* p = pHead1;
        ListNode* q = pHead2;
        while(p != q)
        {
          p = p == NULL ? pHead2 : p -> next;
          q = q == NULL ? pHead1 : q -> next;
        }
        return p;
    }
};
```

## 数字在排序数组中出现的次数 

题目描述:
统计一个数字在排序数组中出现的次数。

考点：知识迁移能力 

[牛客网](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2?tpId=13&tqId=11190&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

参考推荐答案

看就有序就二分

```
/**
* 运行时间：3ms
* 占用内存：384k
*/
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) 
    {
        int m;
        int l = 0;
        int cnt = 0;
        int r = data.size()-1;
        while(l <= r)
        {
          m = l + (r - l)/2;
          // <= 保证算法停止时l指向第一个k
          if(k <= data[m]) 
            r = m - 1;
          else
            l = m + 1;
        }
        for(;l < (int)data.size() && data[l] == k;++l) ++cnt;
        return cnt;
    }
};
```

## 二叉树的深度 

题目描述：
输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/435fb86331474282a3499955f0a41e8b?tpId=13&tqId=11191&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

递归

```
/**
* 运行时间：3ms
* 占用内存：396k
*/
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
      if(pRoot == NULL)
        return 0;
      return 1 + max(TreeDepth(pRoot -> left),TreeDepth(pRoot -> right));
    }
};
```

迭代，BFS

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
      if(pRoot == NULL) return 0;
      TreeNode* p;
      int n;
      int depth = 0;
      queue<TreeNode*> myqueue;
      myqueue.push(pRoot);
      while(!myqueue.empty())
      {
        n = myqueue.size();
        ++ depth;
        // 一层一层的处理
        for(int i = 0;i < n;i++)
        {
          p = myqueue.front();
          myqueue.pop();
          if(p -> left != NULL) myqueue.push(p -> left);
          if(p -> right != NULL) myqueue.push(p -> right);
        }
      }
      return depth;
    }
};
```

## 平衡二叉树 

题目描述:
输入一棵二叉树，判断该二叉树是否是平衡二叉树。

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/8b3b95850edb4115918ecebdf1b4d222?tpId=13&tqId=11192&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```
/**
* 运行时间：3ms
* 占用内存：500k
*/
class Solution {
private: bool isBalanced;
public:
    bool IsBalanced_Solution(TreeNode* pRoot) 
    {
      isBalanced = true;
      int height = TreeDepth(pRoot);
      return isBalanced;
    }
private:
    int TreeDepth(TreeNode* pRoot)
    {
      if(pRoot == NULL) return 0;
      int left = TreeDepth(pRoot -> left);
      int right = TreeDepth(pRoot -> right);
      if(abs(left - right) > 1) isBalanced = false;
      return 1 + max(left,right);
    }
};
```

## 数组中只出现一次的数字 

题目描述:
一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

考点：知识迁移能力，位运算

[牛客网](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

[Leetcode 260](https://leetcodFe.com/problems/single-number-iii/description/)，

按位异或

1. 0^a = a; a^a = 0; a^b^a = (a^a)^b = b

2. 两个不相等的元素在位级表示上必定会有一位存在不同，

3. 将数组的所有元素异或得到的结果为不存在重复的两个元素异或的结果，

4. 据异或的结果1所在的最低位，把数字分成两半，每一半里都还有一个出现一次的数据和其他成对出现的数据,
  问题就转化为了两个独立的子问题“数组中只有一个数出现一次，其他数都出现了2次，找出这个数字”。

```
/**
* 运行时间：3ms
* 占用内存：504k
*/
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) 
    {
      int n = data.size();
      int diff = 0;
      int flag = 1;
      for(int i = 0;i < n;++i)
        diff ^= data[i];
      // == 的优先级大于 &
      while((diff & flag) == 0) flag <<= 1;
      * num1 = 0;
      * num2 = 0;
      for(int i = 0;i < n;++i)
      {
        // == 的优先级大于 &
        if((data[i] & flag) == 0)
          *num1 ^= data[i];
        else
          *num2 ^= data[i];
      }
    }
};
```

### 只有一个数出现一次，其他数都出现了2次

[Leetcode 136](https://leetcode.com/problems/single-number/description/)  

```
class Solution {
public:
    int singleNumber(vector<int>& nums)
    {
        int result = 0;
        for(int i = 0;i<nums.size();i++)
          result ^= nums[i];
        return result;
    }
};
```

### 只有一个数出现一次，其他数都出现了k次

统计所有数对应二进制位上1的总数，如果某些数出现了k次，那么这些数对应二进制为1的位上
1的出现次数为k的整数倍。

[Leetcode 137](https://leetcode.com/problems/single-number-ii/description/)，

```
class Solution {
public:
  int find1From3(vector<int>& nums)
  { 
    vector<int> bits(32,0);              
    int n = nums.size();   
    int ans = 0;      
    for(int i = 0; i < n; i++)      
      for(int j = 0; j < 32; j++)              
        bits[j] += ((nums[i]>>j) & 1);                       

    for(int i = 0; i < 32; i++)        
      if(bits[i] % k != 0) // k次         
        ans |= (1 << i);   
    return ans;                 
  }        
};
```

## 和为S的连续正数序列 

题目描述:
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,
他马上就写出了正确答案是100。但是他并不满足于此,
他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。
没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。
现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

输出描述:
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&tqId=11194&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

双指针问题，当总和小于sum，大指针++，否则小指针++

```
/**
* 运行时间：2ms
* 占用内存：512k
*/
class Solution {
public:
    vector<vector<int>> FindContinuousSequence(int sum) 
    {
        vector<vector<int>> ans;
        int plow = 1,phigh = 2;
        while(plow < phigh)
        {
          int curSum = (plow + phigh) * (phigh - plow + 1) / 2;
          if(curSum < sum)
            ++phigh;
          else if(curSum == sum)
          {
            vector<int> path;
            for(int i = plow;i <= phigh;++i)
              path.push_back(i);
            ans.push_back(path);
            ++plow;
          }
          else // curSum > sum
            ++plow;
        }
        return ans;
    }
};
```

## 和为S的两个数字

题目描述:
输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

输出描述:
对应每个测试案例，输出两个数，小的先输出。

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?tpId=13&tqId=11195&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

双指针问题，当总和小于sum，小指针++，否则大指针--，同时更新和比较乘积。

```
/**
* 运行时间：2ms
* 占用内存：372k
*/
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) 
    {
        int minProduct = INT_MAX;
        vector<int> ans(2);
        int plow = 0;
        int phigh = array.size()-1;
        while(plow < phigh)
        {
          int curSum = array[plow] + array[phigh];
          if(curSum < sum)
            ++plow;
          else if(curSum == sum)
          {
            int curProduct = array[plow] * array[phigh];
            if(curProduct < minProduct)
            {
              ans[0] = array[plow];
              ans[1] = array[phigh];
              minProduct = curProduct;
            }
            ++plow;--phigh;
          }
          else // curSum > sum
            phigh--;
        }
        return minProduct == INT_MAX ? vector<int>() : ans;
    }
};
```

## 左旋转字符串 

题目描述:
汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，
就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，
请你把其循环左移K位后的序列输出。
例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。
是不是很简单？OK，搞定它！

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/12d959b108cb42b1ab72cef4d36af5ec?tpId=13&tqId=11196&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    string LeftRotateString(string str, int n) 
    {
        int len = str.size();
        if(len == 0) return "";
        int pos = n % len;
        return str.substr(pos,len-pos) + str.substr(0,pos);
    }
};
```

## 翻转单词顺序列 

题目描述:
牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，
写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，
有一天他向Fish借来翻看，但却读不懂它的意思。
例如，“student. a am I”。
后来才意识到，这家伙原来把句子单词的顺序翻转了，
正确的句子应该是“I am a student.”。
Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

考点：知识迁移能力

[牛客网](https://www.nowcoder.com/practice/3194a4f4cf814f63919d0790578d51f3?tpId=13&tqId=11197&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

思路：两次翻转

第一次翻转每个单词，第二次再整体翻转。

```
/**
* 运行时间：3ms
* 占用内存：500k
*/
class Solution {
public:
    string ReverseSentence(string str) 
    {
      if(str.size() == 0) return "";
      int i = 0, j = 0;
      int n = str.size();
      for(;j <= n;)
      {
        if(str[j] == ' ' || j == n)
        {
          reverse(str,i,j-1);
          i = j + 1;
          j = i;
        }
        else
          ++j;
      }
      reverse(str,0,n - 1);
      return str;
    }
private:
    void reverse(string& str,int start,int end)
    {
      while(start < end)
      {
        char tmp = str[start];
        str[start++] = str[end];
        str[end--] = tmp;
      }
    }
};
```

## 扑克牌顺子 

考点：抽象建模能力 

[牛客网](https://www.nowcoder.com/practice/762836f4d43d43ca9deb273b3de8e1f4?tpId=13&tqId=11198&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

参考牛客网，思路：

1、排序  
2、计算所有相邻数字间隔总数  
3、计算0的个数  
4、如果2、3相等，就是顺子  
5、如果出现对子，则不是顺子

```
/**
*   运行时间：3ms
*   占用内存：508k
    */
class Solution {
public:
  bool IsContinuous( vector numbers )
  {
    int cnt = 0;
    int cut = 0;
    int n = numbers.size();
    if(n != 5)
      return false;
    sort(numbers.begin(),numbers.end());
    for(int i = 0; i < n;++i)
      if(numbers[i] == 0)
        ++cnt;
    for(int i = cnt;i < n - 1;++i)
    {
      if(numbers[i] == numbers[i+1])  // 出现对子，则不是
        return false;
      cut += numbers[i+1] - numbers[i] - 1;
    }
    if(cut > cnt) // 间隔总数 > 0的个数
        return false;
    return true;
  }
};
```

## 孩子们的游戏(圆圈中最后剩下的数) 

题目描述:
每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。
HF作为牛客的资深元老,自然也准备了一些小游戏。
其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。
然后,他随机指定一个数m,让编号为0的小朋友开始报数。
每次喊到m-1的那个小朋友要出列唱首歌,
然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,
从他的下一个小朋友开始,继续0...m-1报数....这样下去....
直到剩下最后一个小朋友,可以不用表演,
并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。
请你试着想下,哪个小朋友会得到这份礼品呢？
(注：小朋友的编号是从0到n-1)

考点：抽象建模能力

[牛客网](https://www.nowcoder.com/practice/f78a359491e64a50bce2d89cff857eb6?tpId=13&tqId=11199&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

约瑟夫环问题

### 用队列模拟

```
/**
* 运行时间：22ms
* 占用内存：512k
*/
class Solution {
public:
  int LastRemaining_Solution(int n, int m)
  {
    if(n == 0) return -1;
    queue<int> myqueue;
    for(int i = 0;i < n;++i) 
      myqueue.push(i);
    while(myqueue.size() > 1)
    {
      for(int i = 0;i < m - 1;++i)
      {
        int tmp = myqueue.front();
        myqueue.pop();
        myqueue.push(tmp);
      }
      myqueue.pop();
    }
    return myqueue.front();
  }
};
```

### 递归

约瑟夫环问题递推关系为 

```x(n)=(m + x(n-1)) % n;```

其中，x(n)表示n个人，每次报m个数，最后胜利的序号。初始条件，

```
x(1) = 0; 
```

递归程序：
```
/**
* 运行时间：4ms
* 占用内存：640k
*/
class Solution {
public:
  int LastRemaining_Solution(int n, int m)
  {
    if(n <= 1) return n-1;
    return (LastRemaining_Solution(n-1,m) + m) % n;
  }
};
```

DP程序：
```
/**
* 运行时间：3ms
* 占用内存：508k
*/
class Solution {
public:
  int LastRemaining_Solution(int n, int m)
  {
    if(n == 0) return -1;
    vector<int> dp(n+1);
    dp[1] = 0;
    for(int i = 2;i <= n;++i)
      dp[i] = (m + dp[i-1]) % i;
    return dp[n];
  }
};
```

## 求1+2+3+...+n 

题目描述:
求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

考点：发散思维能力 

[牛客网](https://www.nowcoder.com/practice/7a0da8fc483247ff8800059e12d7caf1?tpId=13&tqId=11200&rp=2&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

### && 的短路原则

复杂是O(n)

```
/*
* 运行时间：4ms
* 占用内存：512k
*/
class Solution {
public:
    int Sum_Solution(int n) 
    {
        int ans = n;
        ans && (ans += Sum_Solution(n-1));
        return ans;
    }
};
```

## 不用加减乘除做加法 

题目描述:
写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

考点：发散思维能力

[牛客网](https://www.nowcoder.com/practice/59ac416b4b944300b617d4f7f111b215?tpId=13&tqId=11201&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

### 位运算

不计进位的和为 a^b，进位就是 a&b

a+b = a^b + (a&b)<<1;

[CSDN](https://www.cnblogs.com/dandingyy/archive/2012/10/29/2745570.html)

```
/**
* 运行时间：4ms
* 占用内存：512k
*/
class Solution {
public:
    int Add(int num1, int num2)
    {
      int ans = num1;
      int arr = 0;
      while(num2)
      {
        ans = num1 ^ num2;
        num2 = (num1 & num2) << 1;
        num1 = ans;
      }
      return ans;
    }
};
```

## 把字符串转换成整数 

题目描述:
将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

输入描述:
输入一个字符串,包括数字字母符号,可以为空

输出描述:
如果是合法的数值表达则返回该数字，否则返回0

示例1 

输入
+2147483647
    1a33
    
输出
2147483647
    0

考点：综合

[牛客网](https://www.nowcoder.com/practice/1277c681251b4372bdef344468e4f26e?tpId=13&tqId=11202&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：500k
*/
class Solution {
public:
  int StrToInt(string str) 
  {
    int n = str.size();
    if(n == 0) return 0;
    int isNegative = str[0] == '-';
    int ans = 0;
    for(int i = 0;i < n;++i)
    {
      if(i == 0 &&(str[i] == '-' || str[i] == '+'))
        continue;
      if(str[i] < '0' || str[i] > '9')
        return 0;
      ans = ans*10 + str[i] - '0';
    }
    return isNegative ? -ans : ans;
  }
};
```

## 数组中重复的数字 

题目描述:
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 
数组中某些数字是重复的，但不知道有几个数字是重复的。
也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 
例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

考点：数组 

[牛客网](https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8?tpId=13&tqId=11203&rp=2&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

### 利用辅助数组

```
/** 
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) 
    {
        vector<int> assist(length,0);
        for(int i = 0;i < length;++i)
        {
          if(assist[numbers[i]] == 0)
            ++assist[numbers[i]];
          else
          {
            *duplication = numbers[i];
            return true;
          }
        }
        return false;
    }
};
```

### 利用原数组进行标记

```
/** 
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    bool duplicate(int numbers[], int length, int* duplication) 
    {
      int num;
      for(int i = 0;i < length;++i)
      {
        num = numbers[i];
        if(num < 0)
          num += length;
        if(numbers[num] < 0)
        {
          *duplication = num;
          return true;
        }
        else
          numbers[num] -= length;
      }
      return false;
    }
};
```

##  构建乘积数组

题目描述:
给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],
其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

考点：数组

[牛客网](https://www.nowcoder.com/practice/94a4d381a68b47b7a8bed86f2975db46?tpId=13&tqId=11204&tPage=3&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

参考[披萨大叔](https://www.nowcoder.com/profile/841505)的回答

B[i]=A[0]*A[1]*...*A[i-1]*1*A[i+1]*...*A[n-1]
分为下三角和上三角DP计算B

```
/**
* 运行时间：4ms
* 占用内存：512k
*/
class Solution {
public:
    vector<int> multiply(const vector<int>& A) 
    {
      int n = A.size();
      vector<int> B(n);
      if(n) B[0] = 1;
      //计算下三角
      for(int i = 1;i < n;++i)
        B[i] = A[i-1]*B[i-1];
      int tmp = 1;
      //计算上三角
      for(int i = n-2;i >= 0;--i)
      {
        tmp *= A[i+1];
        B[i] *= tmp;
      }
      return B;
    }
};
```

## 正则表达式匹配

题目描述:
请实现一个函数用来匹配包括'.'和'*'的正则表达式。
模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 
在本题中，匹配是指字符串的所有字符匹配整个模式。
例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

考点：字符串

[牛客网](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c?tpId=13&tqId=11205&tPage=3&rp=3&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

思路：动态规划

dp[i][j]表示str[0:i-1]与pattern[0:j-1]是否匹配

递推关系

对于当前字符s = str[i-1],p = pattern[j-1]

1. 如果 p == '.'，dp[i][j] = dp[i-1][j-1];
2. 如果 p == '*'
  
  - 如果 p[j-2] == '.' || p[j-2] == s[i-1] 

    dp[i][j] = dp[i][j-2] || dp[i][j-1] || dp[i-1][j];

  - 否则

    dp[i][j] = dp[i][j-2] || dp[i][j-1];

3. 其他 // p != '*' && p != '.'

  dp[i][j] = dp[i-1][j-1] // p == s
  dp[i][j] = false       // p != s


```
/**
* 运行时间：3ms
* 占用内存：504k
*/
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        string s = str;
        string p = pattern;
      int m = p.size();
      int n = s.size();
      vector<vector<bool> > dp(n+1,vector<bool>(m+1,false));
      // 初始化
      dp[0][0] = true;
      if(m >= 1 && p[0] == '*') dp[0][1] = true;
      for(int j = 2;j <= m;++j)
        if(p[j-1] == '*') dp[0][j] = dp[0][j-2];
        if(m >= 1 && n >= 1 && (p[0] == '.' || p[0] == s[0])) dp[1][1] = true;
        // dp
      for(int i = 1;i <= n;++i)
        for(int j = 2;j <= m;++j)
        {
          if(p[j-1] == '.' || p[j-1] == s[i-1]) dp[i][j] = dp[i-1][j-1]; 
          else if(p[j-1] == '*') 
                {
                    dp[i][j] = dp[i][j-2] || dp[i][j-1]; // x* 0次，1次匹配
                    if(p[j-2] == '.' || p[j-2] == s[i-1]) dp[i][j] = dp[i][j] || dp[i-1][j]; // 加上多次匹配
                }
        }
      return dp[n][m];
    }
};
```

## 表示数值的字符串

题目描述:
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。
例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 
但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

考点：字符串

[牛客网](https://www.nowcoder.com/practice/6f8c901d091949a5837e24bb82a731f2?tpId=13&tqId=11206&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

参考推荐答案

```
class Solution {
public:
    bool isNumeric(char* string)
    {
       // 标记符号、小数点、e是否出现过
        bool sign = false, decimal = false, hasE = false;
        for (int i = 0; i < strlen(string); i++) {
            if (string[i] == 'e' || string[i] == 'E') {
                if (i == strlen(string)-1) return false; // e后面一定要接数字
                if (hasE) return false;  // 不能同时存在两个e
                hasE = true;
            } else if (string[i] == '+' || string[i] == '-') {
                // 第二次出现+-符号，则必须紧接在e之后
                if (sign && string[i-1] != 'e' && string[i-1] != 'E') return false;
                // 第一次出现+-符号，且不是在字符串开头，则也必须紧接在e之后
                if (!sign && i > 0 && string[i-1] != 'e' && string[i-1] != 'E') return false;
                sign = true;
            } else if (string[i] == '.') {
              // e后面不能接小数点，小数点不能出现两次
                if (hasE || decimal) return false;
                decimal = true;
            } else if (string[i] < '0' || string[i] > '9') // 不合法字符
                return false;
        }
        return true;
    }

};
```

## 字符流中第一个不重复的字符

题目描述:
请实现一个函数用来找出字符流中第一个只出现一次的字符。
//例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。
//当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

输出描述:
如果当前字符流没有存在出现一次的字符，返回#字符。

考点：字符串

[牛客网](https://www.nowcoder.com/practice/00de97733b8e4f97a3fb5c680ee10720?tpId=13&tqId=11207&rp=3&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```
/**
* 运行时间：4ms
* 占用内存：512k
*/
class Solution{
public:
    Solution()
    {
        first = '#';
        for(int i = 0;i < 128;++i)
            cnt[i] = 0;
    }
  //Insert one char from stringstream
    void Insert(char ch)
    {
        if(++cnt[ch] == 1 && first == '#') first = ch;
        if(cnt[first] > 1)
        {
            int i = 0;
            for(;i < 128 && cnt[i] != 1;++i);
            if(i == 128) first = '#';
            else first = i;
        }
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
      return first;
    }
private:
  int cnt[128];
  char first;
};
```

##  链表中环的入口结点

题目描述:
一个链表中包含环，请找出该链表的环的入口结点。

考点：链表

[牛客网](https://www.nowcoder.com/practice/253d2c59ec3e4bc68da16833f79a38e4?tpId=13&tqId=11208&rp=3&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

hash 或者一快一慢两指针

需要两趟相遇，才能指向入口节点。

第一趟，两指针同时从表头开始移动，快指针速度是慢指针的两倍，
相遇时，慢指针位于环内，且距离入口节点的距离为mk-l个节点。
第二趟，两指针等速移动，新指针从表头开始，慢指针接着在环内移动。
再次相遇时，两指针都移动了l个节点，均指向了入口节点。

其中l为表头到入口节点的节点数，m为环内节点数,k为一正整数。

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
      if(!pHead || !pHead -> next) return NULL;
      ListNode* p = pHead,*q = p;
      /* 第一趟*/
      while(q -> next)
      {
        p = p -> next;
        q = q -> next -> next;
        if(p == q) break;
      }
      /* 第二趟*/
      q = pHead;
      while(p != q)
      {
        p = p -> next;
        q = q -> next;
      }
      return p;
    }
};
```

##  删除链表中重复的结点

题目描述:
在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，
重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 
处理后为 1->2->5

考点：链表

[牛客网](https://www.nowcoder.com/practice/fc533c45b73a41b0b44ccba763f866ef?tpId=13&tqId=11209&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(!pHead || !pHead -> next) return pHead;
      ListNode *p = pHead,*q = p,*head;
      int key;
        bool hasdelete;
      while(q && q -> next)
      {
            head = q;
        key = q -> val;
        q = q -> next;
        /*更新p指针的情况
        1. 删除了左侧非头节点
        2. 出现连续3个不相同的节点
        */
        if(q && key != q -> val)
        {
                if(hasdelete) 
                {
                    p = p -> next;
                    hasdelete = false;
                }
          key = q -> val;
          if(q -> next && (q -> next) -> val != key)
          {
            p = q;
            q = p -> next;
          }
        }
        /* 指针移过重复节点 */
        else //key == q -> val
        {
          for(;q && q -> val == key;q = q -> next);
                if(head == pHead) // 删除的是头节点
                {
                    pHead = q;
                    p = pHead;
                }
                else
                {   p -> next = q; // 删除重复节点
                    hasdelete = true; // 标记删除的是否为非头节点
                }
        }
      }
      return pHead;
    }
};
```

[枫叶依然](https://www.nowcoder.com/profile/672177)的递归思路

```
/**
* 运行时间：3ms
* 占用内存：512k
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(!pHead || !pHead -> next) return pHead;
        ListNode* current;
        if(pHead -> val == pHead -> next -> val)
        {
          current = pHead -> next -> next;
          while(current && current -> val == pHead -> val)
            current = current -> next;
          return deleteDuplication(current);
        }
        else
        {
          current = pHead -> next;
          pHead -> next = deleteDuplication(current);
          return pHead;
        }
    }
};
```

##  二叉树的下一个结点

题目描述:
给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。
注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

考点：树

[牛客网](https://www.nowcoder.com/practice/9023a0c988684a53960365b889ceaf5e?tpId=13&tqId=11210&rp=3&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```
/**
* 运行时间：3ms
* 占用内存：384k
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        if(!pNode) return pNode;
        TreeLinkNode *p = pNode -> right;
        if(p) while(p -> left) p = p -> left;
        else if(pNode -> next && pNode == (pNode -> next -> right)) 
        {
          TreeLinkNode * q = pNode -> next;
          if(q -> next && q -> next -> left == q)
            p = q -> next;
          else
            p = NULL;
        }
        else p = pNode -> next;
        return p;
    }
};
```

## 对称的二叉树

题目描述:
请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

考点：树

[牛客网](https://www.nowcoder.com/practice/ff05d44dfdb04e1d83bdbdab320efbcb?tpId=13&tqId=11211&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

递归

```
/**
* 运行时间：3ms
 *占用内存：512k
*/
class Solution {
public:
    bool isSymmetrical(TreeNode* pRoot)
    {
      if(!pRoot) return true;
      return areSymmetrical(pRoot -> left,pRoot -> right);
    }
private:
  bool areSymmetrical(TreeNode* left, TreeNode* right)
  {
    if(left == NULL && right == NULL) return true;
    return isSame(left,right) && areSymmetrical(left -> right,right -> left) && areSymmetrical(left -> left,right -> right);
  }
  bool isSame(TreeNode* t1, TreeNode* t2)
  {
    return t1 && t2 && t1 -> val == t2 -> val;
  }
};
```

##  按之字形顺序打印二叉树

题目描述:
请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，
第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

考点：树

[牛客网](https://www.nowcoder.com/practice/91b69814117f4e8097390d107d2efbe0?tpId=13&tqId=11212&rp=3&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

栈，奇数行先左子树入栈，后右子树入栈，偶数行相反。

```
class Solution {
public:
    vector<vector<int> > Print(TreeNode* pRoot) 
    {
        int row = 1;
        vector<vector<int> > ans;
        stack<TreeNode*> mystack;
        if(pRoot) mystack.push(pRoot);
        while(!mystack.empty())
        {
          vector<int> v;
          int cnt = mystack.size();
          while(cnt > 0)
          {
            TreeNode* p = mystack.top();
            mystack.pop();
            --cnt;
            v.push_back(p -> val);
            if(row % 2 == 1)
            {
              if(p -> left) mystack.push(p -> left);
              if(p -> right) mystack.push(p -> right);
            }
            else
            {
              if(p -> right) mystack.push(p -> right);
              if(p -> left) mystack.push(p -> left);
            }
          }
          ans.push_back(v);
          ++row;
        }
        return ans;
    }  
};
```

##  把二叉树打印成多行

题目描述:
从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

考点：树

[牛客网](https://www.nowcoder.com/practice/445c44d982d04483b04a54f298796288?tpId=13&tqId=11213&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
/**
* 运行时间：3ms
* 占用内存：400k
*/
class Solution {
public:
        vector<vector<int> > Print(TreeNode* pRoot) 
        {
          vector<vector<int> > ans;
          queue<TreeNode*> myqueue;
          if(pRoot) myqueue.push(pRoot);
          while(!myqueue.empty())
          {
            int cnt = myqueue.size();
            vector<int> v;
            while(cnt > 0)
            {
              TreeNode * p = myqueue.front();
              myqueue.pop();
              --cnt;
              v.push_back(p -> val);
              if(p -> left) myqueue.push(p -> left);
              if(p -> right) myqueue.push(p -> right);
            }
            ans.push_back(v);
          } 
          return ans; 
        }
    
};
```

## 序列化二叉树

题目描述:
请实现两个函数，分别用来序列化和反序列化二叉树

考点：树

[牛客网](https://www.nowcoder.com/practice/cf7e25aa97c04cc1a68c8f040e71fb84?tpId=13&tqId=11214&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

这是一道开放题，只要能够唯一确定一棵树的序列化方法都可以。
可以用层序遍历或者先序遍历序列化二叉树，
空节点用'#'表示，节点间用','分隔。

```
// 内存超限:您的程序使用了超过限制的内存
class Solution {
public:
    char* Serialize(TreeNode *root) 
    {    
        string str = SerializeHelper(root);
        // string 转 char[]
        int n = str.length();
        char * cstr = new char[n+1];
        str.copy(cstr,n,0);
        cstr[n] = '\0';
        return cstr;

    }
    TreeNode* Deserialize(char *str) 
    {
      // char[] 转 string
      string Str = "";
      Str += str;
      int start_pos = 0;
      return DeserializeHelper(Str,start_pos);
    }
private:
  string SerializeHelper(TreeNode *root)
  {
    if(!root) return "#";
    return to_string(root -> val) + "," 
          + SerializeHelper(root -> left) 
          + SerializeHelper(root -> right);
  }
  TreeNode* DeserializeHelper(string str,int& pos) 
    {
      if(str.size() == pos) return NULL;
      int index = str.find(",",pos);
      if(pos == -1) return NULL;
      int length = index - pos + 1;
      pos = index + 1;
      string sub = str.substr(pos,length);
      if(sub == "#") return NULL;
      int val = atoi(sub.c_str());
      TreeNode* t = new TreeNode(val);
      t -> left = DeserializeHelper(str,pos);
      t -> right = DeserializeHelper(str,pos);
      return t; 
    }
};
```

##  二叉搜索树的第k个结点

题目描述:
给定一颗二叉搜索树，请找出其中的第k大的结点。
例如， 5 / \ 3 7 /\ /\ 2 4 6 8 中，
按结点数值大小顺序第三个结点的值为4。

考点：树

[牛客网](https://www.nowcoder.com/practice/ef068f602dde4d28aab2b210e859150a?tpId=13&tqId=11215&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

中序遍历到第k个值时输出

```
/**
* 运行时间：4ms
* 占用内存：396k
*/
class Solution {
public:
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
      if(pRoot)
        {
            if(pRoot -> left) KthNode(pRoot -> left,k);
          if(++cnt == k) ans = pRoot;
            if(pRoot -> right) KthNode(pRoot -> right,k); 
        }
        return ans;
    }
private:
  int cnt = 0;
    TreeNode* ans = NULL;
};
```

## 数据流中的中位数

题目描述:
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，
那么中位数就是所有数值排序之后位于中间的数值。
如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

考点：树

[牛客网](https://www.nowcoder.com/practice/9be0172896bd43948f8a32fb954e1be1?tpId=13&tqId=11216&rp=3&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```
class Solution {
public:
    void Insert(int num)
    {
        
    }

    double GetMedian()
    { 
    
    }

};
```