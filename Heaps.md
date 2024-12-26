1. K Most Frequent Strings
```python
# K Most Frequent Strings
# Find the k most frequently occurring strings in an array, and return them sorted by frequency in descending order. 
# If two strings have the same frequency, sort them in lexicographical order.
# Input:["go", "coding", "byte", "byte", "go", "interview", "go"], k=2
# "go":3, "byte":2
# Output:["go", "byte"]
# if list<k return None?
# store frequencies in dict. Sort dict by *
# first problem: get k most frequent strings
    # store frequencies in a dict
    # build a min heap from the frequencies we get. Maintain only k elements in heap. 
        # so we pop the minimum freq from the root until we are left with the k most frequency elements in the heap
        # key being the element in the heap and the priority of the heap determined by the value
# second problem: sort by frequency then lexicographical if same frequency
    # How can i identify a similar priority item in the heap?
    # when we are building the min heap, maintain a set of frequencies encountered
        # How can I map the set frequency back to the word it came from?
        # the solution was to use a custom comparator class for heapq
# Above is first crack at solving problem
```
```python
# Attempted Solution
# K Most Frequent Strings
# Find the k most frequently occurring strings in an array, and return them sorted by frequency in descending order. 
# If two strings have the same frequency, sort them in lexicographical order.
    # In ascending lexicographical order?
# Input:["go", "coding", "byte", "byte", "go", "interview", "go"], k=2
# "go":3, "byte":2
# Output:["go", "byte"]
# if list<k return None?
# store frequencies in dict. Sort dict by *
# first problem: get k most frequent strings
    # store frequencies in a dict
    # build a min heap from the frequencies we get. Maintain only k elements in heap. 
        # so we pop the minimum freq from the root until we are left with the k most frequency elements in the heap
        # key being the element in the heap and the priority of the heap determined by the value
# second problem: sort by frequency then lexicographical if same frequency
    # How can i identify a similar priority item in the heap?
        # the heap will be ordered so pop from the heap and check the previous with the current if same swap based on lexicographical order.
    # when we are building the min heap, maintain a set of frequencies encountered*
        # How can I map the set frequency back to the word it came from?

# "go":3, "coding":1, "byte":2, "interview":1
# build min heap
# Time: n being number of elements in input, creating dict: o(n), heapify dict: O(klogk) -> pop from heap -> O(klogk)
# overall time: O(klogk + n)
# Space: heap and dict overall: O(n)

def kMostFreqString(arr: List[str], k:int] -> List[str]:
    dict_arr = Counter(arr)
    heap = []
    for key, value in dict_arr.items():
        heapq.heappush(heap, key, key=value)
        if len(heap)>k:
            heapq.pop(heap)
    output = []
    for h in heap:
        curr = heapq.pop(heap)
        if output:
            # TODO: what if there are more prev elements with same freq?
            prev=output[-1]
            if dict_arr[prev] == dict_arr[curr] and curr<prev:
                # sort prev and current element lexicographically
                temp = output.pop()
                output.append(curr)
                output.append(temp)
        else:
            output.append(curr)
    return output
# we could use min heap to maintain k elements then return the heap as the output.
    # we have to modify the comparator above to break ties of frequencies with lexicographical sorting
# the space suboptimal approach is to use a max heap then pop k elements from the max heap at the end. 
    # The max heap being the extra unnecessary space used. it will still be O(n+k) in space optimal solution
    # whereas in max heap solution its O(n+n) space

# Actual Solution
# handle frequency ties with custom comparator class
# we will define the __lt__ function since python's heapq implementation uses < to build the min-heap
class Pair:
    def __init__(self, str, freq):
        self.str = str
        self.freq = freq
    
    # min heap uses < comparator to build heap
    # so we define priority of heap here
    def __lt__(self, other):
        if self.freq == other.freq:
            # break tie of frequency by sorting it lexicographically
            # higher lexicographical string goes first
            return self.str > other.str
        return self.freq < other.freq

def k_most_frequent_strings_min_heap(strs: List[str], k:int) -> List[str]:
    freqs = Counter(strs)
    min_heap = []
    for str, freq in freqs.items():
        heapq.heappush(min_heap, Pair(str, freq))
        # ensure heap only contains k elements
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    res = [heapq.heappop(min_heap).str for _ in range(k)]
    res.reverse() # since its min heap
    return res
```

2. Combine K Sorted Linked Lists
```python
# Combine Sorted Lists
# Given k singly linked lists, each sorted in ascending order, combine them into one sorted linked list.

# Example
# 1->6
# 1->4->6
# 3->7
# 1->1->3->4->6->6->7

# time complexity has to be at least O(k*n)
# put each Node into a min heap
# pop from the min heap until heap is empty
# we are saying n is the number of elements in each list
# overall time: O(k*n*log(k*n))
# overall space: O(k*n)

# Space Optimal
# Space Complexity: O(k)
# overall time: O(k*n*log(k))
# with two linked lists we could have a pointer at each list and do comparisons before inserting to new list.
    # is there a way to generalize this to k lists?
    # use the heap approach
    # but only add the heads of the k linked list. Then when one node gets popped off add the next node in the same linked list to the heap.
    # continue this until all the linked lists are exhausted

# say n is the total number of elements
# space optimal approach
# Space Complexity: O(k) for heap
# Overall Time: O(n*log(k))
    # for each node we perform a push and pop into the heap which can be at most k elements

# 1*->6*
# 1*->4*->6*
# 3*->7*
# 1->1*->3*->4*->6*->6*->7*

# dummy -> None
# dummy=node -> node=1 -> node=1 -> node=1 -> node=1 -> node=3 -> node=3 -> node=4 -> node=4 
# node=6 -> node=6 -> node=6 ->  node=7 -> node=7 -> None
# heap: []
# return dummy.next
class Node:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next - next
        
def combineLinkedLists(nodeL: List[Node]) -> Node:
    dummy = Node()
    node = dummy
    if not nodeL:
        return head
    heap = []
    # populate heads of all k lists into heap
    for head in nodeL:
        # make sure head exists
        if head:
    `       heapq.heappush(heap, head)
    # go until heap is empty
    while heap:
        smallest_node = heapq.heappop(heap)
        node.next = smallest_node
        node = node.next
        if smallest_node.next:
            heapq.heappush(heap, smallest_node.next)
    return dummy.next
