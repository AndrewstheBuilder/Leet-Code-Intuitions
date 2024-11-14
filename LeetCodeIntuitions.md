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
