Given a singly linked list, determine if it is a palindrome(回文).

Follow up:
Could you do it in O(n) time and O(1) space?

Approach #1: vector O(n)-O(n)
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
    bool isPalindrome(ListNode* head) 
    {
        vector<int> array;
        ListNode* p = head;
        if(p == NULL)
            return true;
        while(p != NULL)
        {
          array.push_back(p -> val);
          p = p -> next;
        }
        for(int i=0; i <= (array.size()-1)/2;i++)
        {
          if(array[i] != array[array.size()-1-i])
            return false;
        }
        return true;
    }
};
```