
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。  
使用栈，遇到左括号就压栈，遇到右括号就与栈顶元素匹配。  
字符串长度是奇数时；遍历完字符串后栈非空时；右括号与栈顶不匹配时，返回False


```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if len(s)%2:
            return False
        stack = []
        dyct = {')':'(', ']':'[','}':'{'}
        for i in s:
            if i not in dyct:
                stack.append(i)
            elif not stack or dyct[i] != stack.pop():
                return False
        return not stack
```
暴力解法：  
```python
class Solution(object):
    def isValid(self, s):
        while '()' in s or '[]' in s or '{}' in s:
	    s.replace('()', '').replace('[]', '').replace('{}', '')
	return len(s) == 0
```
