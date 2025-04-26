1. [Subtree of another tree](https://leetcode.com/problems/subtree-of-another-tree/description/?envType=problem-list-v2&envId=string-matching)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
# at each node of root we run a dfs on it alongside subRoot
# is_identical(root) or is_identical(left) or is_identical(right) return True
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def dfs(root):
            if root == None:
                return False
            if is_identical(root, subRoot):
                return True
            return dfs(root.left) or dfs(root.right)
            
        def is_identical(node, subNode):
            if node is None or subNode is None:
                return node is None and subNode is None # both have to be None
            return node.val==subNode.val and is_identical(node.left, subNode.left) and is_identical(node.right, subNode.right)
            
        return dfs(root)
```
```python
# I made a minor tweak to the dfs return
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def dfs(root):
            if root == None:
                return False
            # if is_identical(root, subRoot):
            #     return True
            return is_identical(root, subRoot) or dfs(root.left) or dfs(root.right)
            
        def is_identical(node, subNode):
            if node is None or subNode is None:
                return node is None and subNode is None # both have to be None
            return node.val==subNode.val and is_identical(node.left, subNode.left) and is_identical(node.right, subNode.right)
            
        return dfs(root)
```
- Time Complexity of Both: O(M\*N) M being root and N being subTree number of nodes
- Space Complexity: O(M + N). We go through M nodes so there can be M elements in the recursive stack and at each call we go through the subtree which adds N elements to the recursive stack.
- TODO look at string matching solution to this problem