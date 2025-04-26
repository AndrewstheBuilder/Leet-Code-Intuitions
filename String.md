- [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)
```python
# My inefficient solution
# TODO figure out the efficient solution
# aba
# r=2, 'aba', i=3, length=2
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        l=0
        for r in range(len(s)):
            length = r-l+1
            i=r+1
            if r+1==len(s):
                return False
            while i<len(s) and i+length<=len(s):
                if s[l:r+1] != s[i:i+length]:
                    break
                i+=length
            if i==len(s):
                return True
        return False
```
