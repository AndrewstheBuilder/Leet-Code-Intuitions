- Combinations II
	- This solution is hard to understand where we avoid duplicates so I need to go back through it.
	  
```python
	class Solution:
    # [1,6,1]
    # => i=0, [1] -> i=1, [1,6] -> i=2, [1,6,1] => append([1,6,1])
    #                             -> i=3, [1,6] append([1,6]) *
    #               -> i=2, [1,1] => append([1,1])
    #               -> i=3, [1] append([1])
    #   => i=1, [6] -> i=2, [6,1] => append([6,1]) *
    #               -> i=3, [6] append([6])
    #   => i=2, [1] -> [1] append([1])
    #   => i=3, [] append([])
    # ^ will sorting fix the duplicate problem * above?
    # [1,1,1,6]
    # => 
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        # generate combinations using recursive backtracking
            # only add combinations that add up to target
            # this currently treats order as part of the uniqueness of a combination
        output = []
        # sort the array to avoid duplicates
        candidates.sort()
        def recurse(arr, curr, total, index):
            if(total > target):
                return # invalid path
            elif(total == target):
                output.append(curr[:])
            else:
                for i in range(index, len(arr)):
                    # no need to consider the same number for a combination again.
                    # avoid duplicate combinations with respect to curr
                    if i > index and arr[i] == arr[i-1]:
                        continue
                    curr.append(arr[i])
                    total += arr[i]
                    recurse(arr, curr, total, i+1)
                    total -= curr.pop() # backtracking
        recurse(candidates,[], 0, 0)
        return output
```
```python
'''
candidates = [1,6,1,1], target=8
[1,1,1,2,6] candidates sorted
(call 1) i=0, [] -> (call 2) i=0, [1] -> call(3) i=1, [1,1] ->(call 4)i=2, [1,1,1] ->(call 5)i=3, [1,1,1,6] ->(call 6)i=4 return invalid path
                                                                                     ->|(call 5)i=4, [1,1,1] return @ end
                                                              ->|(call 4)i=3, [1,1,6] |(call 4)i=4, [1,1] return @ end ->(call 7)i=4, [1,1,6] total==target append([1,1,6] return
                                        ->|(call 3)i=2, [1] continue|(call 3)i=3, [1] |(call 3) i=4, [1] return @ end ->(call 8) i=4, [1,6] return @ end
                   ->|(call 2) i=1, [] continue |(call 2) i=2, []continue|i=3, []|i=4, [] return @ end -> (call 9) i=4, [6] return @ end
'''
# redo this but with a better system of writing:
    # how to show changes in a single instance of function
    # how to show that we are in a new function

# back tracking already tries unique combinations if you look at the combinations of indices they are all sets of unique indices. The problem becomes when you have duplicate elements in those indices. So we are letting the recursive call take care of looking at a combination with duplicate elements once. Then in a iteration where we have the startIndex of that function call defined we are making sure we do not add any elements that are the same as the one that came before it(which is why we need to do the sorting of candidates initially before any recursive calls)

```