## List and Array
list and array are most basic sequential data structure, these are basic but important.
*We can access specific element by index, then assume that read and write about specific element can serve in 1 unit time in virtual enviorment. 
list in Python has dynamic array that change capacity automatically. However, array in C we should allocate new array that has much bigger capacity.

About, method of list that append, pop have O(1) time complexity and insert, remove, index, count have O(n)

## Sequential data structures

Stack, Queue, Dequeue
restricted access(push, pop) only allowed.
Stack : LIFO(Last In First Out)
Queue : FIFO(First In First Out)
Dequeue : Can do operation of both Stack and Queue.

Linked list : *Different with list in Python.
Values are sotred in non-contiguous memory space. 
Each value also contains address of next value. 
It's impossible that access value by index. If we want access specific value we should find step by step from previous value.

Stack has push, pop, top, len methods. 
Each method has O(1) time complexity.
'push' is put value in Stack squentially.
'pop' is same as 'pop' in Python list, this can delete value.
'top' is find the last value in Stack, however doesn't delete value.
'len' is offer the number of values in Stack.
