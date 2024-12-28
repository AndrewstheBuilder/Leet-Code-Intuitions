* When presented with an interval problem its important to clarify whether the intervals are open, closed, or half-open intervals. This can change the nature of the intervals overlapping.
* Two intervals overlap if they share at least 1 common value.
* It can be helpful to sort intervals so they can be processed. If you sort them by the start point it allows you to go through thek intervals in chronological order.
	* What happens if you sort the intervals by the end point?

2. Overlapping Intervals
```python
# Identify All Interval Overlaps
# Return an array of all overlaps between two arrays of intervals: intervals1 and intervals2. Each individual interval array is sorted
# by start value, and contains no overlapping intervals within itself.
# Input: intervals1=[[1,4],[5,6],[9,10]], intervals2=[[2,7], [8,9]]
# Output: [[2,4], [5,6], [9,9]]


# intervals1=[[1,4],[5,6],[9,10]], intervals2=[[2,7], [8,9]]
# intervals1=[[1,4],[5,6],[9,10]], intervals2=[[6,7], [8,8]]

# How to traverse between both intervals 1 and 2?
    # have a pointer at start of each intervals
        # how to move a pointer forward?
        # move forward the interval with smaller endpoint
            # if both endpoints are equal move both intervals forward
# comparison for merge: 
    # Identifying the overlapping portions
    # make sure 1 intervals start is less than other interval's end but greater than other interval's start
    # consistently call an interval interval A if its start is less than the other interval.
    # if start of B is less than end of A than there is an overlapp
    # overlap: [B, min(end A, end B)]
        # the merged overlapp is the greater of the start between the two intervals
        # and the lesser of the ends between the two intervals
        # and if the start or end is equal return that!
# Time: O(n1+n2)
# Space: O(1)

# intervals1=[[1,4],[5,6],[9,10]], intervals2=[[2,7], [8,9]]
# p2=2, p1=2
# A=[8,9], B=[9,10]
# overlaps: [[2,4], [5,6], [9,9]]

# edge case starts are the same
# intervals1: [[1,3]], intervals2: [[1,5]]
# p1=0, p2=0
# A = [1,5] , B=[1,3]
# overlap=[[1,3]]

# edge case endpoints are the same
# intervals1: [[2,3], [5,6]], intervals2: [[1,3], [6,7]]
# p1=0, p2=0
# A=[1,3], B=[2,3]
# 
# both intervals are exact same
# My implementation which is correct
def identify_overlap_intervals(intervals1: List[List[int]], intervals2: List[List[int]]) -> List[List[int]]:
    if not intervals1 or not intervals2:
        return []
    p1=p2=0
    n1 = len(intervals1)
    n2 = len(intervals2)
    overlaps = []
    while p1<n1 and p2<n2:
        if intervals1[p1].start < intervals2[p2].start:
            A = intervals1[p1]
            B = intervals2[p2]
        else:
            A = intervals2[p2]
            B = intervals1[p1]
        if B.start<=A.end:
            # there is overlap
            overlaps.append([B.start, min(A.end, B.end)])
        if intervals1[p1].end < intervals2[p2].end:
            p1+=1
        else:
            p2+=1
    return overlaps

# Chat GPT Implementation which I verified works by looking at drawing of intervals
# More efficient comparison of overlap
# the pointer movement is more efficient by moving both pointers forward if the intervals end at the same time.
from typing import List

def identify_overlap_intervals(intervals1: List[List[int]], intervals2: List[List[int]]) -> List[List[int]]:
    if not intervals1 or not intervals2:
        return []
    
    p1, p2 = 0, 0
    n1, n2 = len(intervals1), len(intervals2)
    overlaps = []
    
    while p1 < n1 and p2 < n2:
        a_start, a_end = intervals1[p1]
        b_start, b_end = intervals2[p2]
        
        # Calculate the start and end of the potential overlap
        start_overlap = max(a_start, b_start)
        end_overlap = min(a_end, b_end)
        
        # If there is an overlap, add it to the list
        if start_overlap <= end_overlap:
            overlaps.append([start_overlap, end_overlap])
        
        # Move the pointer that has the interval ending first
        if a_end < b_end:
            p1 += 1
        elif a_end > b_end:
            p2 += 1
        else:
            # If both intervals end at the same time, move both pointers
            p1 += 1
            p2 += 1
    
    return overlaps           
```
