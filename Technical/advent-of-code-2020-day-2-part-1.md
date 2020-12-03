# Advent of code 2020 - Day 2 (Part 1)

[Day - 2 of the series](https://twitter.com/SVRSN_Shashank/status/1334591121461309441) - The challenge looks like a candidate for regex but I start with the simplest approach as *Attempt-1*

## Attempt 1: Time Complexity O(n*m)

We process the input string character-by-character:

````python
""" After a line is read and processed, we do not need it anymore. So no datastructures except for a counter to keep track of no. of correct passwords.
    - Set "valid_password_counter" to 0
    - Split the line by spaces to fill 3 variables:
    -- (1) min-max pair -- includes '-' sign
    -- (2) character to be searched
    -- (3) password
    -
    - Split the min-max pair by '-' to fill 2 variables:
    -- (1) min_count
    -- (2) max_count
    - Then set a flag "valid_password" to false and "character_counter" to 0 and enter the loop to process each letter in password
    - When the character being searched is seen, increment the "character_counter"
    -- if character_counter is greater than or equal to "min_count" and less than or equal to "max_count", flip "valid_password" to true
    - At the end of loop, if "valid_password" is true, increment "valid_password_counter" by 1
    - At the end of loop, print "valid_password_counter"
"""
def execute():
    valid_password_counter = 0

    for line in lines:    
        min_max_pair, character, password = line.split()
        min_count, max_count = map(int, min_max_pair.split('-'))
        character = character[0]
    
        """print(min_count, max_count, character, password)"""

        valid_password = False
        character_counter = 0
        for letter in password:
            if letter == character:
                character_counter += 1
                if character_counter >= min_count and character_counter <= max_count:
                    valid_password = True
                else:
                    valid_password = False
        
        if valid_password:
            valid_password_counter += 1
    
    print(valid_password_counter)

lines = open('input-2.txt', 'r')

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-1-attempt-1.py 
500

real    0m0.021s
user    0m0.021s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-1-attempt-1.py 
500

real    0m0.043s
user    0m0.034s
sys     0m0.009s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-1-attempt-1.py 
500

real    0m0.042s
user    0m0.034s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ 
````

## Attempt 2: Time Complexity O(n*m)

As I came back to give this another try using *regex* here, I realize this is not a *pattern* problem. We don't where this character occurs and the minimum and maximum occurrences are not sequential to say something as `q{1-3}`. That *q* indeed can occur at minimum `1` time and at maximum `3` times but those don't have to be in sequence.

Any attempt to form such a *pattern* would have to make use of possibly multiple *backreferences* for each position. This is not a candidate for *regex* as initially thought but the [Part 2](advent-of-code-2020-day-2-part-2.md) is indeed. Look at the [Attempt 2](advent-of-code-2020-day-2-part-2.md#attempt-2-time-complexity-on) there!

## Lessons Learned

1. If every case is independent of previous case, you do not need a specialized datastructure to store
   1. A corollary is that if future computations would carry out the same calculation we are doing now, store it - increasing space used to compensate for time reduced
2. Use Python's `timeit` module or *nix's `time` command to instrument the running time of programs
