1. [[Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)](https://leetcode.com/problems/substring-with-concatenation-of-all-words/?envType=company&envId=amazon&favoriteSlug=amazon-six-months)
```python
class Solution:
    # I can do a character by character match for every word then remove the word that was matched then go to the next word
    # for each character try the word in words n*n

    # generate all permutations of words
        # go through all permutations and try to match at current index in s
        # ["ab","cd","ef"]
        # "abcdef", "cdefab", "efabcd"
        # "abefcd", "cdabef", "efcdab"
        # ["ab","cd","ef", 'gh','hi','lm'] => 6*5*4*3*2*1 = 720
    def generate_permutations(self, arr):
        # Base case: if there's only one element, return it as the only permutation
        if len(arr) == 1:
            return [arr]
        # List to store all permutations
        permutations = []
        # Iterate through the elements, treating each as the first element
        for i in range(len(arr)):
            # Remove the current element
            current = arr[i]
            # Remaining elements
            remaining = arr[:i] + arr[i+1:]
            # Generate all permutations of the remaining elements
            for perm in self.generate_permutations(remaining):
                permutations.append([current] + perm)
        return permutations

    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        p = 0 # pointer to start
        permutations = self.generate_permutations(words) # time complexity??
        n = len(''.join(words))
        perm_s_arr = []
        for perm in permutations:
            perm_s_arr.append(''.join(perm))
        # print('perm_s_arr',perm_s_arr)
        output = []
        for i in range(len(s)-(n-1)):
            for perm_s in perm_s_arr:
                if perm_s == s[i:i+n]:
                    output.append(i)
                    break
        return output
```
- Two Pointer Sliding Window Approach. Time Limit exceeded on last test case!
```python
from collections import Counter

# two pointer
# words are all the same length
# see if word exists in hashtable reduce count of hashtable and total count
    # if word does not exist left moves forward by word length.
# if totalCount becomes 0 we have a valid permutation
# reset counts, and total count
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        # sliding window with a hashtable of counts
        counts = Counter(words)
        totalCounts = len(words)
        inc = len(words[0])
        enc={}
        encCounts=0
        l= r = 0
        output=[]
        while r<len(s):
            possibleWord = s[r:r+inc]
            if possibleWord in counts and enc.get(possibleWord, 0) < counts[possibleWord]:
                r = r+inc
                # found match expand window
                encCounts += 1
                enc[possibleWord] = enc.get(possibleWord, 0)+1
                if encCounts==totalCounts:
                    # found permutation substring
                    output.append(l)
                    # reset contract window by moving up left pointer
                    l = l+1
                    r=l
                    encCounts=0
                    enc={}
            else:
                # no match contract the window past the mismatched character
                l+=1
                r=l
                # we may need to reset here too!
                if len(enc) > 0:
                    encCounts=0
                    enc={}
        return output
```
### **The Answer**
- The most bizarre idea. Sliding window concepts are maintained. We have resetting the window completely and then expanding the window with a fixed expansion size.
- We can try all possible sets of word length in a string. If we move a for loop by the wordLength
- s="abbbabaab", words=["ab","ba","ba"]
	- If we start at index 0 we can break s into "ab", "bb", "ab","aa","b" left over *not included*. We can start at index 1 and break s into "bb", "ba","ba","ab". See!! We found the answer by starting at index 1. The answer for substring starts at index 3.
		- We can see that we only need to go from 0 to wordLength. If I start at index 2 s will break down into "bb","ab","aa", "b" leftover *not included* which is a repeat of starting at index 0. 
```python
from collections import Counter

# two pointer
# words are all the same length
# see if word exists in hashtable reduce count of hashtable and total count
    # if word does not exist left moves forward by word length.
# if totalCount becomes 0 we have a valid permutation
# reset counts, and total count
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        wordLength = len(words[0])
        def search(i,output):
            # sliding window with a hashtable of counts
            counts = Counter(words)
            totalCounts = len(words)
            l= r = i
            while r<len(s):
                possibleWord = s[r:r+wordLength]
                r=r+wordLength
                if possibleWord in counts:
                    totalCounts -= 1
                    counts[possibleWord] -= 1
                    while counts[possibleWord]<0:
                        # move past excess word
                        counts[s[l:l+wordLength]] += 1
                        totalCounts += 1
                        l+=wordLength
                    if totalCounts==0:
                        # found permutation substring
                        output.append(l)
                        # contract the window
                        counts[s[l:l+wordLength]] += 1
                        totalCounts += 1
                        l = l+wordLength
                else:
                    # no match contract the window past the mismatched character
                    l=r
                    counts = Counter(words)
                    totalCounts = len(words)
        output=[]
		# the bizarre idea is this for loop
        for i in range(wordLength):
            search(i, output)
        return output
```