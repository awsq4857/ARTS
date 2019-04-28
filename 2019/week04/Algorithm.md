# [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

```
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        if len(s)<1:
            return ""
        max_str = ""
        tempStr = ""
        tempIndex = 0
        flag = 0
        startIndex = 0
        index = startIndex
        while index < len(s):
            char = s[index];
            index += 1;
            findIndex = tempStr.find(char);
            if findIndex == -1 and not flag:
                tempStr += char;
            else:
                if not flag:
                    flag = True
                    distance =  len(tempStr) - findIndex
                    if distance <= 2:
                        tempIndex = len(tempStr) - distance -1
                        tempStr += char
                        if tempIndex < 0 and len(tempStr)>len(max_str):
                            max_str = tempStr
                            tempStr = ""
                            startIndex +=1
                            index = startIndex
                            tempIndex = 0
                            flag = False
                        continue
                    else:
                        tempStr = ""
                        tempIndex = 0
                        flag = False
                else:
                    if tempIndex == findIndex:
                        if tempIndex == 0:
                            if len(tempStr)>len(max_str):
                                max_str = tempStr
                            tempStr = ""
                            startIndex +=1
                            index = startIndex
                            tempIndex = 0
                            flag = False
                        else:
                            tempIndex -= 1
                    else:
                        tempStr = ""
                        startIndex +=1
                        index = startIndex
                        tempIndex = 0
                        flag = False
        return max_str
```
