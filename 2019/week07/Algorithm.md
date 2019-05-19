# [Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

```
class Solution(object):
    def intToRoman(num):
	    """
	    :type num: int
	    :rtype: str
	    """
	    trans_relation = [{"value":"I", "check":True, "divisor":1},
	    				{"value":"V", "check":False, "divisor":5},
	    				{"value":"X", "check":True, "divisor":10},
	    				{"value":"L", "check":False, "divisor":50},
	    				{"value":"C", "check":True, "divisor":100},
	    				{"value":"D", "check":False, "divisor":500},
	    				{"value":"M", "check":False, "divisor":1000}]

	    result = ""
	    for i in xrange(0,len(trans_relation)):
	    	index = len(trans_relation) - 1 - i
	    	divisor = trans_relation[index]["divisor"]
	    	while num/divisor:
	    		if trans_relation[index]["check"] and num/divisor == 4:
	    			num = num - 4*divisor
	    			# print result
	    			if len(result) > 0 and result[len(result)-1] == trans_relation[index+1]["value"]:
	    				result = result[0:len(result)-1] + trans_relation[index]["value"] + trans_relation[index+2]["value"]
	    			else:
	    				result = result + trans_relation[index]["value"] + trans_relation[index+1]["value"]
	    		else:
	    			num = num - divisor
	    			result = result + trans_relation[index]["value"]

	    return result
```
