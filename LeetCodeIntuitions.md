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

- Solution without sorting would be time complexity O(n)...
