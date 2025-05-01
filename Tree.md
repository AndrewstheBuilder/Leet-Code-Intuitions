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
* String matching solution
```python
'''
Time Complexity: O(N+M), postorder(), and the string in function which could O(N*M) in the worst case according to Grok but that is in a rare case
Space Complexity: O(N+M), postorder(), other data structures used to store intermediary and string
	*with N being number of nodes in root, M being number of nodes in subroot
'''
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        root_postorder=[]
        subRoot_postorder=[]
        def postorder(node, postorder_list):
            if node==None:
                postorder_list.append('#,')
                return
            postorder(node.left, postorder_list)
            postorder(node.right, postorder_list)
            postorder_list.append(str(node.val)+',')

        postorder(root, root_postorder)
        postorder(subRoot, subRoot_postorder)
        root_str = ''.join(root_postorder)
        subRoot_str=''.join(subRoot_postorder)
        if subRoot_str in root_str:
            return True
        else:
            return False
```