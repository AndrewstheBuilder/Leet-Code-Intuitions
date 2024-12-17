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
