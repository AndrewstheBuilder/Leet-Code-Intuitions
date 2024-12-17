1. Two Pointer
  - Sliding Window
    - useful when the search might involve two for loops instead we can use two pointers and slide a window across it
    - fixed sliding window. Example We are tasked with finding a substring inside a larger string
    - Dynamic Sliding Window. Example a condition has to be met inside a string
2. Binary Search
  - midpoint calculation to avoid overflow: left+(right-left)//2
    - integer overflow prone in languages other than python midpoint: (right+left)//2
  - we can use left==right for the search condition possibly everytime? while left<right:
  - As a general rule, in upper bound binary search, we should bias mid to the right.
    - bound search is when we keep the mid after having found the target and keep it pusshing
    - We have lower and upper bound search
    - The mid calculation is normally bias to the left because of the floor //2 operation.
```python
# Lower Bound
def lower_bound_binary_search(nums:List[int], target:int) -> int:
  left,right=0, len(nums)-1
  while left<right:
    mid=(left+right)//2
    if nums[mid] > target:
      right=mid-1
    elif nums[mid] < target:
      left = mid+1
    else:
      right=mid
return left if nums and nums[left]==target else -1
```
```python
def upper_bound_binary_search(nums: List[int], target:int) -> int:
  left,right=0,len(nums)-1
  while left<right:
    # In upper-bound binary search, bias the midpoint to the right.
    mid = (left+right)//2 + 1
    if nums[mid]>target:
      right = mid-1
    elif nums[mid] < target:
      left=mid+1
    else:
      left = mid
  # If the target doesn't exist in the array, then its possible that
  # 'left=mid+1' places the left pointer outside the array when
  # 'mid == n-1'. So, we use the right pointer in the return
  # statement instead.
  return right if nums and nums[right] == target else -1
```
