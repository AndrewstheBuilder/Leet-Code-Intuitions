1. [Pow(x,n)](https://leetcode.com/problems/powx-n/description/?envType=company&envId=facebook&favoriteSlug=facebook-thirty-days)
- One of the hardest solutions to get but once I ran through a possible problem in the solution I got it
- solution
```python
def myPow(self, x: float, n: int) -> float:
        def pow(n, x):
            # base case
            if n == 0:
                return 1
            if(n<1):
                # all the negative exponent does is return the result in fractional form
                return 1.0/(pow(-1*n, x))
            
            if(n%2==1):
                return x * pow((n-1)//2, x*x)
            else:
                return pow(n//2, x*x)
        return pow(n,x)
```
- Intuitions:
- we are saying if n == 0 return 1 that parts straight forward.
- if n < 1 return the final answer in a 1/ fraction. Evaluate the rest of the problem as if the exponent is positive that takes care the negative exponent.
- for odd numbers we are leaving out one x and then halving the search space. x*x that is passed as a argument is only returned in the odd case. if n==1 it return x*x.
    - we are halving the search space because doing x*x makes the exponent on x go from 2,4,8,16. Its exponentiating all the way until x==0.
       - Flooring the division also has the effect of moving n towards 0
    - if its even we do x*x and halve n.
- if the initial input is even it will eventually hit n = 1 where we evaluate x^n * (0//2, x*x).
- if the initial input is odd it will start out with x * pow() and hit one where it returns x*(n-1)
    - so the last return will be 1 * x * x^(n-1).
    - the n-1 is not explicitedly in the code. Its a result of doing x*x over and over again so n will be a exponentiated 2. Then since its odd there is one more x left over by design!
- Time Complexity: O(logn) halving the search space. (n/2^k) = 1 in the search space. that is either exactly 1 in the search space or close to it. take n = 2^k log of both side log(n) = log(2^k) --> log(n) = k
