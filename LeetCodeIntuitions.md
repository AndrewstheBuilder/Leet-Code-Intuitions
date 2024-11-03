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
