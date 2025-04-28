- [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)
```python
'''
# My inefficient solution
# TODO figure out the efficient solution
# aba
# r=2, 'aba', i=3, length=2
'''
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
'''
Time: O(n*sqrt(n)) for loop then inner part of creating a string.
  - A number can have maximum of 2*sqrt(n) divisors that gives a whole number as a quotient with no remainders. As a result we would execute the inner loop that concatenates the substring O(sqrt(n)) times. *hard realization lmao*
Space: O(n) for creating new string
Efficient solution. Recognize the substring we are looking for has to be a prefix of s and must be a divisor that goes into the length of s with no remainders. That we can generate what s should be using the substring.

Solution:
# substring must be a prefix of s
# substring must be a length that is a divisor of s with no remainders
'''
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        for i in range((n//2)):
            length = i+1
            if(n%length==0):
                # no remainders for length
                quotient = int(n/length)
                if s[:length]*quotient==s:
                    return True
        return False
'''
TODO: String Concatenation solution. And above figure out sqrt(n) time complexity.
'''
```

