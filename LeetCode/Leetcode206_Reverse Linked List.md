Reverse a singly linked list.

Hint:
A linked list can be reversed either iteratively or recursively. Could you implement both?

三指针，递归
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) 
    {
      ListNode* p = head;
      ListNode* q;
      ListNode* s;

      if(p == NULL)
        return NULL;

      q = p -> next;
      head -> next = NULL;

      while(q != NULL)
      {
        s = q ->next;
        q -> next = p;
        p = q;
        q = s;
      }
      return p;
    }
};
```


迭代
```
/**
* 假设从第二个节点到最后的节点已经完成倒叙
*/
class Solution {
public:
    ListNode* reverseList(ListNode* head) 
    {
      if(head == NULL || head -> next == NULL) return head;
      ListNode* p = reverseList(head -> next);
      (head -> next) -> next = head;
      head -> next = NULL;
      return p;
    }
};
```