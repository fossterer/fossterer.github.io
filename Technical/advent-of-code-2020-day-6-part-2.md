# Advent of code 2020 - Day 6 (Part 2)

[Day - 6](https://twitter.com/SVRSN_Shashank/status/1336881096018026498) Part 2. Now we are to consider only those questions to which everyone answered "yes".

## Attempt 1: Time Complexity O(n)

Here's my `__main__.py` used to solve this challenge. Observe that there is no change at all in this *driver* program:

````python
""" Program to determine the sum of unique questions across groups of persons

    Read every line, make it a set and union to existing group's set

    If a blank line is encountered, increment the sum_of_counts by the length of this set.
    Clear the set and proceed to next

    As the final set wouldn't have gotten processed, due to absence of blank line, process it outside the loop
"""
from processors import count, fill, reset


sum_of_counts = 0

with open('input-6.txt', 'r') as lines:
    for line in lines:
        if not line.split():
            sum_of_counts += count()
            reset()
        else:
            fill(line)

    sum_of_counts += count()
    reset()

print(sum_of_counts)
````

Here're the `processors.py` module used:

````python
""" Module to maintain counts of questions per person and reset them
"""
from containers import group_set
from os import linesep


def fill(person_string):
    """    Processes the person string to construct a new and union with an existing set
    """

    global group_set
    group_set = group_set.intersection(
        set(person_string)) if not group_set == None else set(person_string)
    group_set.discard(linesep)
    # print(group_set)


def count():
    """    Returns the length of set filled for this group so far
    """

    global group_set
    return len(group_set)


def reset():
    """    Resets the set filled for a group
    """

    global group_set
    group_set = None
````

Observe that `containers.py` module changed only slightly from [previous part](advent-of-code-2020-day-6-part-1.md)

````python
""" Module to maintain datastructures that hold questions per group

    We choose 'set' since we only need unique question count
"""


group_set = None
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-6$ time python3 part-2-attempt-1
3178

real    0m0.029s
user    0m0.029s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-6$ time python3 part-2-attempt-1
3178

real    0m0.033s
user    0m0.025s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-6$ time python3 part-2-attempt-1
3178

real    0m0.027s
user    0m0.023s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-6$ 
````

Do you have a better solution? Please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1336881096018026498) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. In Python *null* is represented as `None`
2. In Python, empty *set()* is not same as *None*
