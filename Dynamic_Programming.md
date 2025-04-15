1. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/?envType=company&envId=amazon&favoriteSlug=amazon-thirty-days)
```python
class Solution:
    # seems to be a recursive problem
    # we can optimize after solving it recusively by making it iterative
    # s = "rabbbit", t = "rabbit"
    # what are the base cases and subproblems to optimize?
    # * I really have to solve the problem first in a simple, algorithmic way before breaking it down into subproblems and base cases
    # if there is a match we can make two choices
    # func(i, j) = func(i + 1, j) + func(i + 1, j + 1) # ignore the match or add the match and move the pointer forward on both strings
    # if there is no match, there is only one choice
    # func(i, j) = func(i + 1, j)
    #  So, i == M or j == N will form our base case. However, what we return in our base case is what will tie this whole thing together.
        # we should return a count in the base case. we could return True or False for if we found t in s or not.
        # return True
        # func(i, j) = True
        # if True: count += 1
        # formed_str="rabbb", source_of_truth="rabbit", pointer_to_options=4, we compare [length-1] of formed_str to source_of_truth
            # pointer 4 disagrees so ignore that index (4) and move to next index
            # "rabbi"
            # "rabbit"
            # one path = [0,1,2,3,5,6]
            # how can I ignore indexes with for loops -> just skip it
                # How can I make recursion calls to find all possible ways to generate source_of_truth
                # What are the subproblems to solve?
    # A subsequence is a string that can be formed in order by deleting different characters inside the provided options.
    # - the base case is when there is only 1 character in the formed_string? No!
    # "r", curr_index=0 -> how many distinct ways are there to form "r" 1
    # "ra",  -> 1
    # "rab" -> 3
    # "rabb" -> 2
    def numDistinct(self, s: str, t: str) -> int:
```
- Recursive DP Solution
``` python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:

        # Dictionary for memoization
        memo = {}

        def uniqueSubsequences(i: int, j: int) -> int:

            M, N = len(s), len(t)

            # Base case
            if i == M or j == N or M - i < N - j:
                return int(j == len(t))

            # Check if the result is already cached
            if (i, j) in memo:
                return memo[i, j]

            # Always make this recursive call
            ans = uniqueSubsequences(i + 1, j)

            # If the characters match, make the other
            # one and add the result to "ans"
            if s[i] == t[j]:
                ans += uniqueSubsequences(i + 1, j + 1)

            # Cache the answer and return
            memo[i, j] = ans
            return ans

        return uniqueSubsequences(0, 0)
```
- My recursive DP Solution:
```python
class Solution:
    # s="tom", t="to"
    # M=3, N=2
    # i=0, j=0 return recurse(0,0)==1 -> return ans=1 (1,0) return 0 + (1,1) return 1 -> (2,0) return ans=0 -> (3,0) return ans=0 -> return 0
    # -> (1,1) -> (2,1) return ans=0 + (2,2) return 1 -> return 1
    # -> (2,1) return ans=0 -> (3,1) -> return 0
    def numDistinct(self, s: str, t: str) -> int:
        M = len(s)
        N = len(t)
        def recurse(i,j):
            if(i==M or j==N):
                # if we are at the end of t we found a subsequence
                return 1 if(j==N) else 0
            ans = 0
            if(s[i] == t[j]):
                # if a character matches you can skip it in s
                # or you can move pointers for s and t forward
                ans += recurse(i+1, j) + recurse(i+1, j+1)
            else:
                # characters do not match
                # skip that character in s
                ans += recurse(i+1, j)
            return ans
        return recurse(0,0)
```
- TODO: make the iterative solution for this problem from the recursive calls and their results used above. We will mimic the recursive calls by using a 2D matrix.
- [Unique Paths](https://leetcode.com/problems/unique-paths/description/?envType=problem-list-v2&envId=dynamic-programming)
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        grid = []
        for i in range(m):
            grid.append([0]*n)
        grid[m-1][n-1] = 1
        directions = [(-1,0),(0,-1)]
        stack = {(m-1,n-1)}
        while stack:
            # find neighbors and add count from origin node
            neighbors = set()
            while stack:
                node = stack.pop()
                for d in directions:
                    x=node[0]+d[0]
                    y=node[1]+d[1]
                    if(x>=0 and y>=0 and x<m and y<n):
                        grid[x][y] += grid[node[0]][node[1]]
                        neighbors.add((x,y))
            stack.update(neighbors)
        #     print('stack',stack)
        # print('grid',grid)
        return grid[0][0]
        

    
'''
At each position the robot has two choices to make. 
Start at the bottom right and work backwards to top left.
The bottom right cell will be 1. Now we work back wards by moving left and up to the top left. Each new cell on first encounter has 0 but we add the value of the previous cell to it.
* We have to process the nodes in the stack first before updating the stack with the neighbors we discovered. This is because we want the final count of the neighbors before using the neighbors to discover further neighbors*
* Also I did not use a list based stack because then I would have to make sure I did not add the same node twice. With a list based node it could have been the normal pop and update procedure instead of popping everything then updating.
'''
```
- Unique Paths II
```python
'''
My solution, a less space efficient solution, but still a good one.
Time: O(M*N) because we process each node once. 
Space: O(M*N) because we have to store every node
'''
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        row_l=len(obstacleGrid)
        col_l=len(obstacleGrid[0])
        if(obstacleGrid[row_l-1][col_l-1]==1 or obstacleGrid[0][0]==1):
            return 0
        for i in range(row_l):
            for j in range(col_l):
                obstacleGrid[i][j] *= -1
        directions = [(-1,0),(0,-1)]
        neighbors=set()
        def getNeighbors(m,n,neighbor_list):
            for d in directions:
                x=m+d[0]
                y=n+d[1]
                if(x<row_l and y<col_l and x>=0 and y>=0 and obstacleGrid[x][y]!=-1):
                    obstacleGrid[x][y] += obstacleGrid[m][n]
                    neighbor_list.add((x,y))        
        obstacleGrid[row_l-1][col_l-1] = 1
        getNeighbors(row_l-1, col_l-1,neighbors)
        while neighbors:
            # to prevent double counting of a neighbor process all neighbors existing in list first
            temp_neighbors=set()
            while neighbors:
                x,y = neighbors.pop()
                getNeighbors(x,y,temp_neighbors)
            neighbors = temp_neighbors
        return obstacleGrid[0][0]
```
```python
'''
Official leetcode Solution
Time: O(M*N)
Space: O(1)
TODO: Implement this without looking at it when I come back!
'''
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:

        m = len(obstacleGrid)
        n = len(obstacleGrid[0])

        # If the starting cell has an obstacle, then simply return as there would be
        # no paths to the destination.
        if obstacleGrid[0][0] == 1:
            return 0

        # Number of ways of reaching the starting cell = 1.
        obstacleGrid[0][0] = 1

        # Filling the values for the first column
        for i in range(1, m):
            obstacleGrid[i][0] = int(
                obstacleGrid[i][0] == 0 and obstacleGrid[i - 1][0] == 1
            )

        # Filling the values for the first row
        for j in range(1, n):
            obstacleGrid[0][j] = int(
                obstacleGrid[0][j] == 0 and obstacleGrid[0][j - 1] == 1
            )

        # Starting from cell(1,1) fill up the values
        # No. of ways of reaching cell[i][j] = cell[i - 1][j] + cell[i][j - 1]
        # i.e. From above and left.
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 0:
                    obstacleGrid[i][j] = (
                        obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1]
                    )
                else:
                    obstacleGrid[i][j] = 0

        # Return value stored in rightmost bottommost cell. That is the destination.
        return obstacleGrid[m - 1][n - 1]
```