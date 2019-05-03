
设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。  
示例：
```java
int k = 3;  
int[] arr = [4,5,8,2];  
KthLargest kthLargest = new KthLargest(3, arr);  
kthLargest.add(3);   // returns 4  
kthLargest.add(5);   // returns 5  
kthLargest.add(10);  // returns 5  
kthLargest.add(9);   // returns 8  
kthLargest.add(4);   // returns   
```
思路一：维护一个长度为k的数组，把前k大数据放到里面，每次新数据来的时候，如果比最小的还小，就不管，不然就把最小的去除，把新数据加进数组并进行一次排序，输出最小值 单次操作时间复杂度是：$$O(Klogk)$$  
思路二：维护一个长度为k的优先级队列（堆），Python中heapq模块实现了小顶堆，是通过列表来实现的，却不能建立固定长度的堆，所以只能通过列表的长度来判断堆的大小。单次操作的时间复杂度是：$$O(logk)$$


```python
class KthLargest(object):
    
    import heapq
    
    def __init__(self, k, nums):
        """
        :type k: int
        :type nums: List[int]
        """
        self.heap = nums
        self.k = k
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)
        

    def add(self, val):
        """
        :type val: int
        :rtype: int
        """
        if len(self.heap) < self.k:
            heapq.heappush(self.heap, val)
        elif val > self.heap[0]:
            heapq.heapreplace(self.heap, val)
        return self.heap[0]


# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```
