* When presented with an interval problem its important to clarify whether the intervals are open, closed, or half-open intervals. This can change the nature of the intervals overlapping.
* Two intervals overlap if they share at least 1 common value.
* It can be helpful to sort intervals so they can be processed. If you sort them by the start point it allows you to go through the intervals in chronological order.
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
3. Largest Overlap of Intervals [LeetCode Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/editorial/)
```python
# Largest Overlap of Intervals
# Given an array of intervals, determine the maximum number of intervals that overlap at any point. 
# Each interval is half-open, meaning it includes the start point but excludes the end point.

# constraints: the interval's start < end so start==end is not true so no solely open intervals here, the input will contain at least one interval
# not guaranteed to be in order

# input: intervals = [[1,3], [2,6], [4,8], [6,7], [5,7]]
# sort intervals by start 
    # intervals=[[1,3],[2,6],[4,8],[5,7],[6,7]]

# what counts as an overlap? 
    # Is it the number of intervals that have an overlap? Or the amount of overlaps each interval has with another interval?
# For each interval see how many other intervals it has an overlap with. We take the maximum number of intervals that are overlapping to be the output
# I think this answer has to be O(n^2) time.
    # With sorted intervals if start of next interval is greater than interval we are on then we can stop checking further intervals    
# How to count an overlap?
    # A is the interval we are checking all intervals after it are having start1 =< start_next
    # B start is less than A end we have an overlap
        # count up overlaps
            # compare to maxOverlaps found so far
        # stop when B start is greater than or equal to A end.
    # start curr_overlap at 1 to include the interval itself.
# time complexity is dominated by O(n^2)
# space complexity is O(1)

# intervals=[[1,3],[2,6],[4,8],[5,7],[6,7]]
# A = [4,8], B=[6,7]
# maxOverlaps=2
# curr_overlap=2
# return 3

# this algorithm is not correct because [[0,30],[5,10],[15,20]] returns 2 but its because it counts the total number of overlaps with 1 interval
    # it does not check if the first two intervals overlap with the third one which it does not, but 2 is the right answer because 2 intervals do overlap.
    # so its a misunderstanding then :)
# Time: O(n^2)
# Space: O(1)
# edge case is it possible to have an interval start and end at the same spot?
    # Yes and this algorithm will handle it I believe
def largest_overlap(intervals:List[List[int]]):
    # sort interval over start
    intervals.sort(key=lambda a: a[0])
    
    max_overlaps = 1
    n = len(intervals)
    for i in range(n-1):
        curr_overlaps=0
        start_curr, end_curr = intervals[i]
        for j in range(i+1, n):
            start_nxt, end_nxt = intervals[j]
            if start_nxt>=end_curr:
                # next interval is past overlap point 
                # move on to next curr interval
                break
            if start_nxt<end_curr:
                curr_overlaps += 1
        max_overlaps = max(max_overlaps, curr_overlaps)
    if max_overlaps==0:
        return 0
    # do +1 to account for the interval we are comparing with
    return max_overlaps+1

# Intuition for Sweeping Line Algorithm. We collected the starts and ends. Sorted by point, then end, then start.
    # if we encounter starts without encountering an end then we have multiple active intervals. 
    # if we encounter an end then we have 1 less active interval
    # we have to sort by end first if the points match because ends are open and we should say an interval ended first 
        # to decrement active intervals before counting the next start. so we do not count an overlap betweenend and start.
# Optimal Solution: Sweeping Line Algorithm
# Time: O(nlogn). O(n) to go through intervals, O(nlogn) to sort the points, then to go through points of size 2n which is O(n)
# Space: O(n). because of points

# input: intervals = [[1,3], [2,6], [4,8], [6,7], [5,7]]
# points = [[1,'S'], [3,'E'], [2,'S'], [6,'E'], [4,'S'], [8,'E'], [6,'S'], [7, 'E'], [5,'S'], [7,'E']]
# points sorted = [[1,'S'],[2,'S'],[3,'E'],[4,'S'],[5,'S'],[6,'E'],[6,'S'],[7,'E'],[7,'E'],[8,'E']]
# active_intervals=0
# max_intervals=3
# return max_intervals=3

# edge case is it possible to have an interval start and end at the same spot?
    # Yes and this algorithm will handle it
def largest_overlap_of_intervals(intervals: List[Interval]) -> int:
    points = []
    for interval in intervals:
        points.append((interval.start, 'S'))
        points.append((interval.end, 'E'))
    # Sort in chronological order. If multiple points occur at the same
    # time, ensure end points are prioritized before start points
    points.sort(key=lambda x: (x[0], x[1]))
    
    active_intervals = 0
    max_intervals = 0
    for p in points:
        if p[1] == 'S':
            active_intervals += 1
        else:
            active_intervals -= 1
        max_intervals = max(active_intervals, max_intervals)
    return max_intervals
```