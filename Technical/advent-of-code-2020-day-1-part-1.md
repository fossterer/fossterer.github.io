# Advent of code 2020 - Day 1

[Day - 1 of the series](https://twitter.com/SVRSN_Shashank/status/1334265716921528323) - The challenge is a *2-sum problem* that goes like this:

You have an *array*. Find *2 values* such that their sum equals a *target value*

## Attempt 1: Time Complexity O(n<sup>2</sup>)

Having solved it earlier I quickly jumped to solve it as below

````python
lines = file('input-1.txt', 'r')

arr = []
index = 0
for line in lines:
    
    val = int(line)
    
    for entry in arr:
        if val + entry == 2020:
            print("Result: " + str(val * entry))
    
    arr.append(val)
````

However, my friend [Ram](https://www.linkedin.com/in/sairamnellutla) challenged that it could be solved with O(n) complexity itself. As he hints *HashMaps* so I try

## Attempt 2: Time Complexity O(n)

By leveraging HashMaps, we further adhere to our core principles - *Never process anything that was already seen once*.

````python
from collections import defaultdict

"""
We make up our 'arr' a HashMap - i.e a dictionary in Python.
Since a lookup in Hashmap is in constant time, we can avoid one loop.
    Keys would be the values we read in.
    We don't care for values. Let them default to zero

If we find the difference of (2020 and incoming value) as an existing key, simply multiply the value and (2020 - value) and break.
If we don't find the incoming value already as a key, we store it as key while value is set to the default (0).
"""

lines = file('input-1.txt', 'r')

arr = defaultdict(int)

for line in lines:
    
    val = int(line)

    if 2020-val in arr:
        print("Result: " + str(val * (2020-val)))
        break
    else:
        arr[val] = 0
````

## Attempt 3: Reduced Space Usage; Time Complexity O(n)

If always all our values were 'zero', why are even considering such a datastructure?

I started with an array *for its direct facilitation of storing multiple values* and then advanced to a datastructure that *promises constant time retrieval as opposed to full iteration like in an array* that turned out to be a *HashMap (dictionary)*

The same promise can be provided by sets too. See [here](https://wiki.python.org/moin/TimeComplexity) that the average time complexity of *search* is still O(1)

````python
"""
We make up our 'arr' a Set - since we didn't have to fully utlilize any "key-value" facility in the last attempt.
All we are interested in is only one entity "key" and we still avoid the one inner loop from our first attempt.
    Entries would be the values we read in.

If we find the difference of (2020 and incoming value) as an existing set constituent, simply multiply the value and (2020 - value) and break.
If we don't find the incoming value already as a key, we store it in set.
"""

lines = file('input-1.txt', 'r')

arr = set()

for line in lines:
    
    val = int(line)

    if 2020-val in arr:
        print("Result: " + str(val * (2020-val)))
        break
    else:
        arr.add(val)
````

Thanks again [Ram](http://portfolio.ramnellutla.com)!

## Lessons Learned

1. Never process anything that was already seen once
   1. To support that, always look for something *better than arrays*
   2. Do you think you will calculate something again after some time? Calculate now and store it such that *search* takes *constant time*
2. HashMaps are not always the answer. If you have only one piece of information to store, you just need a *Set*
   1. However, look at [next post](advent-of-code-day-1-part-2) where we ultimately (not at the start) have to use HashMap
