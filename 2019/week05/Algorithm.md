# [ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)

> 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。你的输出需要从左往右逐行读取，产生出一个新的字符串。

```
def convert(s, numRows):
    """
    :type s: str
    :type numRows: int
    :rtype: str
    """
    # LEETCODEISHIRING,3,LCIRETOESIIGEDHN
    # LEETCODEISHIRING,4,LDREOEIIECIHNTSG
    unitNum = numRows + numRows - 2
    units = len(s) / unitNum
    result = list(s)
    for i in xrange(0,len(s)):
    	unit = i / unitNum
    	mod = i % unitNum
    	if mod <= unitNum/2:
    		if mod == 0:
    			result[unit] = s[i]
    		elif mod == unitNum/2:
    			result[units*(unitNum-1)+unit]=s[i]
    		else:
    			result[units + 2*units*(mod-1) + 2*unit+mod-1]=s[i]
    			index = unit*unitNum + (unitNum-mod)
    			result[units + 2*units*(mod-1) + 2*unit+mod]=s[index]
    print result
```
