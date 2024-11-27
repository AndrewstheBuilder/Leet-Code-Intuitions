1. [Pow(x,n)](https://leetcode.com/problems/powx-n/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- One of the hardest solutions to get but once I ran through a possible problem in the solution I got it
- solution
```python
def myPow(self, x: float, n: int) -> float:
        def pow(n, x):
            # base case
            if n == 0:
                return 1
            if(n<1):
                # all the negative exponent does is return the result in fractional form
                return 1.0/(pow(-1*n, x))
            
            if(n%2==1):
                return x * pow((n-1)//2, x*x)
            else:
                return pow(n//2, x*x)
        return pow(n,x)
```
- Intuitions:
- we are saying if n == 0 return 1 that parts straight forward.
- if n < 1 return the final answer in a 1/ fraction. Evaluate the rest of the problem as if the exponent is positive that takes care the negative exponent.
- for odd numbers we are leaving out one x and then halving the search space. x*x that is passed as a argument is only returned in the odd case. if n==1 it return x*x.
    - we are halving the search space because doing x*x makes the exponent on x go from 2,4,8,16. Its exponentiating all the way until x==0.
       - Flooring the division also has the effect of moving n towards 0
    - if its even we do x*x and halve n.
- if the initial input is even it will eventually hit n = 1 where we evaluate x^n * (0//2, x*x).
- if the initial input is odd it will start out with x * pow() and hit one where it returns x*(n-1)
    - so the last return will be 1 * x * x^(n-1).
    - the n-1 is not explicitedly in the code. Its a result of doing x*x over and over again so n will be a exponentiated 2. Then since its odd there is one more x left over by design!
- Time Complexity: O(logn) halving the search space. (n/2^k) = 1 in the search space. that is either exactly 1 in the search space or close to it. take n = 2^k log of both side log(n) = log(2^k) --> log(n) = k

2. [Problem Nested List Weight Sum](https://leetcode.com/problems/nested-list-weight-sum/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Attempted Not Working Solution:
```python
global running_sum = 0
    def dfs(l, depth, i):
        # base case
        if(i==len(l)):
            return

        if(l[i].isInteger()):
            running_sum += depth * l[i].getInteger()
            dfs(l, depth, i+1)
        if(l[i].getList()):
            dfs(l[i].getList(),depth+1, 0)
        return

    dfs(nestedList, 1, 0)
```
- Incorrect and inefficient ideas. I was going to keep a global variable which could affect code debugging if other parts of the programming is using it and maybe even thread safety if other parts of the programming are using it in different threads. I also wanted to pass and keep track of the index which is fine for the first nested list we run into but then once we get out there is no index information.
  
- Solution:
```python
    def depthSum(self, nestedList: List[NestedInteger]) -> int:
        def dfs(nested_l, depth):
            total = 0
            for nested in nested_l:
                if nested.isInteger():
                    total += nested.getInteger() * depth
                else:
                    total += dfs(nested.getList(), depth+1)
            return total

        return dfs(nestedList, 1)
```
- Intuitions:
        - Keep track of the total in the recursive function by returning it at the end. This will pass it back to the function calls and the answer will bubble up.
        - keep track of the index of each list in one instance of the function so each list gets iterated in one instance. If we encounter a nested list we will go to another instance of the function to iterate through the list
        - This way we will iterate on the nested lists and integers at each level. If you can name each for loop in each function call and say its distinct you can think that each for loop will handle each level of the nestedList.
        - The base case is when we are done iterating through the list and return the total
        - Then we have a case where we add to the total and when we go one level deeper and iterate on the next nested list.
- Time Complexity: O(n) we visit each element once
- Space Complexity: O(n) imagine each element is a nested list we will have a function call that we need to keep track of for each element.
3. [71. Simplify Path](https://leetcode.com/problems/simplify-path/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
  - Sort of difficult string parsing problem because some key intuitions I missed!
  - Solution:
```python
def simplifyPath(self, path: str) -> str:
        ret = []

        components = path.split('/')

        for c in components:
            if(c==''):
                continue
            elif(c=='..'):
                if(ret):
                    # ignore last directory
                    ret.pop()
            elif(c=='.'):
                continue # stay at current directory
            else:
                ret.append(c)

        return '/'+'/'.join(ret)
```
- O(n) space and time
- Intuitions I missed:
          - Doing split on '/' will get rid of the '/' and replace them with spaces while also splitting out the components that are not '/'s by delimiting with '/'s. That takes care of my '/' issues. I can join them on '/' at the end
          - Now we are only inserting directories into the stack
          - Using a stack I can pop the last inserted directory if we encounter '..' and there is something in stack.
          - I just continue if I encounter '.'
          - I do not have to worry about '/'s!!

4. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
Solution
```python
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        def bfs(node):
            if(node==None):
                return []
            stack = deque([node, None])
            ret = [node.val]
            while(len(stack)>0):
                n = stack.popleft()
                if(n!=None):
                    if(n.left):
                        stack.append(n.left)
                    if(n.right):
                        stack.append(n.right)
                else:
                    if(stack):
                        ret.append(stack[-1].val) # assume get by value and not reference
                        stack.append(None) # new level
            return ret
        return bfs(root)
```
- Time Complexity and Space Complexity O(n)
- Intuition: We are doing a level by level traversal with BFS. Keeping track of when we reach the end of the level by using None. We append a None when we reach the end of the previous level that way it delimits the end of the next level.
          - Once we go through all the elements of the previous level we should have the next level appended to stack
          - Append the last added value in stack because that is the right most element of the previous level if the stack has any elements at all. Remember if there are no elements there are no nodes on the previous level and we are done traversing the tree.
- All the tree traversals: <img width="837" alt="traversals" src="https://github.com/user-attachments/assets/f2786168-591f-43bd-a208-a4700f67375f">
5. [1570. Dot Product of Two Sparse Vectors](https://leetcode.com/problems/dot-product-of-two-sparse-vectors/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
  Solution
  ```python
        class SparseVector:
            def __init__(self, nums: List[int]):
                self.indexes = {}
                for i in range(len(nums)):
                    if(nums[i] != 0):
                        self.indexes[i] = nums[i]
        
            # Return the dotProduct of two sparse vectors
            def dotProduct(self, vec: 'SparseVector') -> int:
                total = 0
                for key in self.indexes:
                    total += vec.indexes.get(key,0)*self.indexes[key]
                return total
  ```
- Important thing here is the time and space complexity differences between __init__ and the dotProduct function. __init__ is where we iterate through the input O(n) and create the data structure to store the sparse array O(L). Then dotProduct we iterate through the dict O(L) time complexity and create new no space with reference to the input O(1).
