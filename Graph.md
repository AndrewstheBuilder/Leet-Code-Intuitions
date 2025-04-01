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

3. 207: Course Schedule - Using DFS to get order of courses to take and making sure there are no cycles. We are doing topological sort using DFS.
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
4. Graph Valid Tree
```python 
class Solution:
'''
incorrect implementation
You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.
'''
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        # a valid tree is going to have no cycles
        # there will be n node and n-1 edges
        # it will be a single tree so no disjointed graphs
        # a valid tree will have one root node
        # my assumption that the edges will be directed is wrong!
            # so this implementation is off
        if n==1 and len(edges)==0:
            return True
            
        if(n-1 != len(edges)):
            return False

        # build graph from edges
        possible_root_node = defaultdict(int)
        graph = {}
        for edge in edges:
            if(edge[0] not in graph):
                graph[edge[0]] = []
            if edge[0] not in possible_root_node:
                possible_root_node[edge[0]] = 0
            possible_root_node[edge[1]] += 1
            graph[edge[0]].append(edge[1])
        
        # Find a root or return False for invalid root
        root = None
        for key, value in possible_root_node.items():
            if value==0 and root==None:
                root = key
            elif value==0 and root!=None:
                return False # more than one root
        if(root==None):
            return False
        
        # single orphan node edge case
        if(n > len(possible_root_node.keys())):
            return False 

        # can't use dfs to detect a cycle because it is a undirected graph
        # use dfs to detect cycles.
            # if no cycles then return True
        visited = set()
        recursive_stack = set()
        def dfs(node):
            visited.add(node)
            recursive_stack.add(node)
            for child in graph.get(node, []):
                if child in recursive_stack:
                    return False # cycle detected
                # visit the edge
                if not dfs(child):
                    return False # cycle detected and passed up recursion stack
            recursive_stack.remove(node)
            return True
        ret=dfs(root)
        print('ret',ret)
        # check if we visited every node from the root 
        # otherwise its a disjointed graph
        for node in graph.keys():
            if node not in visited:
                return False
        return ret

# [[0,1],[0,2],[0,3],[1,4]]
# possible_root_node = (0:0, 1:1, 2:1, 3:1, 4:1)
# root = 0
# graph:
    # 0:1,2,3
    # 1:4
# visited: (0,1,4,2,3)
# recursive_stack: (0)

# [[0,1],[0,2],[0,3],[1,4]]
# possible_root_node = (0:0, 1:1, 2:1, 3:1, 4:1)
# root = 0
# graph:
    # 0:1,2,3
    # 1:4

# [[1,2],[2,3],[1,3]]
# 1:2,3
# 2:3
# And somehow this is said to have 4 nodes
```
```python
'''
for a undirected graph make the adjacency list have records for both
also we just need to check visited to make sure there are no cycles since each node is guaranteed to be visited only once. Only one incoming edge to each node
'''
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        # a valid tree:
            # has a single root -> we can turn the tree if it has multiple roots and it will end up with 
                # a single root so this is no problem.
            # n-1 edges for n nodes
                # each node has 1 incoming node. Above check should cover this
            # no disconnected trees allowed. Only a single valid tree
        # edges are listed in a undirected way

        # form an adjacency list representing neighbors of nodes
        graph = {}
        for edge in edges:
            if(edge[0] not in graph):
                graph[edge[0]] = []
            if(edge[1] not in graph):
                graph[edge[1]] = []
            graph[edge[0]].append(edge[1])
            graph[edge[1]].append(edge[0])
        
        # perform dfs to make sure there are no cycles
        seen = set()
        def dfs(node, parent=-1):
            seen.add(node)
            for child in graph.get(node, []):
                if child==parent:
                    continue # allow the trivial cycle of child-parent
                if child in seen:
                    return False # cycle detected
                # visit the edge
                if not dfs(child, node):
                    return False # cycle detected and passed up recursion stack
            return True
        if dfs(0):
            return len(seen) == n
        else:
            return False

# [[0,1],[0,2],[0,3],[1,4]]
# graph:
    # 0:1,2,3
    # 1:0,4
    # 2:0
    # 3:0
    # 4:1
# visited: (0,1,4,2,3)
# dfs(0,-1)
# return True

# [[0,1],[1,2],[2,3],[1,3],[1,4]]
# graph:
    # 0:1
    # 1:0,2,3,4
    # 2:1,3
    # 3:2,1
    # 4:1
# visited=(0,1,2,3)
# dfs(0,-1) return False -> dfs(1,0) return False -> dfs(2,1) return False -> dfs(3,2) child==1 return False

# Space: O(N)
# Time: O(N)
```

4. Hard graph problem: Alien Dictionary
   Topological sort using BFS
