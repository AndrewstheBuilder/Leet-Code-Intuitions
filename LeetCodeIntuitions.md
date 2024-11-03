Problem: Longest Substring Without Repeating Characters

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
