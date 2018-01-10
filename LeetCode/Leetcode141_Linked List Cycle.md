Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

对地址按位或 14/16 成圈处节点的地址将是第一个重复出现的地址 有点类似No.136 Single Number 
```
/* 可能会出现 b|c == a 的情况，导致出错
  a|b = b|a
  a|a = a
  a|b|a = a|a|b = a|b
Submission Result: Wrong Answer
Input:
[-21,10,17,8,4,26,5,35,33,-7,-16,27,-12,6,29,-12,5,9,20,14,14,2,13,-24,21,23,-21,5]
no cycle
Output:true
Expected:false
*/
class Solution {
public:
    bool hasCycle(ListNode *head) 
    {
        ListNode *p = head;
        unsigned long int result = (unsigned long int) p;
        while(p != NULL && p ->next != NULL)
        {
            p = p->next;
            if(result == (result|(unsigned long int)p))
                return true;
            result |= (unsigned long int) p;
        }
        return false;
    }
};
```

```
//修改 14/16
class Solution {
public:
    bool hasCycle(ListNode *head) 
    {
        ListNode *p = head;
        unsigned long int result = (unsigned long int) NULL;
        while(p != NULL)
        {
            if(result == (result|(unsigned long int)p))
                return true;
            result |= (unsigned long int) p;
            p = p -> next;
        }
        return false;
    }
};
```

Hash表，检查当前节点是否已经访问过
```
//  22 ms
class Solution {
public:
    bool hasCycle(ListNode *head) 
    {
        ListNode *p = head;
        unordered_set <ListNode*>  myset;
        myset.insert(p);
        while( p != NULL && p -> next != NULL)
        {
            p = p -> next;
            if(myset.find(p) != myset.end())
                return true;
            myset.insert(p);
        }
        return false;
    }
};
```

两个指针，快的(速度2)在前，慢的(速度1)在后，如果存在圈，
快的最终会追上慢的。
```
// 9 ms
class Solution {
public:
    bool hasCycle(ListNode *head) 
    {
        ListNode *p = head;
        ListNode *q = p;
        while( q != NULL && q -> next != NULL) // 因为q的速度快，只需要检查q就可以了
        {
            p = p -> next;
            q = q -> next;
            if(q -> next == NULL)
                return false;
            q = q -> next;
            if( p == q)
                return true;
        }
        return false;
    }
};
```