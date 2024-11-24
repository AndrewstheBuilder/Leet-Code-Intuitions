1. Problem: Longest Substring Without Repeating Characters

Solution: Sliding Window
``` python
def length_of_longest_substring(s: str) -> int:
	# Dictionary to store the last seen position of each character
	char_index = {}
	# Initialize pointers for the sliding window
	left = 0
	max_length = 0

	for right in range(len(s)):
    	# If the character is already in the window, move the left pointer
    	if s[right] in char_index and char_index[s[right]] >= left:
        	left = char_index[s[right]] + 1

    	# Update the last seen position of the current character
    	char_index[s[right]] = right
    	# Update the maximum length
    	max_length = max(max_length, right - left + 1)

	return max_length
```

Intuition: you are sliding a continuous window across the string. If you find a match you put the left pointer past the character you found a max on. And you update the char_index where you found the character which takes care of the case where you found a duplicate character and updated the left pointer and having a unique character.

2. Problem: Remove Duplicates from sorted array

Solution: Go through the array and remove the duplicate
``` python
 def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        past = None
        i = 0
        while i<len(nums):
            # print('past',past)
            # print('nums[i]',nums[i])
            # print('----------------')
            if(nums[i] == past):
                past = nums.pop(i)
            else:
                past = nums[i]
                i += 1
        return len(nums)
```
Intuition: If you are popping a digit from the array that is basically the same as moving forward. THe array length decreases by 1 but you are keeping the i the same so you are onto the next element in the array. That is why only the else statement has i+=1

3. Problem: [https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/submissions/1442054017/]

Solution: Go through the string and test each substring
```python
def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        if(len(needle)>haystack):
            return -1
        i = 0
        while i < len(haystack):
            if(haystack[i:len(needle)+i] == needle):
                return i
            else:
                i += 1
        return -1
```
Intuition: I made a mistake by skipping the len of the strength by updating i = i + len(needle) but the current word I am testing might be wrong but have the start of needle in it. like missiissipi search for **issi** if I skip the length of issi after testing missi I would end up at the next **I** and not the first occurrence of **issi**

4. Arrays Problem: https://leetcode.com/problems/merge-sorted-array/solutions/5714203/video-simple-solution-coding-exercise/?envType=study-plan-v2&envId=top-interview-150

Solution: Using three pointers. Two pointers for the end of each array. And the third pointer to include the padding for merging in nums1. Iterate from the back of both arrays. And move the largest element to the end.
``` python
def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        p1 = m - 1  # Last index of the initial elements in nums1
        p2 = n - 1  # Last index of nums2
        p = m + n - 1  # Last index of nums1

        # Merge the arrays starting from the end
        while p1 >= 0 and p2 >= 0:
            # Debug statements (optional)
            # print(f"nums1: {nums1}")
            # print(f"nums2: {nums2}")
            # print(f"Comparing nums1[{p1}] = {nums1[p1]} and nums2[{p2}] = {nums2[p2]}")

            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1

        # If nums2 still has elements, copy them over
        # No need to copy nums1's elements since they are already in place
        while p2 >= 0:
            nums1[p] = nums2[p2]
            p2 -= 1
            p -= 1
```
Intuition: Using three pointers and starting from the end to move the largest element to the end avoids shifting elements over after insertion. Since we know both arrays are sorted in increasing order and the largest element has to go to the end.

5. Linked List Problem: https://leetcode.com/problems/merge-k-sorted-lists/description/

