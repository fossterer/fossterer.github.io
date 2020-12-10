# Advent of code 2020 - Day 5 (Part 1)

[Day - 5](https://twitter.com/SVRSN_Shashank/status/1336852189671723009) of the series surprised me big as I got the program to execute with absolutely no errors right on the first attempt!

## Attempt 1: Time Complexity O(n)

Continuing the tradition of modularized programs, here's my `__main__.py` used to solve this challenge:

````python
""" Program to determine the highest seat ID from given boarding passes

    Read every line and determine seat ID

    If current seat ID is higher than the existing "highest seat ID", save the current seat ID into "highest seat ID"
"""
from processors import seat_id


highest_seat_id = -1

with open('input-5.txt', 'r') as lines:
    for line in lines:
        highest_seat_id = max(highest_seat_id, seat_id(line))

print(highest_seat_id)
````

Here're the `processors.py` module used:

````python
""" Module to process boarding pass strings specified in "binary space partitioning" notation

    For each character, if 'B', multiply 1 by 2^n (from n=6 to n=0) and sum together
    When n reaches 0, multiply the sum by 8

    Then, for each character, if 'R', multiply 1 by 2^n (from n=2 to n=0) and sum together

    Return the final sum as seat_id
"""


def seat_id(boarding_pass_string):
    """    Processes the boarding pass string from left to right

    Obtains part identifiers.
    Multiplies and sums them to get full seat_id and return
    """

    return partial_id(boarding_pass_string, 0, 7)*8 + partial_id(boarding_pass_string, 7, 10)


def partial_id(boarding_pass_string, startIndex, endIndex):
    """    Processes the boarding pass string read by breaking as per provided indices and performing multiplications as if in a bitmap

    Breaks the string into row and column strings to determine part identifiers.
    """

    string = boarding_pass_string[startIndex:endIndex]
    exponent = endIndex - startIndex - 1

    part_id = 0
    for char in string:
        if char == 'B' or char == 'R':
            part_id += 2**exponent
        exponent -= 1

    return part_id
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-1-attempt-1
855

real    0m0.033s
user    0m0.029s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-1-attempt-1
855

real    0m0.029s
user    0m0.029s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-1-attempt-1
855

real    0m0.031s
user    0m0.027s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ 
````

If you are curious about the choices made here or have suggestions to improve this even further, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1336852189671723009) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

Not one but many!

1. When you see *binary* in the problem description, not necessarily it is always a Binary Search Tree problem. Sometimes it is just an encoding problem that can be approached with bitmaps etc.
2. While breaking up your Python logic into multiple modules, name the *driver program* `__main__.py`. Then invoking the program is just calling the containing folder with *Python executable*
3. Seeing *pydoc* in modules is achieved as `pydoc3 folder_name.module_name` *i.e* separated by a dot (.)
