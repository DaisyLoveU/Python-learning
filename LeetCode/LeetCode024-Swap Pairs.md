
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。  

**示例：**  
给定 1->2->3->4, 你应该返回 2->1->4->3.  

**思路一：递归**  
第一步，找出递归终止条件。明显是当前节点为空或下一节点为空时，终止。  
第二步，确定每层递归的返回值。每层返回给上一层的应该是已完成交换后的子链表。  
第三步，实现单次递归要做的任务。假设每次需要交换的节点是head和tmp，tmp应该接收上一层递归返回的子链表。


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head):
        if not head or not head.next:
            return head
        tmp = head.next
        head.next = self.swapPairs(tmp.next)
        tmp.next = head
        return tmp
```

**思路二：迭代**  



```python
class Solution:
    def swapPairs(self, head):
        pre = dummy = ListNode(0)
        pre.next = head
        while pre.next and pre.next.next:
            a = pre.next
            b = pre.next.next
            pre.next, b.next, a.next = b, a, b.next
            pre = a
        return dummy.next
```
