# Day01

## Basic concept of words
Algorithm : The result of data that finite calculated by memory which read, write, search, delete and insert. 

Examples of data_structures : variable, array, list ... ect

## Time complexity
We simulate virtual environment which virtual machine, pesudo language, pseudo code. To assume virtual environment, we don't care about difference of performance among machines.

### Virtual Machine
Our machine has changed Turing machine to Von Neumanm archtectures.
In that archtectures consist by RAM(Random Access Machine) = CPU + Memory + basic calculation. 
Basic calculation is calculation carried out in 1 unit time, which means assignment, substitution, copy, arithmetic(except likes remainder operation), compare operation, logical operation, bit operation.

### Pseudo(Virtual) Languages
This languages for simulation not actual action, so we don't care about complex grammers. These one just able to express that basic calculation, repeat and function(def)

If someone wirte like this:
```
algorithm arrayMax(A, n):
    input : Array A has n integers
    output : Max value in A
    for i = 1 to n - 1 do
        if currentMax < A[i] : 
            currentMax = A[i]
    return currentMax
```
It doesn't work in real computer, however we can understand what this means.
For example, we can write code in Python that like this ``` for i in range(1, n): ```. 

