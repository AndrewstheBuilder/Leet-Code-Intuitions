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
```