6. (Longest Palindromic Substring)[https://leetcode.com/problems/longest-palindromic-substring/description/]

Solution that times out:
``` python
def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        # gather all possible contiguous substrings
        cs = []
        for i in range(len(s)):
            cs.append(s[i])
            for j in range(i+1, len(s)):
                prevElement = cs[-1]
                newElement = prevElement+s[j]
                cs.append(newElement)
        # print('all substrings:',cs)
        longestSubLen = 0
        ans = ""
        for i in range(len(cs)):
            # determine if palindrome
            c = cs[i]
            p1=0
            p2=len(c)-1
            is_palindrome = True
            while p1 < p2:
                if(c[p1]!=c[p2]):
                    is_palindrome=False
                    break
                p1+=1
                p2-=1
            if(is_palindrome and len(c)>longestSubLen):
                longestSubLen = len(c)
                ans = c
        return ans
```

Intuition: Generate all possible substrings. Determine if a substring is a palindrome by having two pointers starting at the opposite end of the string. If the pointers become equal we do not have to check that entry because self=itself. How do I make this perform faster? I think the substring creation has to be modified to be faster. Currently makes it O(n^2)

Solution that works:
```python
def longestPalindrome(s):
    # Helper function to expand around the center and return the longest palindrome
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        # Return the substring that is the palindrome
        return s[left + 1:right]

    largest = ""
    for i in range(len(s)):
        # Odd-length palindromes (expand around one character center)
        odd_palindrome = expand_around_center(i, i)
        if len(odd_palindrome) > len(largest):
            largest = odd_palindrome
        
        # Even-length palindromes (expand around two characters center)
        even_palindrome = expand_around_center(i, i + 1)
        if len(even_palindrome) > len(largest):
            largest = even_palindrome
    
    return largest
```

Intuition: 
   
7. [Jump Game Two](https://leetcode.com/problems/jump-game-ii/description/?envType=problem-list-v2&envId=dynamic-programming)

Solution: 
``` python
def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        def recurse(i, n, arr):
            if(i==len(arr)-1):
                ans.append(n)
                return
            for x in range(arr[i]):
                if(i+x <= len(arr)-1):
                    recurse(i+x, n+1, arr)
        ans = []
        recurse(0,0,nums)
```
This solution goes past the maximum recursion depth. It does not work.
But intuitively it makes sense. I am building up the recursion tree with all the possibilies. Then I will have all of it in ans. And I will pick the lowest entry in ans.

9. Rotational Cipher

Solution: Using ASCII arthimetic
``` python
def rotationalCipher(input_str, rotation_factor):
    if input_str is None:
        return ""
    
    new_str = []
    for char in input_str:
        if char.isdigit():
            # Rotate digits
            new_char = chr(((ord(char) - ord('0') + rotation_factor) % 10) + ord('0'))
            new_str.append(new_char)
        elif char.isalpha():
            # Rotate letters
            if char.islower():
                new_char = chr(((ord(char) - ord('a') + rotation_factor) % 26) + ord('a'))
            else:
                new_char = chr(((ord(char) - ord('A') + rotation_factor) % 26) + ord('A'))
            new_str.append(new_char)
        else:
            # Non-alphanumeric characters remain unchanged
            new_str.append(char)
    
    return ''.join(new_str)
```

10. Valid Word Abbreviation: String Easy problem from Meta Leet Code

My Solution Time: O(n), Space: O(n):
``` python
def validWordAbbreviation(self, word, abbr):
        skip_char = []
        numberEncountered=[]
        for a in range(len(abbr)):
            if(abbr[a].isalpha()):
                if(len(numberEncountered)!=0):
                    if(numberEncountered[0]=="0"):
                        # leading zero encountered
                        return False
                    skip_char.append(''.join(numberEncountered))
                    numberEncountered=[] # reset numberEncountered
                skip_char.append(abbr[a])
            elif(abbr[a].isdigit()):
                if(len(numberEncountered)>0 and numberEncountered[0]=="0"):
                    return False # leading zero encountered
                numberEncountered.append(abbr[a])
        if(len(numberEncountered) != 0):
            # if number encountered at end abbr
            if(numberEncountered[0]=='0'):
                return False # leading zero encountered or empty skip
            skip_char.append(''.join(numberEncountered))

        i = 0
        while i < len(word):
            if(len(skip_char)==0):
                # abbr does not match word
                return False
            skip = skip_char.pop(0)
            if(skip.isdigit()==False):
                if(skip!=word[i]):
                    # abbr does not match
                    return False
                i+=1
            else:
                i+=int(skip)
                if(i>len(word)):
                    return False # we skipped too far
        return len(skip_char)==0
```

Better Solution using pointers Time: O(n), Space:O(1)
```python
def validWordAbbreviation(self, word: str, abbr: str) -> bool:
    i, j = 0, 0  # `i` for word, `j` for abbr
    
    while j < len(abbr):
        if abbr[j].isalpha():
            # If there's a mismatch in characters
            if i >= len(word) or word[i] != abbr[j]:
                return False
            i += 1
            j += 1
        elif abbr[j].isdigit():
            # Leading zero check
            if abbr[j] == '0':
                return False
            # Build the number from digits
            num = 0
            while j < len(abbr) and abbr[j].isdigit():
                num = num * 10 + int(abbr[j])
                j += 1
            i += num  # skip the characters in word
            if i > len(word):  # if skip goes beyond word length
                return False
        else:
            return False  # invalid character in abbr

    return i == len(word)
```

An even better solution built on top of the chat gpt one. Code is more succinct: Time O(n), Space: O(1)
```python
i = j = 0

        while j<len(abbr) and i<len(word):
            if(abbr[j].isalpha()):
                if(word[i] != abbr[j]):
                    return False # the abbr and word does not match 
                i += 1
                j += 1
            elif abbr[j].isdigit():
                if abbr[j] == '0':
                    return False # leading 0
                num = 0
                while j<len(abbr) and abbr[j].isdigit():
                    num = num * 10 + int(abbr[j])
                    j += 1
                i += num # skip ahead
            else:
                return False # invalid character encountered
        return i == len(word) and j == len(abbr)
```

11. Minimum Remove to Make Valid Parenthesis (Medium String (Problem)[https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days])

Not Optimal Solution:
``` python
def minRemoveToMakeValid(self, s):
        """
        :type s: str
        :rtype: str
        """
        st = []
        invalid_indices=[]
        res = []

        # find invalid parenthesis
        for i in range(len(s)):
            if(s[i]=="("):
                st.append(i)
            elif(s[i]==")" and len(st)>0):
                st.pop() # pop the last inserted element
            elif(s[i]==")"):
                invalid_indices.append(i)
        
        if(len(st)>0):
            invalid_indices.extend(st)

        # construct the string without the invalid indices
        for i in range(len(s)):
            if(i not in invalid_indices):
                res.append(s[i])
        
        return "".join(res)
```
- Time: O(n^2) -> because of the last for loop we are looking in invalid_indices which in the worst case could contain n elements
- Space: O(n)

Optimal Solution with regards to time complexity by using Set
```python
``` python
def minRemoveToMakeValid(self, s):
        """
        :type s: str
        :rtype: str
        """
        st = []
        invalid_indices=set()
        res = []

        # find invalid parenthesis
        for i in range(len(s)):
            if(s[i]=="("):
                st.append(i)
            elif(s[i]==")" and len(st)>0):
                st.pop() # pop the last inserted element
            elif(s[i]==")"):
                invalid_indices.add(i)
        
    	invalid_indices.update(st)

        # construct the string without the invalid indices
        for i in range(len(s)):
            if(i not in invalid_indices): # O(1) lookup in set
                res.append(s[i])
        
        return "".join(res)
```
- Time Complexity: O(n)
- Space Complexity: O(n)

More optimal solution. Save on space
```python
def minRemoveToMakeValid(self, s):
    """
    :type s: str
    :rtype: str
    """
    s = list(s)  # Convert string to list for mutability
    stack = []

    # First pass: Mark invalid parentheses
    for i in range(len(s)):
        if s[i] == '(':
            stack.append(i)  # Store index of '('
        elif s[i] == ')':
            if stack:  # Valid closing parenthesis
                stack.pop()
            else:  # Invalid closing parenthesis
                s[i] = ""  # Mark for removal

    # Remove leftover unmatched '(' by using indices in the stack
    while stack:
        s[stack.pop()] = ""

    # Return the string after removing invalid parentheses
    return "".join(s)
```
Above code modifies the string in place to have space complexity of O(n). We are converting the string to a new list.

Overall Intuition: find indices of invalid parenthesis. Construct the string without the invalid indices


12. Basic Calculator II Medium String Parsing expression Problem [Link](https://leetcode.com/problems/basic-calculator-ii/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)

Broken Solution I basically cannot evaluate this expression. This was my first idea though
```python
# first for loop to parse integers and operators to build expr
# second for loop evaluate expr
import operator

# Map operators to their corresponding functions
operator_map = {
    '+': operator.add,
    '-': operator.sub,
    '*': operator.mul,
    '/': operator.floordiv,  # Use floordiv for truncated division
}
i = 0
expr = []

while i < len(s):
    if(s[i].isdigit()):
        num=[s[i]]
        i += 1
        while(i<len(s) and s[i].isdigit()):
            num.append(s[i])
            i+=1
        expr.append(int(num))
    elif(s[i] in operator_map):
        expr.append(operator_map[s[i]])
        i += 1
    else:
        # ignore spaces
        i += 1

# evaluate expression
```
Working Solution: Create postfix expression from infix and evaluate as you are creating postfix
``` python
def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        # create post fix array
        prec = {'+':1, '-':1, '*':2, '/':2}
        opr_a = {'+': operator.add, '-': operator.sub, '*':operator.mul, '/':operator.floordiv}
        out=[]
        opr=[]
        i = 0

        # evaluate infix operation using postfix iteration
        while i < len(s):
            if s[i].isdigit():
                num = [s[i]]
                i += 1
                # iterate until we get all the indiv digits of the number
                while(i<len(s) and s[i].isdigit()):
                    num.append(s[i])
                    i += 1
                out.append(int("".join(num)))
            elif s[i] in prec:
                while len(opr)>0 and prec[opr[-1]] >= prec[s[i]]:
                    n2 = out.pop()
                    n1 = out.pop()
                    res = opr_a[opr.pop()](n1, n2)
                    out.append(res)
                opr.append(s[i])
                i+=1
            else:
                i+=1
            
        while len(opr)>0:
            n2 = out.pop()
            n1 = out.pop()
            res = opr_a[opr.pop()](n1, n2)
            out.append(res)

        return out[0]
```

- [Valid Palindrome II Problem](https://leetcode.com/problems/valid-palindrome-ii/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days&role=full-stack)
- TIme Limit Exceeded Solution and maybe using more space than necessary
```python
def validPalindrome(self, s):
    def isPalindrome(ss):
        if(ss==ss[::-1]):
            return True
        return False
    
    # generate all possiblities of words with None or one char removed
    res = isPalindrome(s)
    if(res):
        return res
    for i in range(len(s)):
        res = isPalindrome(s[:i]+s[i+1:])
        if(res):
            return True
    return False

# time: O(n^2)
# space: O(n)
```
- Optimal Solution O(n): time, O(1): space
```python
    def validPalindrome(self, s):
        def isPalindromeRange(left, right):
            while left < right:
                if s[left] != s[right]:
                    return False
                left += 1
                right -= 1
            return True
        
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                # Try skipping left or right
                return isPalindromeRange(left+1, right) or isPalindromeRange(left, right-1)
            left += 1
            right -= 1
        return True
```
- Intuition: we can delete at most 1 char. We can either go down a path where we delete the right char when the chars don't match or the left char. We have already verified the ends of the string if we are in the midst of iteration. Otherwise we can have a char we skip at the ends. Example: 'abaz' or in the midst example: 'abza'. -> the midst example becomes a sub problem as iteration happens. 1. 'abza' 2. 'bz' 3. 'b'

- Problem Binary Tree Vertical Order Traversal [Problem](https://leetcode.com/problems/binary-tree-vertical-order-traversal/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)

- Solution
``` python
def verticalOrder(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: List[List[int]]
        """
        # boundary condition
        if(root==None):
            return []

        # store the column of each node
        # while doing bfs
        q = deque([(0, root)])
        d = {}
        while(q):
            (coln, n) = q.popleft()
            if(coln not in d):
                d[coln] = []
            d[coln].append(n.val)
            if(n.left):
                q.append((coln-1,n.left))
            if(n.right):
                q.append((coln+1, n.right))
        # get the sorted list of keys
        # input into resulting array what was gathered previously in dict
        res = []
        i = 0
        sorted_keys = sorted(d.keys())
        for key in sorted_keys:
            res.append([])
            res[i] = d[key]
            i += 1
        return res
```
Time Complexity: O(nlogn) because of sorting
Space Complexity: O(n) because of various data structures used to store results
- The intuition here is to do a breadth first visiting of each node which means it will visit each node from root to left child to right child until the end. And store the corresponding visits with column count.

- Solution without sorting would be time complexity O(n)...
``` python
        # boundary condition
        if(root==None or root.val==None):
            return []

        # store the column of each node
        # while doing bfs
        q = deque([(0, root)])
        d = {}
        # keep track of min and max col and you won't have to sort the dict at the end
        min_col = 0
        max_col = 0
        while(q):
            (coln, n) = q.popleft()
            if(coln not in d):
                d[coln] = []
            if(coln>max_col):
                max_col=coln
            if(coln<min_col):
                min_col=coln
            d[coln].append(n.val)
            if(n.left):
                q.append((coln-1,n.left))
            if(n.right):
                q.append((coln+1, n.right))
        res = []
        for i in range(min_col, max_col+1):
            res.append(d[i])
        return res
```
- Intuition: keep track of min and max and since each new column is +1 or -1 from previous column you can iterate from min_col to max_col+1 and cover all the columns you needed

- [Problem Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- Solution
```python
    def levelOrder(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: List[List[int]]
        """
        if(root==None):
            return []
        q = deque([(1, root)])
        res = []
        res.append([root.val])

        while(q):
            lvln, n = q.popleft()
            if(lvln>=len(res) and (n.left or n.right)):
                res.append([])
            if(n.left):
                res[lvln].append(n.left.val)
                q.append((lvln+1, n.left))
            if(n.right):
                res[lvln].append(n.right.val)
                q.append((lvln+1, n.right))
        return res
```
- Time Complexity: O(n), Space: O(n)
- Intuition: Do BFS while keeping track of what level you are at
- TODO: do BFS with recursion

- [Divide Two Integers Problem](https://leetcode.com/problems/divide-two-integers/)
- First Attempted Solution Time Limit Exceeded
```python
def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        neg = 0

        # make the inputs negative to avoid issues when subtracting
        # making them positive would cause an integer overflow issue
        # keep track of number of negatives
        if(dividend<0):
            neg += 1
        else:
            dividend = -dividend
        if(divisor<0):
            neg += 1
        else:
            divisor = -divisor

        if(divisor<dividend):
            # divisor is a bigger value than dividend 
            # return trunacated decimal quotient
            return 0

        doubled = divisor
        q = 1
        
        # make sure next doubling does not go over dividend
        while(doubled+doubled >= dividend):
            q += q
            doubled += doubled
        remainder=dividend-doubled

        # after doubling go through remainder with subtracting by divisor
        if(remainder != 0):
            while(remainder-divisor<=0):
                q += 1
                remainder -= divisor
        
        i_min = -2**31
        i_max = (2**31)-1
        if(neg==1):
            if(-q < i_min):
                return i_min
            return -q
        if(q>i_max):
            return i_max
        return q
```
- Intuition: We are going to subtract the divisor from the dividend until we get the quotient. To speed this process up double the divisor and stop before it becomes too big. Then we take care of the remainder with subtracting remainder - divisor. Make both the dividend and divisor be negative because making a negative into a positive comes with the risk of integer overflow according to the constraints of the problem.
- What is the time complexity of this? I think it is O(logn) because the the doubling that happens in the for loop can in the worst case where divisor is 1 take care of everything. The remainder subtraction loop is O(k). n being the dividend.
- Time Complexity: n would be the (dividend/divisor). the doubles loop is O(logn) but the remainder loop might be O(n/2) which is just O(n) not good!

- Better Solution that is accepted
- ```python
  def divide(self, dividend, divisor):
        if(dividend==0):
            return 0

        max_i = 2**31 - 1
        min_i = -2**31
        half_min_i = min_i >> 1 #divide by two by right shifting

        # special edge case overflow
        if (dividend==min_i and divisor==-1):
            print('special case')
            return max_i
                
        neg = 2
        if(dividend>0):
            neg -= 1
            dividend = -dividend
        
        if(divisor>0):
            neg -= 1
            divisor = -divisor
            
        q = 0
        while(divisor>=dividend):
            d = divisor
            n_d = 1
            while(half_min_i<=d and d+d>=dividend):
                n_d+=n_d
                d += d
            q += n_d
            dividend -= d

        if(neg==1):
            return -q
        return q
  ```
  - Time Complexity: O(log^2n) with n being the dividend
  - Space Complexity: O(1)
 
  - [First Leet Code Hard Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/description/)
  - Inefficient and Wrong Solution
  ``` python
	def subStringIndices(s, words):
	    def check(hash_t, index, l_word, n_words, s):
	        j = index
	        res = []
	        for _ in range(n_words):
	            r = s[j:j+l_word]
	            if(r not in hash_t):
	                # unknown word found in s
	                return []
	            if(hash_t[r] == 0):
	                return [] # encountered too many of one word
	            hash_t[r] -= 1
	            res.append(j)
	            j = j+l_word
	        return 
	    
	    hash_og=collections.Counter(words)
	    l_word = len(words[0])
	    n_words = len(words)
	    substring_len = l_words * n_words
	
	    for i in range(len(s)-substring_len):
	        res = check(dict(hash_og), i, l_word, n_words, s)
	        if(res):
	            # found concantenated substring
	            return res
	    return []
    # Time Complexity: O(n^2)
    # space complexity: O(n^2) creating new hashtable everytime we call check()
  # above time and space complexity may be incorrect we have to consider all the input terms that are being iterated on. word length, words length, length of s
  ```
- Intuition: go through each index of the string see if the substring starts at that index. Store a hash table of all the words and the number of their occurrences. Use that to check if the substring thats found matches the words and the number of their occurrences. (order does not matter)
- My solution is wrong too. Its returning the index of the beginning of each char in words in the substring. TODO: try this one again

- [Problem Next Permutation]()
- Incorrectish solution:
- ``` python
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if(len(nums)==1):
            return
        def swap(i,j,arr):
            temp = arr[i]
            arr[i] = arr[j]
            arr[j] = temp
        def reverse(start,arr):
            i,j = start,len(nums)-1
            while(i<j):
                swap(i,j,arr)
                i += 1
                j -= 1

        j = len(nums)-2
        while j >-1 and nums[j+1]<nums[j]:
            # traverse array from right to left
            j -= 1
        if(j==-1):
            # highest permutation possible
            # everything in descending order
            # reverse the arr
            reverse(0,nums)
            return
            
        # largest index (j) with nums[j]<nums[j+1] found. Found pivot
        # find smallest element to the right of j that is larger than j to replace with j
        # array to the right is already sorted in descending order
        # find the smallest element that is larger than the pivot j
        i = len(nums)-1
        while i>j and nums[i]<=nums[j]:
            i -= 1
        # swap the two elements
        swap(i, j, nums)
        # to make the smallest permutation possible reverse the order of what came after the pivot
        # everything after the pivot is in descending order for now
        # reverse
        reverse(j+1, nums)
  ```
  - Time Complexity: O(n)
  - Space Complexity: O(1) since we are adjusting in place
  - Intuition: This solution is not entirely correct, but its on the right track. When we compare items lexicographically we start from the left of both comparables and work towards the right. The element with the greater value at the first instance wins the title of lexicographically greater. [2,1,3] is greater than [1,2,3]. We are looking for the smallest next possible greater permutation that is lexicographically greater. Step 1 find the first value going from right to left (because we are trying to increase the lexicographical value the smallest possible next permutation) that is the greatest index that is in ascending order arr[j] < arr[j+1]. Step 2. Look to the right of j to find the smallest value that is greater than arr[j] perform a swap. Step 3. We know values after the pivot j are in descending order. Convert it to ascending order to ensure that its the smallest possible next permutation. This is the reverse step.
  - Current solution fails on [5,1,1] it should return [1,1,5] Which is the edge case where the entire array is in descending order so its the highest possible permutation. Reverse the entire array to go back and start again with the smallest possible permutation
  - Its because when we are finding the pivot the while loop does not have the equal sign next to it. it should be `nums[j+1]<=nums[j]` instead of `nums[j+1]<nums[j]`. We are trying to find the highest index that meets the ascending order criteria which we will when it fails this condition because `nums[j+1] will be > nums[j]`
 
- [Problem 4Sum](https://leetcode.com/problems/4sum/)
- I cannot create a combination by just using a single tested loop. Why? Combination involving k elements requires k nested loops.

- [Problem 2Sum](https://leetcode.com/problems/two-sum/)
- Inefficient O(n^2) solution:
``` python
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if(len(nums)==2):
            return [0,1]
        res = []
        # generate all combinations
        for i in range(len(nums)-1):
            for j in range(i+1, len(nums)):
                res.append([i, j])

        # go through all combinations
        for i in range(len(res)):
            if(nums[res[i][0]] + nums[res[i][1]]==target):
                return res[i]
        return res[i] # but we will not hit this
```
- Solution that works:
``` python
   def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        h = {}
        for i in range(len(nums)):
            complement = target-nums[i]
            if(complement in h):
                return [i, h[complement]]
            h[nums[i]] = i

        # we should not hit this
        return []
```
- Intuition: We are doing a single pass through nums there can be two numbers that add to target. We store in the hashtable all the values and the indexes we pass through. If we find the second number then the complement should be in the hash table. target = 1stnum + 2ndnum. Complement = target - 1stNum. Complement is also 2ndNum.

- [Problem TwoSumII](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
- Solution:
```python
    def twoSum(self, nums, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        i = 0
        j = len(nums)-1

        while(i<j):
            if(target>nums[i]+nums[j]):
                i+=1
            elif(target<nums[i]+nums[j]):
                j -= 1
            else:
                return [i+1,j+1]
```
- Intuition: Use the two pointer approach since the array is sorted. We can add the 2 candidates get a prospect and if thew prospect is greater than target then decrease the overall prospect by taking the right pointer to the left. We know the array is sorted in increasing order. 
