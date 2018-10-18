### LeetCode2 Add Two Numbers 
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.  

You may assume the two numbers do not contain any leading zero, except the number 0 itself.  

Example:  

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)  
Output: 7 -> 0 -> 8  
Explanation: 342 + 465 = 807.  

**思路一:**  
定义两个指针head,now指向同一个新的结点，temp保存进位、l1.val与l2.val的和，用temp与10的商更新temp，余数添加至now的下一个结点  
注意考虑l1 l2是否为空结点，以及进位temp不为0的情况
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
**结果：**  
![](./img/leetcode2_res_1.png)

**思路二：**  
比较优雅的代码，
```Python
class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        now = head = ListNode(0)
        
        temp = 0
        while l1 or l2 or temp:
            temp = (temp + l1.val) if l1 else temp
            temp = (temp + l2.val) if l2 else temp
            temp,mod = divmod(temp, 10)
            
            now.next = ListNode(mod)
            now = now.next
            
            l1 = l1.next if l1 else l1
            l2 = l2.next if l2 else l2
        return head.next
```
**结果：**  
![](./img/leetcode2_res_2.png)  
**分析：**  
时间/空间复杂度均为$O(max(l1,l2))$
