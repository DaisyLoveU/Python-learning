
判断链表是否有环

使用快慢双指针，当两个指针重合时即存在环；  
当快指针指向None时，不存在环。


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        fast = slow = head
        while fast and slow and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast is slow:
                return True
        return False
```
