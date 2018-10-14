```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None                
class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        head = ListNode(0)
        now = head
        temp = 0
        while 1:
            if l1 is not None:
                temp += l1.val
                l1 = l1.next
            if l2 is not None:
                temp += l2.val
                l2 = l2.next
            temp,now.val = divmod(temp,10)
            if l1 is not None or l2 is not None or temp != 0:
                now.next = ListNode(0)
                now = now.next
            else:
                break
        return head
```
