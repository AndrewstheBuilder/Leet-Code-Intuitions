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
- Solution
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
  - Solution
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

6.[146. LRU Cache](https://leetcode.com/problems/lru-cache/editorial/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solution:
```python
class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.head = LNode(key=None)
        self.tail = LNode(key=None)
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        ret = self.cache.get(key,-1)
        if(ret != -1):
            self._remove(ret)
            self.cache[key] = self._add(key, ret.val)
            return ret.val
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self._remove(self.cache[key])
            self.cache[key] = self._add(key, value)
        else:
            self.cache[key] = self._add(key,value)

        # capacity check
        if(len(self.cache.keys())>self.capacity):
            lru_node=self.head.next
            self._remove(lru_node)
            self.cache.pop(lru_node.key, "not Found")
    
    def _add(self, key, value):
        lastVal = self.tail.prev
        newVal = LNode(key, value, self.tail, lastVal)
        lastVal.next = newVal
        self.tail.prev = newVal
        return newVal
    
    def _remove(self, node):
        '''
        1. prev - node - next
        2. prev - next
        '''
        node.prev.next = node.next
        node.next.prev = node.prev

class LNode:
     def __init__(self, key, val=None, nextNode=None, prev=None):
        self.next = nextNode
        self.prev = prev
        self.val = val
        self.key = key

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
- Intuition: Doubly linked list allows for removal and addition of element in O(1) time. Implement it manually without using python specific functions. 
- I ran into problems because I doubted what it means to remove and add an element. And I wanted to make sure that the doubly linked list always had a head or tail but could not think of adequete test cases. I need to test instead of thinking in my head. The test case would be to remove an element when capacity is at 1. And to make sure init is correct.
- I also did not consider the edge cases when I remove or add an element.
- I forgot that I needed to point to the key of the cache just like the cache points to the node in doubly linked list.
          - The cache value is not the actual value but the LinkedList node that contains the value.
  
- Python Library Solution uses a ordered dict.
```python
import collections


class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.dic = collections.OrderedDict()

    def get(self, key: int) -> int:
        if key not in self.dic:
            return -1

        self.dic.move_to_end(key)
        return self.dic[key]

    def put(self, key: int, value: int) -> None:
        if key in self.dic:
            self.dic.move_to_end(key)

        self.dic[key] = value
        if len(self.dic) > self.capacity:
            self.dic.popitem(False)


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
7. [1091. Shortest Path in Binary Matrix 2D Graph Problem](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Attempted Solution(not working)
```python
   def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        def bfs(grid,i,j):
            s=deque([(i,j,1)])
            visited = set((0,0))
            ans = 20001 #max answer is 10000
            n = len(grid)
            while(s):
                (x,y,l) = s.popleft()
                if(grid[x][y]==1):
                    # can't be at 1
                    continue
                if(x==n-1 and y==n-1 and l<ans):
                    # arrived at final bottom right square
                    ans=l # found shorter path
                    continue
                if(x+1<n and (x+1,y) not in visited):
                    s.append((x+1,y,l+1))
                    visited.add((x+1,y))
                elif(x-1>=0 and(x-1,y) not in visited):
                    s.append((x-1,y,l+1))
                    visited.add((x-1,y))
                elif(y-1>=0 and (x,y-1) not in visited):
                    s.append((x,y-1,l+1))
                    visited.add((x,y-1))
                elif(y+1 < n and (x,y+1) not in visited):
                    s.append((x,y+1,l+1))
                    visited.add((x,y+1))
                elif(x+1<n and y+1 < n and (x+1,y+1) not in visited):
                    s.append((x+1,y+1,l+1))
                    visited.add((x+1,y+1))
                elif(x+1<n and y-1 >= 0 and (x+1,y-1) not in visited):
                    s.append((x+1,y-1,l+1))
                    visited.add((x+1,y-1))
                elif(x-1>=0 and y+1 < n and (x-1,y+1) not in visited):
                    s.append((x-1,y+1,l+1))
                    visited.add((x-1,y+1))
                elif(x-1>=0 and y-1 >=0 and (x-1,y-1) not in visited):
                    s.append((x-1,y-1,l+1))
                    visited.add((x-1,y-1))
            return -1 if ans == 20001 else ans
        return bfs(grid, i=0,j=0)
```
- I attempted this long ass solution its probably a sign that its not correct. I heard BFS and thought about finding all the possible children. This is how I went about trying to do it and its massively infficient.
- To find the shortest path in a graph we should use BFS
- Efficient Solution
```python
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        n = len(grid)-1
        q = deque([(0,0,1)])
        visited = set((0,0))
        # MAX_INT=200000
        # ans = MAX_INT

        if(grid[0][0] != 0 or grid[n][n] != 0):
            return -1 # no possible path

        directions = [(0,1),(1,0),(1,1),(-1,0),(0,-1),(-1,-1), (1,-1),(-1,1)]

        def get_neighbors(x,y,n,visited,grid):
            for i,j in directions:
                new_row = x+i
                new_col = y+j
                if (new_row < 0 or new_row>n or new_col<0 or new_col>n or (new_row, new_col) in visited):
                    continue
                if(grid[new_row][new_col] != 0):
                    continue
                yield (new_row, new_col)

        while(q):
            (x,y,d) = q.popleft()
            # if(x==n and y==n and d<ans):
                # ans = d
            if(x==n and y==n):
                return d
            for new_row, new_col in get_neighbors(x,y,n,visited,grid):
                visited.add((new_row, new_col))
                q.append((new_row, new_col,d+1))
        # return ans if ans!=MAX_INT else -1
        return -1
```
- Time Complexity O(n) n being the total amount of cells in the grid
- Space Complexity O(n)
- Intuition: Do BFS for grid search problem. There is only 8 directions you can go and you can calculate the offsets for all 8 directions. so that is why you can create the get_neighbors(). From there its pretty straight forward.
        - I did not need to keep a ans variable because the shortest ans is the one I reach first so return d immediately upon reaching the end of the grid.
        - return early if you have an unreachable path
        - that getNeighbors() function is a iterator. the yield keyword makes it so.

- [1762: Buildings with an Ocean View](https://leetcode.com/problems/buildings-with-an-ocean-view/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solution
```python
    def findBuildings(self, heights: List[int]) -> List[int]:
        j = len(heights)-1
        biggestHeight = -1
        ret = deque()
        while j>=0:
            if(biggestHeight<heights[j]):
                biggestHeight = heights[j]
                ret.appendleft(j)
            j-=1

        return list(ret)
```
- O(n) space and time complexity

- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solution
```
# Time Complexity: O(n^2), Space Complexity: O(1)
    def subarraySum(self, nums: List[int], k: int) -> int:
        total = 0
        for i in range(len(nums)):
            sum = 0
            for j in range(i,len(nums)):
                sum += nums[j]
                if(sum==k):
                    total += 1
        return total
    # Time Complexity: O(n), Space Complexity: O(n)
    def subarraySum(self, nums: List[int], k: int) -> int:
        total = 0
        sum = 0
        hash = {}
        for i in range(len(nums)):
            sum += nums[i]
            complement = sum-k
            if(complement in hash):
                # sum-complement=k
                # complement being a sum to another index
                # (sum-complement) this gets sum of a subarray if the complement exists. 
                # sum(subarray[complement_sum_i:i+1])
                total += hash.get(complement)
            hash[sum]=hash.get(sum,0)+1 # add this after so we do not count sum[i]==k twice
        # get total += instances where sum[i]==k
        total += hash.get(k, 0)
        return total
```
- Two solutions above.
- First Intuition get the sum at sum[start:end] with start being in the first for loop and end being in the second for loop.
   - You get the sum by for i in range(start,end). And since subarrays are contigouous you can do sum[i] = sum[i-1]+nums[i]
- Second Intuition store the sum at index in hash map. Get the sum[start,end] by seeing if complement is in hashmap. Complement is sum[some_other_index_from_before] and k = sum[curr]-sum[some_other_index_from_before] which can be rewritten for the complement we are searching for by calculating sum[curr]-k=sum[some_other_index_from_before]
          - Also we get the sums where it just equals k
          - We have to insert into hashmap after checking complement. Edge case nums=[1] k=0. 1-0=1 and 1 is in hashmap but its not a sum from some other index that came before. 1-1=0. But we cannot subtract the value at the index from itself that is wrong math for finding the complement.

- [Problem Merge Intervals](https://leetcode.com/problems/merge-intervals/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solution
```python
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:  
        intervals = sorted(intervals, key=lambda inter: inter[0])    
        ret = [intervals[0]]
        for i in range(1,len(intervals)):
            prev = ret.pop()
            curr = intervals[i]
            if(prev[1]>=curr[0]):
                maxElem = prev[1] if prev[1]>curr[1] else curr[1]
                ret.append([prev[0],maxElem])
            else:
                ret.append(prev)
                ret.append(curr)
        return ret
```
- Time Complexity: O(nlogn)
- Space Complexity: O(n)
- I had to sort the array. They were being quiet about sorting it or not. I have to convince myself the sorting is correct. I am sorting by the first part of the array arr[0] and not arr[1]
          - Example:
          - [[2,4],[4,6],[5,7]].
          - The array is in ascending order on the first part. But the second part we not really sure about. That is what we have to check is in the interval or not. The second array could be in the interval of the first one. but not vice versa because we sorted by arr[0]

- [Problem Custom Sort String](https://leetcode.com/problems/custom-sort-string/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solutions:
```python
    def customSortString(self, order: str, s: str) -> str:
        if(len(order)==1 or len(s)==1):
            return s
            
        freq_table = {}
        for i in range(len(s)):
            freq_table[s[i]] = freq_table.get(s[i],0)+1
        
        ret = []
        for i in range(len(order)):
            if order[i] in freq_table:
                # attach to return when encountered in order
                # according to the freq gathered from s
                ret.extend([order[i]]*freq_table[order[i]])
                del freq_table[order[i]]
        for key in freq_table:
            # get remaining elements in freq_table that are not in order
            ret.extend([key]*freq_table[key])
        return ''.join(ret)
    # Time Complexity: O(n), Space Complexity: O(n)

    def customSortString(self, order: str, s: str) -> str:
        if len(order)==1 or len(s)==1:
            return s

        ordered_dict = {value: index for index, value in enumerate(order)}
        s = sorted(s, key=lambda x:ordered_dict.get(x, float('inf')))
        return ''.join(s)
    # remember to look at input and output to see how the solution should be implemented
        # input/output is the best source of information
    # Time Complexity: O(nlogn), Space Complexity: O(n)
```
- first solution I create a custom comparator by iterating through s and looking at the ranking established by order for each s. If s[i] is not in order then make it infinity ranked.
- second solution create a frequency table from s then iterate through order and add in s to ret the number of times it occurs in s.
- Don't forget to look over your solution line by line and run through a possible input/output. Make sure to include edge cases that can be reasonably walked through like s==1 or order==1

- [Minimum Add to Make Parenthesis](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- Solution
```python
    def minAddToMakeValid(self, s: str) -> int:
        if(len(s)==0):
            return 0
        incorrect = 0
        open_bracket = 0
        # st = []
        for i in range(len(s)):
            if(s[i]=='('):
                open_bracket += 1
                # st.append(i)
            else:
                if(open_bracket==0):
                    incorrect += 1
                    continue
                # st.pop()
                open_bracket -= 1
        incorrect += open_bracket
        return incorrect
```
- I first had a stack based solution. But its not necessary for this problem. I just have to match the number of open brackets to the closed brackets. Just need to store an integer.

- [Sum Root to Leaf Numbers]()
- Recursive DFS Solution
```python
   def sumNumbers(self, root: Optional[TreeNode]) -> int:
        # since one has to visit all nodes the best possible time complexity is linear
            # we can improve on space!
        # DFS preorder -> root -> Left -> Right
            if root==None:
                return 0
            def dfs(node, num, total):
                if node:
                    if(node.left==None and node.right==None):
                        # encountered leaf
                        total += num
                        return total
                    if node.left:
                        newNum = num*10 + node.left.val
                        total = dfs(node.left, newNum, total)
                    if node.right:
                        newNum = num*10 + node.right.val
                        total = dfs(node.right, newNum, total)
                return total
            return dfs(root, num=root.val,total=0)
```
- Intuition: Visit the nodes preorder so it goes root -> left subtree -> right subtree
-         - If you encounter leaf then visit the node otherwise ignore the visitation.
-         - Each function stack gets its own version of num. Hence why we had to do newNum
-             -  The current function gets the total returned from its recursive call and gets the total updated
