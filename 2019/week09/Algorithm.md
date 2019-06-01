# [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```
class Solution(object):
	def longestCommonPrefix(strs):
	    """
	    :type strs: List[str]
	    :rtype: str
	    """
	    result = ""
	
	    if len(strs) == 0:
	        return result
	    
	    firstItem = strs[0]
	    for index in xrange(0, len(firstItem)):
	        char = firstItem[index]
	        for j in xrange(1, len(strs)):
	            item = strs[j]
	            if index < len(item) and item[index] == char:
	                if j == len(strs)-1:
	                    result += char
	                continue
	            else:
	                return result
	
	    return result
```
