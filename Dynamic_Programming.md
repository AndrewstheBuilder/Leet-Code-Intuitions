1. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/?envType=company&envId=amazon&favoriteSlug=amazon-thirty-days)
```python
class Solution:
    # seems to be a recursive problem
    # we can optimize after solving it recusively by making it iterative
    # s = "rabbbit", t = "rabbit"
    # what are the base cases and subproblems to optimize?
    # * I really have to solve the problem first in a simple, algorithmic way before breaking it down into subproblems and base cases
    # if there is a match we can make two choices
    # func(i, j) = func(i + 1, j) + func(i + 1, j + 1) # ignore the match or add the match and move the pointer forward on both strings
    # if there is no match, there is only one choice
    # func(i, j) = func(i + 1, j)
    #  So, i == M or j == N will form our base case. However, what we return in our base case is what will tie this whole thing together.
        # we should return a count in the base case. we could return True or False for if we found t in s or not.
        # return True
        # func(i, j) = True
        # if True: count += 1
        # formed_str="rabbb", source_of_truth="rabbit", pointer_to_options=4, we compare [length-1] of formed_str to source_of_truth
            # pointer 4 disagrees so ignore that index (4) and move to next index
            # "rabbi"
            # "rabbit"
            # one path = [0,1,2,3,5,6]
            # how can I ignore indexes with for loops -> just skip it
                # How can I make recursion calls to find all possible ways to generate source_of_truth
                # What are the subproblems to solve?
    # A subsequence is a string that can be formed in order by deleting different characters inside the provided options.
    # - the base case is when there is only 1 character in the formed_string? No!
    # "r", curr_index=0 -> how many distinct ways are there to form "r" 1
    # "ra",  -> 1
    # "rab" -> 3
    # "rabb" -> 2
    def numDistinct(self, s: str, t: str) -> int:
```
- Recursive DP Solution
``` python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:

        # Dictionary for memoization
        memo = {}

        def uniqueSubsequences(i: int, j: int) -> int:

            M, N = len(s), len(t)

            # Base case
            if i == M or j == N or M - i < N - j:
                return int(j == len(t))

            # Check if the result is already cached
            if (i, j) in memo:
                return memo[i, j]

            # Always make this recursive call
            ans = uniqueSubsequences(i + 1, j)

            # If the characters match, make the other
            # one and add the result to "ans"
            if s[i] == t[j]:
                ans += uniqueSubsequences(i + 1, j + 1)

            # Cache the answer and return
            memo[i, j] = ans
            return ans

        return uniqueSubsequences(0, 0)
```
- My recursive DP Solution:
```python
class Solution:
    # s="tom", t="to"
    # M=3, N=2
    # i=0, j=0 return recurse(0,0)==1 -> return ans=1 (1,0) return 0 + (1,1) return 1 -> (2,0) return ans=0 -> (3,0) return ans=0 -> return 0
    # -> (1,1) -> (2,1) return ans=0 + (2,2) return 1 -> return 1
    # -> (2,1) return ans=0 -> (3,1) -> return 0
    def numDistinct(self, s: str, t: str) -> int:
        M = len(s)
        N = len(t)
        def recurse(i,j):
            if(i==M or j==N):
                # if we are at the end of t we found a subsequence
                return 1 if(j==N) else 0
            ans = 0
            if(s[i] == t[j]):
                # if a character matches you can skip it in s
                # or you can move pointers for s and t forward
                ans += recurse(i+1, j) + recurse(i+1, j+1)
            else:
                # characters do not match
                # skip that character in s
                ans += recurse(i+1, j)
            return ans
        return recurse(0,0)
```
- TODO: make the iterative solution for this problem from the recursive calls and their results used above. We will mimic the recursive calls by using a 2D matrix.
        
