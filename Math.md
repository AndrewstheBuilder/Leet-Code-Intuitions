1. [Build Array from Permutation](https://leetcode.com/problems/build-array-from-permutation/description/?envType=company&envId=amazon&favoriteSlug=amazon-six-months)
- the optimization to do in place modifications here is sick
```python
# Input: nums = [0,2,1,5,3,4]
# nums[0] = 0
# [0,8,13,29,33,22]
# [0,1,2,4,5,3]

# 
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        n = len(nums)
        # encoding
        # we want to store what nums[i] was previously => (nums[nums[i]] % n)
        # addings nums[i] -> currValue and newValue*n means when we do %n we get i's prevValue
            # it also means we get the newValue by doing //n to reverse the *n and chop of the + nums[i] that was used to retain the previous value
            # an index i is only modified once in encoding so we do not have to worry about more than 1 impact of these calculations
        # and the new value for nums[i]
        # nums[i] is the current value which is added to the newValue
	        # the current value is added as a way to encode it and retreive it later on as i's original value by doing %n
        # the newValue is retrieved by doing %n to get the original value at that location. 
        # we encode with *n to be able to retain the previous value by doing %n and 
        # the newValue by doing //n which chops of the encoded original value.
        for i in range(len(nums)):
            nums[i] = nums[i] + (nums[nums[i]] % n) * n
        
        # Decode the values
        for i in range(n):
            nums[i] = nums[i] // n
        return nums
```
2. Binary Exponentiation - A Faster way to exponentiate. O(n) is the brute force. O(logn) is the time optimal solution. Its a clever solution that waits to update the output when n is odd. This is not something I could think up. I tried various configurations based on the ideas I knew and was lost lol. The idea of binary exponentiation itself is not very intuitive and its very clever.
```python
class Solution:
    # init: n=5
    # x = 5, n=1
    # expected: 3125
    # output=1*1*5*5*5*5*5
    def myPow(self, x: float, n: int) -> float:
        # solve it iteratively
        # binary exponentiation
            # x^4 = (x*x)^2
            # x^5 = x*(x*x)^2
        # x^1
        # return x

        # x^2
        # x*x, 2//=2 => 1, 1*x*x, 1-=1 return x*x

        # x^3
        # output = 1*x*x*x, n=0, x=x*x*(x*x)
        # return output

        if n==0:
            return 1
        # preprocessing for negative exponents
        if n<0:
            x = 1/x
            n = n*-1
        output = 1
        while n != 0:
            if n%2==1:
                output *= x
                n=(n-1)
            x *= x
            n //= 2
        return output
```
3. [Count Good Numbers](https://leetcode.com/problems/count-good-numbers/description/?envType=company&envId=amazon&favoriteSlug=amazon-six-months)
```python
# Chat GPT Solution
class Solution:
    def _modexp(self, base: int, exp: int, mod: int) -> int:
        """
        Computes (base^exp) % mod using fast exponentiation
        (a.k.a. exponentiation by squaring) in O(log exp) time.
        """
        result = 1
        base %= mod  # Make sure base is under mod
        while exp > 0:
            if exp % 2 == 1:      # Chat GPT Code: If the current bit is 1 (odd exponent) ```exp & 1```
                result = (result * base) % mod
            base = (base * base) % mod
            exp //= 2 # 1
        return result

    def countGoodNumbers(self, n: int) -> int:
        """
        We know:
          - Positions at even indices (0, 2, 4, ...) can take 5 possible digits: [0, 2, 4, 6, 8].
          - Positions at odd indices  (1, 3, 5, ...) can take 4 possible digits: [2, 3, 5, 7].

        For n digits:
          - If n is even, # of even positions = n/2, # of odd positions = n/2
              => total ways = 5^(n/2) * 4^(n/2) = (5*4)^(n/2) = 20^(n/2).
          - If n is odd,  # of even positions = (n+1)/2, # of odd positions = (n-1)/2
              => total ways = 5^((n+1)/2) * 4^((n-1)/2)
              => 5 * 20^((n-1)//2), because 5 * 4 = 20 when you pair an even and an odd index.

        We just need to compute these values modulo (10^9 + 7).
        """
        mod = 10**9 + 7
        # Edge case
        if n == 0:
            return 0  # Based on your problem statement, or handle as needed

        if n % 2 == 0:
            # Even n => 20^(n//2) mod
            return self._modexp(20, n // 2, mod)
        else:
            # Odd n => 5 * 20^((n-1)//2) mod
            return (5 * self._modexp(20, (n - 1) // 2, mod)) % mod

# my solution
class Solution:
    # 1 -> even
    # 2 -> even, odd
    # 3 -> even, odd, even
    # brute force get each position as being even or odd

    # prime_numbers=[2,3,5,7]
    # even_numbers=[0,2,4,6,8]
    # solution from binary exponentiation
    def myPow(self, x: float, n: int, mod:int) -> int:
        if n==0:
            return 1
        output = 1
        while n != 0:
            if n%2==1:
                output = (output*x)%mod
                n=(n-1)
            x = (x*x)%mod
            n //= 2
        return output

    def countGoodNumbers(self, n: int) -> int:
        if n==0:
            return 0
        mod = (10**9 + 7)
        total_len = 1
        ans = 1
        if n%2 == 0:
            total_len = n//2
            ans = self.myPow(20, total_len, mod)
        else:
            total_len = (n-1)//2
            ans = self.myPow(20, total_len, mod)
            ans = ans*5%mod
        return ans
```
- Math And Linked List
	- The key to solving this problem is recognizing the carry could only be 1 or 0. 999 + 999 notice the largest number generated even with a carry and the largest two digits possible is 19. This handles the carry over problem
	- The linked lists are in reverse order which means we can start at the head and work our way to the tail. With manual addition we start with the ones place and go on to higher places.
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

# go forwards on both linked list and establish a prev connection to previous node. 
# do manual addition like normal, matching up the numbers place. Carrying the carry over to the next number

# 8000 :2
# 900 :2
# result 8900

# 8090 :2
# 950 :2
# carry = 1  ---> how do I select the number in the ones place when the result has two digits
# result 894

# above implementation is wrong the numbers being backwards is actually an asset.
# how to implement the carry over when adding two numbers
    # I only want to get the ones place. I could convert to a string and back to int. But this sucks!
# 999 + 999 = 1998
# 411 + 32 = 443
# 1* -> 1* -> 4*
# 2* -> 3* -> *None
# dummy -> h: val=3 -> val=4 -> val=4 

# 9 -> 9 
# 9 -> 9
# 198
# carry=True, 
# 8 -> 9 -> 1

# 1 -> 2
# 3
# 4 -> 2
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        head = ListNode()
        dummy.next = head
        carry=False
        while l1 != None or l2 != None:
            if(carry):
                head.val=1
            if(l1 != None):
                head.val += l1.val
                l1 = l1.next
            if(l2 != None):
                head.val += l2.val
                l2 = l2.next
            # handle carry
            if(head.val>=10):
                carry=True
                head.val = head.val%10
            elif(carry):
                carry=False
            if(l1!=None or l2!=None):
                head.next = ListNode()
                head = head.next
        if(carry):
            head.next = ListNode()
            head = head.next
            head.val = 1
        return dummy.next
```