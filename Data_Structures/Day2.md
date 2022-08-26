## Time complexity
Basic methods : Adds the number of basic operations about all inputs and get average.
Basic method is accurate, however it's impossible that calculate all inputs.
So, we usually use another one that calculate about 'worst case input'.
Worst case input means input that takes the longest to calculate in a specific algorithm. 

```
algorithm arrayMax(A, n):
    input : Array A has n integers
    output : Max value in A
    for i = 1 to n - 1 do
        if currentMax < A[i] :   #  --> compare 1 unit time
            currentMax = A[i]  # --> allocate 1 unit time
    return currentMax
```
in this code. 2 unit time in 1 for loop and number of repetitions about 1 to n-1, so (n-1)*2 = 2n-2 is time complexity of arrayMax(A, n)

```
def number_of_bits(n):
    count = 0  # --> allocate 1 unit time
    while n > 1:
        n = n // 2  # --> quotient operation 1 unit time + allocate 1 unit time 
        count += 1  # --> plus 1 unit time + allocate 1 unit time
    return count
```
In this case, we assume quotient operation has same unit time with divide.
If n = 8, in first loop n = 4, count = 1.
Second, n = 2, count = 2.
Third, n = 1, count = 3.
Fourth, stop loop and return count.
So, n will divide until n going to 1 that n / 2^count = 1. 
Then, we can get time complexity = 4 * (lb n) + 1.

## Big O 
There are three time complexity that T1 = 2n - 1, T2 = 4n + 1, T3 = n^2 + 2n + 1.
T1 is almost twice as fast as T2. However, if n going to infinite number there are no difference about rate of increase between T1 and T2. So, we can say simply that T1 and T2 have the same time complexity. But, T3 is different, because T3 has square number about n, so rate of increase is higher in twice than T1, T2.
In Big O, we express T1, T2 = O(n), T3 = O(n^2) that regardless of coefficients and constants, only the highest order term remains. 
If log contained in complexity, we can express O(log n). It's same reason with square number, log is much less than 1st term about rate of increase.
If only constant term, we write O(1), because n^0 = 1.