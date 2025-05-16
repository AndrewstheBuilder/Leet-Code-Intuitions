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
String Concatenation solution. And above figure out sqrt(n) time complexity.
Mathematical Proof: 
If we concatenate s twice, i.e., form another string t where t = s + s and remove the first and last character from it, t would look like t = (head + p * (k - 1)) + (p * (k - 1) + tail) where head is p without first character, tail is p without last character and p * (k - 1) denotes p repeated k - 1 times.

As a result, t = head + p * (2k - 2) + tail. We now have 2 possibilities: either k = 1 and the answer is false, or k > 1 and the answer is true.
'''
"avcavc" -> "avcavcavcavc" -> t[1:-1] = "vcavcavcav"
"avcavcs" -> "avcavcsavcavcs" -> t[1:-1] = "vcavcsavcavc"
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        t = s + s
        if s in t[1:-1]:
            return True
        return False
```
* [Repeated Pattern Matching]()
```python
'''
Recursive Solution. There is also a dynamic programming approach LOL. The below solution is not accepted.
'''
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        if not p:
            return not s
        first_match = bool(s) and p[0] in {s[0], '.'}
        if len(p)>1 and p[1]=="*":
                return self.isMatch(s,p[2:]) or (first_match and self.isMatch(s[1:], p))
        return first_match and self.isMatch(s[1:], p[1:])
```
- [Word Subsets](https://leetcode.com/problems/word-subsets/)
```python
'''
Dumb solution that does not work!!!
Each w2 in words2 needs to be a subset of the universal word. Here I am accumulating all the character counts of words2 which is incorrect
'''
class Solution:
    def wordSubsets(self, words1: List[str], words2: List[str]) -> List[str]:
        word2_dict = defaultdict(int)
        for word in words2:
            for char in word:
                word2_dict[char] += 1
        # print('word2_dict',word2_dict)
        ans = [] # list of all universal words from words1
        for word in words1:
            word_char = Counter(word)
            universal=True
            # print('word_char',word_char)
            for key, value in word2_dict.items():
                if word_char[key] < value:
                    universal=False
            if universal:
                ans.append(word)
        return ans
```
```python
'''
Accepted Solution.
Let N represent the total information in words1
Let X represent the total information in words2
Time Complexity: O(X+N)
Space Complexity: O(1) because although a new datastructure is created X and N times. Its in a for loop and does not persist across the iterations of the loop! The max size of that datastructure is a constant number => 26.
'''
class Solution:
    def wordSubsets(self, words1: List[str], words2: List[str]) -> List[str]:
        ans = []
        w2_ds = defaultdict(int)
        for w2 in words2:
            w2_d = Counter(w2)
            for key, value in w2_d.items():
                w2_ds[key] = max(value, w2_ds[key])
        for w1 in words1:
            universal=True
            w1_d = Counter(w1)
            for key, value in w2_ds.items():
                if w1_d[key]<value:
                    universal=False
                    break
            if universal:
                ans.append(w1)
        return ans
```