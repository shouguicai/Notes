You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* ptr1 = l1;
        ListNode* ptr2 = l2;
        ListNode* ptr3 = l1;
        int jin_wei = 0,add_sum;
        for (;(ptr1 != NULL)||(ptr2 != NULL);)
        {
            add_sum = (ptr1 != NULL ? ptr1->val : 0) 
                    + (ptr2 != NULL ? ptr2->val : 0)
                    + jin_wei;
            ptr3->val = add_sum % 10;
            jin_wei = add_sum / 10;
            ptr1 = (ptr1 != NULL ? ptr1->next: NULL);
            ptr2 = (ptr2 != NULL ? ptr2->next: NULL);
            if(ptr1 == NULL)
            {
                ptr3->next = ptr2;
            }
            ptr3 = (ptr1 != NULL ? ptr1: (ptr2 != NULL ? ptr2: ptr3));
        }
        if(jin_wei != 0)
        {
            ptr3->next = l2;
            l2->val = 1;
            l2->next = NULL;            
        }
        return l1;
    }
};
```