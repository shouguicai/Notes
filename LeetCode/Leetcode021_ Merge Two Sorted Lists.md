Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) 
    {
        ListNode* pos1 = l1;
        ListNode* pos2 = l2;
        ListNode l3(0);
        ListNode* pos3 = &l3;
        
        while(pos1 != NULL || pos2 != NULL)
        {
            if(pos1 != NULL && pos2 != NULL && pos1->val <= pos2->val)
            {
                pos3->next = pos1;
                pos1 = pos1->next;
            }
            else if(pos1 != NULL && pos2 != NULL && pos1->val > pos2->val)
            {
                pos3->next = pos2;
                pos2 = pos2->next;
            }
            else
            {
                pos3->next = (pos1 == NULL ? pos2 : pos1);
                return l3.next;
            }
            pos3 = pos3->next;
        }
        return l3.next;
    }
};
```