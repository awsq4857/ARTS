# [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

```
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s)<1:
            return 0
        max_length = 0
        tempStr = s[0]
        index = 0
        while index < len(s):
            char = s[index]
            j = index + 1;
            index+=1;
            if j < len(s):
                nextChar = s[j];
                if nextChar != char and tempStr.find(nextChar)==-1:
                    tempStr += nextChar;
                else:
                    if max_length < len(tempStr):
                        max_length = len(tempStr)
                    index = j
                    tempStr = ""
        return max_length
```
