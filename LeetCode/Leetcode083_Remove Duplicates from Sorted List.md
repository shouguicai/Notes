Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.

非递归解
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) 
    {
      if (!head) return 0;
      if (!head -> next) return head;

      ListNode* first = head;
      ListNode* second;
      while (first != NULL && first -> next != NULL)
      {
        second = first -> next;
        while(second != NULL && second -> val == first -> val)
          second = second -> next;
        first -> next = second;
        first = second;
      }
     return head;
    }
};
```