```
3. Median of an Integer Stream
```python
# Median of an Integer Stream
# Design a data structure that supports adding integers from a data stream and retrieving the median of all elements received at any point.
# add(num: int) -> None: adds an integer to the data structure
# get_median() -> float: returns the median of all integers so far
# have to run through some examples to get the intuition for this:
    # add(3) -> add(6) -> get_median() [3,6]:(3+6)/2 = returns 4.5 -> add(1) [1,3,5] -> get_median() returns 3
    # [1,3,6]

# when we add we have to keep the numbers in order in the data structure we add to
# when the number of elements is odd we have a single median. When number of elements is even we have two elements in median.
    # we have to solve this problem of finding the median with odd and even numbers
    
# 1. we could add the numbers to a list. Find the appropriate spot to put the number by doing binary search. And insert the new element
    # 2. we can calculate the median by getting the half way point of the list
        # if its even we do the (mid+(mid+1))/2
        # if its odd return mid
# 1. time O(log(n)) and 2. time: O(1)
# Overall space is O(n)
# overall time is O(n^2*log(n))

# Limitations of time and space: what is the range of n?
    # we have to store n elements so has to be at least O(n) space
    # keeping elements in order has to be at least log(n) time
        # instead of binary search we can use a min heap to keep elements in relative order when doing add.
        # now what about finding the median when using heap?
            #IDK
         
# binary search: we calculate mid we have left, right pointer
def binarySearch(element):
    left=0
    right = len(arr)-1
    mid = (right+left)//2
    # two edge cases when element should be inserted at end or beginning
    while left<=right:
        if element>mid:
            left += 1
        elif element<mid:
            right-=1
        else:
            # element==mid
            # insert element into mid position and shift other elements to the right 1
            # results in O(n) time
    # left==right mid will be left biased so right might fall below left but left will end up at the right spot.
    # insert at left and move elements at left and to the end one step to the right.

# calculate median
def getMedian():
    if not arr:
        return None
    n = len(arr)
    even = n%2==0
    mid = n//2 # mid is biased to left
    if len(arr)==1:
        return arr[0]
# Constraints: at least one value will be added before get_median is called

# Example:
# add(3) -> add(6) -> get_median() [3,6]:(3+6)/2 = returns 4.5 -> add(1) [1,3,6] -> get_median() returns 3
# [1,3,6]

# add(3):[3] -> add(6):[3,6]
# add(6):[3,6]
    # left=0
    # right=0
    # mid=0
    # left+=1 => 1
    # insert at 1
# TODO: see what happens if number first inserted is greater than second number
# get_median():[3,6]
    # even is true
    # mid=2//2=1
    # is length of 2 a special case???
    # (mid+(mid-1))//2 => (3+6)//2=4.5
    # return 4.5
# add(1): [1,3,6]
    # left=0
    # right=1 -> right-1=0 -> right-1=-1
    # mid=1//2=0 -> mid=0
    # insert at 0
    # [1,3,6]
# get_median():
    # even is false
    # mid=(3//2)==1
    # return mid

# instead of binary search for inserting into a list data structure.
# we can maintain two heaps. 1. for left half max heap
# 2. for right half min heap.
# the roots of both max and min heap will contain the median if length is even
# the max heap root will contain the median if the length is odd

# in this implementation there is no guarantee that I am inserting to the right heap
# example add(3), add(1), add(5), add(0)
# left_half:[3,5]
# right_half:[0,1]
# get_median() => (3+0)/2=1.5 * my implementation
    # [0,1,3,5] => (1+3)/2=2.0 * true answer
class IntegerStream:
    def __init__(self):
        self.left_half = [] #left half
        self.right_half = [] # right half
        self.len = 0
        
    def add(i):
        if self.len%2==0:
            # length is even
            heapq.heappush(self.left_half, -i)
        else:
            # length is odd
            heapq.heappush(self.right_half, i)
            
    def get_median():
        if self.len%2==0:
            # length is even
            leftMed = -1 * heapq.peek(left_half)
            rightMed = heapq.peek(right_half)
            return (leftMed+rightMed)/2
        else:
            # length is odd
            leftMed =  -1 * heapq.peek(left_half)
            return leftMed

# Actual solution
class IntegerStream:
    def __init__(self):
        self.left_half = [] # max heap
        self.right_half = [] # min heap
        
    def add(self, i:int):
        if not self.left_half or i<-self.left_half[0]:
            # i belongs to left half
            heapq.heappush(self.left_half, -i)
            # left half can be 1 more than right half
            if len(self.left_half) > len(self.right_half)+1:
                # rebalance left and right half
                heapq.heappush(self.right_half, -heapq.heappop(self.left_half))
        else:
            heapq.heappush(self.right_half, i)
            if len(self.right_half)>len(self.left_half):
                # rebalance left and right half
                heapq.heappush(self.left_half, -heapq.heappop(self.right_half))
            
    def get_median(self):
        if len(self.left_half)==len(self.right_half):
            # number of elements is even median is in both halves root
            return (-self.left_half[0] + self.right_half[0])/2.0
        # number of elements is odd median is in left_half
        return -self.left_half[0]
```