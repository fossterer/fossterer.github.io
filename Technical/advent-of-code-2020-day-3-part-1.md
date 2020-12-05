# Advent of code 2020 - Day 3 (Part 1)

[Day - 3 of the series](https://twitter.com/SVRSN_Shashank/status/1335109439360278528) - At first I thought of approaching this as just "3 times the line number" but turned out the horizontal line provided in input doesn't accommodate the position calculated after certain no. of lines (10 in this case)

## Attempt 1: Time Complexity O(n)

I mistakenly placed `line_length = len(next(iter(lines)))` at first where I now hardcoded 31. That had the effect of advancing my lines (file pointer) messing up the count

````python
""" Let's say that our toboggan starts at (0,0).
    The co-ordinates we can visit are (1,3), (2, 6), (3, 9) and so on.
    
    i.e on  1st line, check if position 3 is #, on 2nd line check if position 6 is # and so on.

    As we can repeat each line horizontally as it is even if not provided in input, we simply divide with total length of line to arrive at the position.
    The remainder gives the position inside given input
"""
def execute():
    tree_counter = 0

    with open('input-3.txt', 'r') as lines:
        line_length = 31
        for line_num, line in enumerate(lines):
            position_of_interest = (line_num*3)%line_length
            if line[position_of_interest] == '#':
                tree_counter += 1
    
    print(tree_counter)

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-1-attempt-1.py 
209

real    0m0.022s
user    0m0.022s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-1-attempt-1.py 
209

real    0m0.020s
user    0m0.012s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-1-attempt-1.py 
209

real    0m0.020s
user    0m0.016s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ 
````

I don't see any better way of solving this. Do you? If so, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1334606106325291008) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. Prefer Python's `with open() as pointer` approach where closing of file is automatically taken care of
2. Once a file pointer is created, not excluding in the current `with` approach, applying next() over its iterator affects it permanently causing unexpected issues in latter parts of logic
