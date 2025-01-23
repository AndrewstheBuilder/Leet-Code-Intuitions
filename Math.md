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