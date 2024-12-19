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

```
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
- In [intuition-12-15-24](obsidian://open?vault=Leet-Code-Intuitions&file=12-15-24) there is a way to solve the sorted but rotated array that I think is a very clever use of binary search.
3. Stacks
	- reversing order, handling nested elements like parenthesis, monotonic stacks -> special order stacks maintain elements in a consistent, increasing or decreasing sorted order. Before adding a new element to the stack, any elements that break this order are removed from the top of the stack, ensuring the stack remains sorted.
4. Graphs
	- BFS should be used for shortest path problems. Because BFS does a level by level order traversal in unweighted graphs and each edge in a unweighted graph has an equal cost of 1. This systematic exploration ensures that when we reach the destination node for the first time, we have done so using the shortest possible path.
5. Trees
	- Height of a complete binary tree is log(n) with n being the number of elements
	- Trees that are not complete we will have to call the height a separate variable h.