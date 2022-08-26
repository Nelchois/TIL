## Time complexity
Basic methods : Adds the number of basic operations about all inputs and get average.
Basic methods is accurate, however it's impossible that calculate all inputs.
So, we usually use another one that calculate about 'worst case input'.
Worst case input means input that takes the longest to calculate in specific algorithm. 

```
algorithm arrayMax(A, n):
    input : Array A has n integers
    output : Max value in A
    for i = 1 to n - 1 do
        if currentMax < A[i] :   --> compare 1 unit time
            currentMax = A[i]   --> allocate 1 unit time
    return currentMax
```
in this code. 2 unit time in 1 for loop and number of repetitions about 1 to n-1, so (n-1)*2 = 2n-2 is time complexity of arrayMax(A, n)