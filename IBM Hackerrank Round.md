1. Asks me to find the minimum consecutive number of elements I could remove from the array and have an array that has unique values.
- Similar to LeetCode 2260. Minimum Consecutive Cards to Pick up(Maybe I should have warmed up first before tackling the IBM challenge. Oh well. Live and Learn!)
```python
class Solution:
    def minimumCardPickup(self, cards: List[int]) -> int:
        l=0
        enc={cards[l]}
        min_ans = float('inf')
        for r in range(1,len(cards)):
            if cards[r] not in enc:
                enc.add(cards[r])
            else:
                while cards[l] != cards[r]:
                    # move left pointer to land on duplicate card
                    enc.remove(cards[l])
                    l+=1
                min_ans = min(min_ans, r-l+1)
                l+=1
        if min_ans==float('inf'):
            return -1
        return min_ans
```