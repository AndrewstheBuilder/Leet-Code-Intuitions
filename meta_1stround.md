1. Delete the kth node from a singly linked list
```python
# Given the head of a linked list, remove the nth node from the end of the list and return its head.
# 1 -> 2 -> 3 -> 4 -> None

# use a two pointer approach
# two pointer both start head.
# move right pointer k steps. Then start moving both pointers forward.
# when right pointer hits end, left pointer is at node to delete 
    # or something like that depending on math
    
# 1 -> 2 -> 3 -> 4 -> None, k=1
# right=1,left=1,i=0 -> right=2, left=1, i=1==k -> right=3,left=2 -> right=4,left=3 3.next=4.next 1->2->3 -> None

# 1 -> 2 -> 3 -> 4 -> None, k=1
# right=1,left=1,i=0 -> right=2, left=1, i=1==k -> right=3,left=2 -> right=4,left=3 -> right=None, left=4
# My implementation
def kthNode(root:Node, k:int):
    left,right = root
    i = 0
    while right.next:
        if i == k:
           left = left.next 
        else:
            i += 1
        right = right.next
    # left should be the node to delete the next node from
    left.next = left.next.next
    
# actual solution
# 1 -> 2 -> 3 -> 4 -> None, k=1
# 1 -> None
# i is not correct if k=1 and the first node is also the last node.
# i would be 0 instead of being 1. We are supposed to delete the 1st node, with the first node being the end.
# so adding a dummy node will create the condition for 1 being the first node.
# The pointer is technically one node away on the first node. Which is what we wanted
def removeNthFromEnd(self, root: Optional[ListNode], k: int) -> Optional[ListNode]:
    if root==None:
        return
    dummy = ListNode(-1, root)
    left = right = dummy
    i = 0
    while right.next:
        if i == k:
            left = left.next 
        else:
            i += 1
        right = right.next
    # left should be the node to delete the next node from
    left.next = left.next.next if left.next else None

    # return the head of the singly linked list after removal
    return dummy.next
```
2. Binary Tree Vertical Order Traversal
```python
# 314. Binary Tree Vertical Order Traversal
# Given the root of a binary tree, return the vertical order traversal of its nodes' values. (i.e., from top to bottom, column by column).
# If two nodes are in the same row and column, the order should be from left to right.

# can the root be null? yes.

# Input: root = [3,9,20,null,null,15,7]
# Output: [[9],[3,15],[20],[7]]
#       3
#      / \
#     9   20
#        /  \
#       15   7
# 

# 3,0 -> 9,-1, 20,1 -> 15,0, 7,2
# dict: {0:[3,15], -1:[9], 1:[20], 2:[7]} 
# output:[[9], [3,15], [20], [7]]
# bfs traversal level by level
# initialCol=0
# go left: col-=1, go right: col+=1
# dict to store the col values. values will be in a list
# order dict to get keys in order and append to output list

# n being total number of nodes
# O(n) bfs.
# O(nlogn) for sort.
# O(n) to append to output.
# O(nlogn) time overall
# O(n) space for interm_dict
# O(n) space for sorting interm_dict.keys()
# O(n) space overall


def verticalOrderTraversal(root:Node) -> List[List[int]]:
    node = root
    queue = deque((node, 0))
    interm_dict = defaultdict(list)
    while queue:
        node,col = queue.popleft()
        interm_dict[col].append(node.val)
        if node.left:
            queue.append((node.left,col-1))
        if node.right:
            queue.append((node.right,col+1))
    output = []
    for key in interm_dict.keys().sort():
        output.append(interm_dict[key])
    return output
    
    
# actual solution
# forgot node==None edge case
# syntax error with defining deque
# Also optimized away the sorting portion by keeping track of min and max column
# Definition for a binary tree node.
# Overall Time Complexity: O(n) without sorting like above
# Overall Space Complexity: O(n)
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # root is null edge case
        if root==None:
            return []
        node = root
        queue = deque([(node, 0)])
        interm_dict = defaultdict(list)
        # keep track of min and max column to avoid sorting interm_dict keys 
        # at the end
        min_column=max_column=0
        while queue:
            node,col = queue.popleft()
            min_column = min(min_column, col)
            max_column = max(max_column, col)
            interm_dict[col].append(node.val)
            if node.left:
                queue.append((node.left,col-1))
            if node.right:
                queue.append((node.right,col+1))
        output = []
        for key in range(min_column, max_column+1):
            output.append(interm_dict[key])
        return output
```
- With the time constraints. I struggled to find the optimal solution and I kept trying to jog my memory for it. I should have been more careful about the problem solving process. So I made meaningful mistakes or maybe even happened upon the optimal solution. Instead I may have used fast intuitions to guess and check. I guess its okay. I just need more practice.
