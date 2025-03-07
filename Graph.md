1. BFS
- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/description/?envType=company&envId=amazon&favoriteSlug=amazon-thirty-days)
```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        queue = deque()
        fresh_count = 0

        # Initialize the queue with all rotten oranges and count fresh oranges
        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 2:
                    queue.append((r, c))
                elif grid[r][c] == 1:
                    fresh_count += 1

        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        minutes = 0

        # BFS to propagate the rotting
        # we do not need to visit every cell we just need to visit the ones that are rotten
            # and rot the next oranges
        while queue and fresh_count > 0:
            prevLen = len(queue)
            for _ in range(prevLen):
                r, c = queue.popleft()
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                        grid[nr][nc] = 2
                        queue.append((nr, nc))
                        fresh_count -= 1
            minutes += 1

        return -1 if fresh_count > 0 else minutes
```
2. Using DFS for Detecting Cycles in the Graph
```python
'''
Below is a rough draft of how I began
Problem is asking if a student can finish the numCourses which has certain prerequisites
If there is a cycle that means there is a course that is a prereq and a current req so it cannot be taken.
A -> B -> C -> A 
- You can't take A because you need to take B which relies on C which relies on A
'''
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # iterate on the prerequistes backwards to get [ai,bi] bi -> ai (prev: bi)
            # [bi,ai] ai -> bi X because (ai prev is also bi so this immediately errors)
        # ai:bi
        # bi:ai
        # 15:1,2
        # 16:1
        # 1:2

        # 1 -> 0 -> 2 -> 1
        # 1 : [10,0]
        # 0 : [2] *
        # 3 : [1]
        # 2 : [1]
        # 10: []

        # i went with the idea that a cycle is only if we get back to the start node
        # 1 -> 2, 4
        # 4 -> 2
        # 2 -> 4
        # check_cycle(2 as root, 1 as start)
            # 2 -> 4 -> 2
            # check_cycle(4 as root, 2 as start)
            # check_cycle(2 as root, 4 as start)
        # how do I identify that there is a cycle?
            # I might have to use the dreaded recursion
        prereq_graph = defaultdict(set)
        for req in prerequisites:
            if req[0]==req[1] or req[0] in prereq_graph[req[1]]:
                return False
            prereq_graph[req[0]].add(req[1])
        
        # # recursively check if the graph has a cycle
        # def check_cycle(root, start):
        #     if root == start:
        #         return True
        #     for req in prereq_graph[root]:
        #         ret = check_cycle(req, root)
        #         if(ret == True):
        #             return ret # yes cycle
        #     return False # no cycle
        def has_cycle(graph):
            visited = set()
            recursion_stack = set()

            def dfs(node):
                visited.add(node)
                recursion_stack.add(node)

                # Get neighbors (handle both single value and list cases)
                neighbors = graph.get(node, [])
                if not isinstance(neighbors, list):
                    neighbors = [neighbors]

                for neighbor in neighbors:
                    if neighbor not in visited:
                        if dfs(neighbor):
                            return True
                    elif neighbor in recursion_stack:
                        return True  # Cycle detected

                recursion_stack.remove(node)
                return False

            # Check each node in case the graph is disconnected
            for node in prereq_graph.keys():
                if node not in visited:
                    if dfs(node):
                        return True
        return False

        for key in prereq_graph.keys():
            for req in prereq_graph[key]:
                ret = check_cycle(req, key)
                if ret == True:
                    return False # there is a cycle so its not possible
        return True #its possible 
        
```
```python
'''
The solution: use DFS to detect cycles. The key insight is to use the recursion stack. The recursion stack keeps track of the node we are evaluating neighbors for and if the neighbors point back to any node we are evaluating still then there is a cycle. When we are done evaluating the neighbors for a node we remove it from the recursion stack
We can however visit nodes twice in the case of a unconnected graph
	# A -> B -> C -> E -> G
	# F -> E -> G
'''
# A -> B -> C -> A
# visited=(A,B,C)
# recursion_stack=(A,B,C)
# graph={A:B, B:C, C:A}
# for node in graph.keys() 1. node=A if(dfs(A) == True) return False
# 1. dfs(A), neighbors=(B) -> dfs(B) return True, neighbors=(C) -> dfs(C) return True, neighbors=(A) return True # there is cycle

# A -> B -> C
# A -> F -> E
# graph={A:B,F, B:C, F:E}
# visited=(A,B,C,F,E)
# recursion_stack=(A)
# for node in graph.keys():
# 1. node=A dfs(A), neighbors: (B,F) neighbor=B dfs(B):return False -> dfs(B) neighbors=(C) -> dfs(C):return False  -> dfs(C) return False
# 1. node=A dfs(A), neighbors: (B,F) neighbor=F dfs(F):return False -> dfs(F) neighbors=(E) dfs(E):return False -> dfs(E) return False
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = {}
        for req in prerequisites:
            if req[0] not in graph:
                graph[req[0]] = set()
            graph[req[0]].add(req[1])

        visited = set()
        recursion_stack = set()
        def dfs(node):
            visited.add(node)
            recursion_stack.add(node)
            neighbors = graph.get(node, set())
            for neighbor in neighbors:
                if neighbor not in visited:
                    if dfs(neighbor):
                        return True
                elif neighbor in recursion_stack:
                    return True  # Cycle detected
            recursion_stack.remove(node)
            return False

        # Check each node in case the graph is disconnected
        for node in graph.keys():
            if node not in visited:
                if dfs(node):
                    return False # not possible
        return True
```

