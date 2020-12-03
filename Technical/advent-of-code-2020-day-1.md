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

If we find the incoming value as an existing key, simply multiple the value and (2020 - value) and break.
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
