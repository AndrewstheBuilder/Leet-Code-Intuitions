1. Longest Common Prefix
```python
'''
Hash table Solution
'''
    def longestCommonPrefix(self, arr1: List[int], arr2: List[int]) -> int:
        # generate all prefixes from arr1 -> store in hash set
        prefix = set()
        for i in range(len(arr1)):
            num = arr1[i]
            while num != 0:
                prefix.add(num)
                num = num//10
        # Computer max prefix length: look at prefixes in arr2 if match in hash set get length of prefix
        max_prefix = 0
        for j in range(len(arr2)):
            num = arr2[j]
            while num != 0:
                if(num in prefix):
                    len_digits = len(str(num))
                    max_prefix = max(len_digits, max_prefix)
                    break
                num = num//10
        return max_prefix
```
```python
'''
Trie Implementation
Uses a Trie class that has Trie Nodes we store
a trie node in the current trie node array to indicate we have 
encountered a digit
'''
class TrieNode:
    def __init__(self):
        self.children = [None] * 10

class Trie:
    def __init__(self):
        self.root=TrieNode()
        self.children = None

    def insert(self, num):
        node = self.root
        num_str = str(num)
        for digit in num_str:
            sti = int(digit)
            if node.children[sti] == None:
                node.children[sti] = TrieNode()
            node = node.children[sti]

    def find_longest_prefix(self, num):
        node = self.root
        num_str = str(num)
        len = 0
        for digit in num_str:
            sti = int(digit)
            if(node.children[sti] != None):
                len += 1
                node = node.children[sti]
            else:
                return len
        return len
        
class Solution:
    def longestCommonPrefix(self, arr1: List[int], arr2: List[int]) -> int:
        trie = Trie()
        for num in arr1:
            trie.insert(num)
        longest_prefix = 0
        for num in arr2:
            prefix_len = trie.find_longest_prefix(num)
            longest_prefix = max(prefix_len, longest_prefix)
        return longest_prefix
```