3. Using DFS to get order of courses to take and making sure there are no cycles
```python
'''
The plan + some methods I tried. I ended up needing hints and realized it needs to be topological sort.
'''
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        # 0 -> 1,2
        # 1 -> 2
        # 2 -> 4
        # 0 -> 1 -> 2 -> 3
        # [[1,0],[2,0],[3,1],[3,2], [0,3]]
        # [[1,0],[2,0],[3,0],[2,3]]
        # 0 -> 1 -> 2 -> 3

        # [[1,0],[2,0],[3,1],[3,2]]
        # {0:0,1:1,2:1,3:2}
        # [[1,0],[2,0],[3,1],[2,1],[3,2]]
        # {0:0,1:1,2:2,3:3}

        # [[2,0], [1,0], [5,0], [2,1],[3,2],[5,1],[6,1]]
        # 0 -> 2,1,5
        # 1 -> 2
        # 2 -> 3
        # 5 -> 1
        # 6 -> 1
        # visited = [] # we want it to be in order
        # [0,2,3]
        # [1,2,3]
        # [5,1,2]
        # [6,1]

        # topological sort using dfs
        # global variable numCourse
        # explore all nodes in the graph through a for loop
            # calls dfs on each node
                # when we reach the end of the graph we will assign that the numCourse and decrement numCourse
                # adding each node to visited when we visited
                # recursive_stack to make sure we do not encounter a cycle
        # [[2,0],[1,0],[4,0],[2,1],[3,2],[4,1],[5,1]]
        # create the graph
        # 0 -> 2, 1, 4 *
        # 1 -> 2, 4, 5
        # 2 -> 3 *
        # numCourse = -1 # 0-indexed number of courses global variable
        # output: [0,1,5,4,2,3] -> list that in init with length of numOfCourses
        # visited = [2,3,0,1,4,5] -> hashset
        # recursive_stack = [] -> hashset
        # for each node in graph
            # dfs(node)
        if(numCourses == 1 and len(prerequisities)==0):
            return [0]
        # create the graph
        graph = {}
        for req in prerequisities:
            if(req[1] not in graph):
                graph[req[1]] = []
            graph[req[1]].append(req[0])
        
        # do dfs where we check for cycles and create the output
        output = []
```
```python
'''
Implemented solution using Topological Sort w/ DFS
'''
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        output = []
        if(len(prerequisites)==0):
            for i in range(numCourses):
                output.append(i)
            return output
        # create the graph
        graph = {}
        for req in prerequisites:
            if(req[1] not in graph):
                graph[req[1]] = []
            graph[req[1]].append(req[0])
        
        # do dfs where we check for cycles and create the output
        output = [-1]*numCourses
        courses_remaining = set()
        for i in range(numCourses):
            courses_remaining.add(i)
        visited = set()
        recursive_stack = set()
        num = numCourses - 1

        def dfs(node):
            nonlocal num
            if node in recursive_stack:
                return False # invalid encounter
            if node in visited:
                return True # valid encounter node has been visited
            visited.add(node)
            recursive_stack.add(node)
            for child in graph.get(node, []):
                if not dfs(child):
                    return False # invalid encounter pass it up the recursive stack  
            output[num] = node
            courses_remaining.remove(node)
            num -=1
            recursive_stack.remove(node)
            return True # valid encounter
            
        for node in graph.keys():
            if node not in visited:
                if not dfs(node):
                    return []
        if courses_remaining:
            for i in range(len(output)):
                if output[i] == -1:
                    output[i] = (courses_remaining.pop())
        return output

# Time/Space: O(N+E) N being number of nodes, E being number of Edges
# Time: We have to visit each node and its edges. We visit each node and its associated edges. Its not for each node we visit all the edges the nodes and edges are both a part of each other.
# Space: the adjacency list (graph) dominates with O(N+E) it has the nodes and the edge info.
```
