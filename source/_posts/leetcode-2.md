---
title: leetcode 2. Add Two Numbers
date: 2017-06-11 23:28:41
tags: leetcode
---

#### Question

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

#### Example

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

#### Tip

两个链表相加，返回相加后的链表

需要考虑的点有

- 两个链表可能并不是一样长的
- 相加的最后一个节点的进位情况，如有进位，则在结果链表后添加一个节点

#### Code

```C++
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(-1);
        ListNode *p1, *p2, *p3 = head;
        int lastNum = 0;
        for(p1 = l1, p2 = l2; p1 != NULL && p2 != NULL; p1 = p1 -> next, p2 = p2 -> next) {
            int result = p1 -> val + p2 -> val;
            p3 -> next = new ListNode((result + lastNum) % 10);
            p3 = p3 -> next;
            lastNum =  result + lastNum >= 10 ? (result + lastNum) / 10 : 0;
        }


        ListNode *p4 = p1 == NULL ? p2 : p1;

        if(p4 != NULL) {
            while(p4 != NULL) {
                p3 -> next = new ListNode((p4 -> val + lastNum) % 10);
                lastNum =  (p4 -> val + lastNum) >= 10 ? (p4 -> val + lastNum) / 10 : 0;
                p3 = p3 -> next;
                p4 = p4 -> next;
            }
        }

        if(lastNum != 0) {
            p3 -> next = new ListNode(lastNum);
        }



        return head -> next;
    }
};
```
