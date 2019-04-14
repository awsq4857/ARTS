# [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

算法还有点小问题

```
class ListNode(object):
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        print "-"*80
        length1 = self.getListLength(l1)
        length2 = self.getListLength(l2)
        headList = l1
        tailList = l2
        length = length1 - length2
        if length1 < length2:
            headList = l2
            tailList = l1
            length = -length
        result = ListNode(-1)
        head = ListNode(-1)
        head.next = result
        flag = 0
        while(headList and headList.next):
            value1 = headList.val
            if length > 0:
                length = length -1
            else:
                value = value1 + tailList.val + flag
                flag = value / 10
                node = ListNode(-1)
                result.val = value % 10 
                result.next = node
                result = node
                tailList = tailList.next
            headList = headList.next
        if headList:
            value = headList.val + tailList.val + flag
            node = ListNode(-1)
            result.val = value % 10 
            result.next = node
            result = node
            if value > 10:
                node = ListNode(-1)
                result.val = 1
                result.next = node
                result = node
        if result.next and result.next.val == -1:
            result.next = None
        self.printList(l1)
        self.printList(l2)
        self.printList(head.next)
        return head.next
        
    def getListLength(self, lt):
        length = 0
        if not lt:
            return length
        while(lt):
            length = length + 1
            lt = lt.next
        return length
    
    def printList(self, lt):
        if not lt:
            return ""
        string = ""
        while(lt):
            string = string + " -> " + str(lt.val)
            lt = lt.next
        print string
```

