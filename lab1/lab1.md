# CSCI 3212 Lab 1

## Fibonacci numbers

Source: https://en.wikipedia.org/wiki/Fibonacci_sequence  

The Fibonacci sequence is a sequence of numbers where:  
1. The first and second numbers are both ``1``, that is, ``fibonacci(1) = fibonacci(2) = 1``
2. The numbers that follow are the sum of the previous TWO numbers, so  
``fibonacci(3) = fibonacci(2) + fibonacci(1) = 1 + 1 = 2``  
``fibonacci(4) = fibonacci(3) + fibonacci(2) = 2 + 1 = 3``.

```
TODO: Answer the following questions:
fibonacci(5) = 5
fibonacci(6) = 8
fibonacci(7) = 13
fibonacci(8) = 21
fibonacci(9) = 34
```

## Basic implementation

Let's take a look at an implementation:
```python
def fibonacci(n):
    if n <= 0:
        return 0
    if n == 1:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)
```
You can also find this in ``algorithms-labs-gw26/lab1/fibonacci.py``, and run it: 
```bash
cd algorithms-labs-gw26/lab1
python fibonacci.py
```
The code version also tells you how much time does it take to complete each calculation.
```
TODO:
1. Explain what the code above is doing.
A: It follows the definition directly. The two "if" lines are the base cases (fibonacci(0) = 0,
   fibonacci(1) = 1). For any larger n it calls itself on n-1 and n-2 and adds the two results.
2. What happens if we remove the "if ... return ..." and only keep the last line?
A: There is no base case, so the recursion never stops: n keeps going down into negative numbers
   until Python hits its recursion limit and raises RecursionError.
3. What is fibonacci(20)? how much time did it take to calculate that?
A: fibonacci(20) = 6765, about 1.4e-03 seconds.
4. What is fibonacci(30)? how much time did it take to calculate that?
A: fibonacci(30) = 832040, about 1.6e-01 seconds.
5. How much time did it take you to calculate fibonacci(40)? (this might take a while...)
A: fibonacci(40) = 102334155, about 20 seconds. Each +10 in n made it roughly 120x slower.
```

## How many function calls?

Modify ``fibonacci_counting.py`` so that it does the same calculation as ``fibonacci.py``, but it also counts how many times the function ``fibonacci(n)`` had to be called. Then answer the following:
```
TODO:
1. How many function calls does fibonacci(1) take?
A: 1
2. How many function calls does fibonacci(5) take?
A: 15
3. How many function calls does fibonacci(10) take?
A: 177
4. Why is it so slow? Where does the complexity come from?
A: Every call (except the base cases) makes two more calls, and the two branches overlap.
   fibonacci(n-1) recomputes fibonacci(n-2), which is also computed separately, and so on
   all the way down, so the same small values get recomputed an exponential number of times.
   The number of calls follows C(n) = C(n-1) + C(n-2) + 1, which works out to
   C(n) = 2*fibonacci(n+1) - 1 (e.g. 2*89 - 1 = 177 for n = 10).
5. Is this O(n)? is this O(2^n)? Why?
A: It is not O(n): the call count grows like 1.618^n, which eventually beats any c*n.
   It is O(2^n): C(n) <= 2*C(n-1) + 1, so the call tree has at most 2^n nodes
   (each level at most doubles). The tight bound is Theta(phi^n) with phi ~ 1.618.
6. Is this Ω(n)? Why?
A: Yes. Just the chain fibonacci(n) -> fibonacci(n-1) -> ... -> fibonacci(1) is already n calls,
   so it does at least n work. (It is actually much more, Omega(phi^n), which is also Omega(n).)
```

## Memoization Optimization

Take a look at ``fibonacci_counting.py``, where memoization is used.
```
TODO:
1. How is this one different from the previous one?
A: It stores every result in the "cache" dictionary the first time it is computed. Later calls
   for the same n return the stored value instead of recursing again.
2. How much time does it take to calculate fibonacci(30)?
A: About 2e-05 seconds (vs. about 0.16 seconds without memoization).
3. Why is it often faster?
A: Each fibonacci(k) is only actually computed once. After fibonacci(n-1) finishes, every value
   below it is already cached, so the fibonacci(n-2) call returns immediately. The exponential
   tree of repeated calls collapses into a single chain.
4. Also modify this file to count: how many times the function had to be called for fibonacci(30)?
A: 59 calls (2n - 1): one call for each k from 30 down to 1, plus one cached lookup call for
   each k from 28 down to 0.
5. Is this O(n)? is this O(2^n)? Why?
A: Both. It makes 2n - 1 calls and each does constant work (a dictionary lookup/insert and an
   addition), so it is O(n). Anything that is O(n) is also O(2^n), but O(n) is the useful bound.
6. Is this Ω(n)? is this Ω(2^n)? Why?
A: It is Omega(n): it still has to recurse down through every value from n to 1 once.
   It is not Omega(2^n): the total work is about 2n, which is far below 2^n for large n.
```

## Extension: Staircase Problem

Implement ``fibonacci_threeway.py``, where:
1. The first, second, and third numbers are ``1``.
2. The numbers afterwards are the sum of the previous **THREE** numbers, instead of two.
3. Your implementation should be optimized, taking less than 1 second to calculate ``fibonacci_threeway(50)``.

## Optional, challenge problems
1. Instead of recursion, implement ``fibonacci(n)`` using iteration instead.
2. ``fibonacci_memoized.py`` fails if you give it a very large input number such as one million - why? Try fixing it.
3. There is an even faster way to calculate fibonacci numbers, in (almost) O(1) time. Read Wikipedia and try to implement it, or if you like a big challenge, implement it without looking it up.