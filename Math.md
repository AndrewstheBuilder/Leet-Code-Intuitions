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