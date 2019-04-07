# [Two Sum](https://leetcode-cn.com/problems/two-sum/)

第一次打卡，算法题做个简单一点的。语言使用Python，不过时间复杂度有点高。


```Python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for i in range(len(nums)-1):
            for j in range(i, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```