```python
'''
Here is my attempt with a trie data structure that fails
This fails when we have ["rf", "rt", "ft"]
	I am not able to order f after t
Also the constraint they implicitedly set where
we return "" if the prefix for a word comes
after the word I could not solve yet.
'''
class Node:
    def __init__(self, value, next, prev, order):
        self.value = value
        self.next = next
        self.prev=prev
        self.order=order
    
    def __str__(self):
        return f"TrieNode value={self.value}, order={self.order}"

class Solution:
    def alienOrder(self, words: List[str]) -> str:
        # untold constraint
            # the prefix of a word should be before the word itself. 
            # ["ab", "abcd"] ---> correct
            # ["abcd", "ab"] ---> incorrect
            # this makes the problem so much harder
        visited = {}
        # create the trie
        # if error return ""
        root = Node(None,set(),None,0)
        for word in words:
            curr = root
            for char in word:
                if char not in visited:
                    # create the Node
                    newNode = Node(char, set(), None, curr.order+1)
                    curr.next.add(newNode)
                    visited[char] = newNode # keep reference to node
                    curr = newNode
                else:
                    seenNode = visited[char]
                    if seenNode.order < curr.order:
                        # error
                        return ""
                    curr = seenNode

        # bfs traversal to create possible word to return
        queue = deque()
        for children in root.next:
            queue.append(children)

        result = []
        while queue:
            curr = queue.popleft()
            result.append(curr.value)
            for children in curr.next:
                queue.append(children)

        return ''.join(result)        
```
```python
'''
Here is the solution
This is a tricky problem. I did not know that I should not process a char until its inorder has run down to 0. when inorder is 0 add it to the queue and process its children. makes sense.
Also the in_order data structure + logic helps detect cycles. We will not get to all the chars if there is a cycle. if a char is a part of a cycle it will have a in_order > 0 and it will not reach zero. I am not sure how to get to this intuitively.
Now, a cycle is like a traffic jam: if Ship F waits for Ship T (f → t), but Ship T waits for Ship F (t → f), neither can move—they’re stuck in a loop. Their in_degree never hits zero because each depends on the other. In graph terms, a cycle means some nodes (characters) can’t be processed, so we don’t visit all the characters in the topological sort.
'''
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        all_chars = set()

        # get a unique set of characters
        for word in words:
            all_chars.update(word)

        # initialize all characters at the 0 order
        # this will be used to calculate where the word falls in lexicographical order
        in_order = {}
        for char in all_chars:
            in_order[char] = 0
        
        # compare word with the next word to determine relative orderings of chars
        graph=defaultdict(set)
        for i in range(len(words)-1):
            w1,w2 = words[i], words[i+1]
            if w1.startswith(w2) and len(w1) != len(w2):
                return "" # error unable to make ordering
            for c1, c2 in zip(w1,w2):
                if c1 != c2:
                    if c2 not in graph[c1]:
                        graph[c1].add(c2)
                        in_order[c2] += 1
                    # move on after first mismatch the rest of the string compares will be in ambiguous order
                    # you cannot determine the order of the remaining chars after one mismatch
                    break 

        queue = deque()
        queue.extend([c for c in all_chars if in_order[c]==0])
        result = []
        visited = set()
        # what if in_order[char] is greater than 1? --> we keep processing
        while queue:
            char = queue.popleft()
            result.append(char)
            for child in graph[char]:
                in_order[child] -= 1
                if in_order[child]==0:
                    queue.append(child)

        return ''.join(result) if len(result) == len(all_chars) else ""
    
```
- Find the Celebrity [https://leetcode.com/problems/find-the-celebrity/]
```python
# The knows API is already defined for you.
# return a bool, whether a knows b
# def knows(a: int, b: int) -> bool:

class Solution:
    def isCelebrity(self, i, n):
        for j in range(n):
            if i==j:
                continue
            if knows(i,j) or not knows(j,i):
                return False
        return True

    def findCelebrity(self, n: int) -> int:
        celebrity_candidate = 0
        for i in range(1,n):
            if knows(celebrity_candidate, i):
                celebrity_candidate = i
        if(self.isCelebrity(celebrity_candidate, n)):
            return celebrity_candidate
        return -1


# brute force
    # go through each number and its surrounding numbers (nested for loop)
        # candidates are numbers that only know itself
        # a solution is when a candidate is known by n-1 people 

# optimized below by removing candidates when i saw that a possible_candidate was unknown by somebody
# it does not seem like there is another way to optimize this I thought about DFS but I am not going to be able to find out if a candidate I just got into has n-1 "fans"


# optimal solution
    # if they are a celebrity candidate they would be known by everyone.
    # if 0 knows 1, 0 cannot be a celebrity candidate, but 1 can.
        # and if 1 does not know anyone that comes farther down then they cannot be celebrity candidates 
```

