## Queue
Queue is contrast as Stack. In Queue data access severed as FIFO(First in First out).
```inqueue()``` means put the data in queue.
```dequeue()``` means delete the data in queue. It's same as pop() in Stack.

In Queue, we should know about first_data and last_data. So we call first_data = front_index and last_data = append.

Example of Queue
```
class Queue:
    def __init__(self):
        self.items = []
        self.front_index = 0
    
    def inqueue(self, val):
        self.items.append(val)
    
    def dequeue(self):
        if self.front_index == len(self.items):
            print("Queue is empty)
        else:
            x = self.items[front_index]
            self.front_index += 1
            return x
```
This is code of basic structure about Queue. We make class of Queue then, we assume about inqueue and dequeue. 