# Advent of code 2020 - Day 2 (Part 2)

[Day - 2 of the series](https://twitter.com/SVRSN_Shashank/status/1334606106325291008) - The challenge screams more and more like a regex problem but I still choose to start with the simplest approach as *Attempt-1*

## Attempt 1: Time Complexity O(n)

We process the input string character-by-character:

````python
""" After a line is read and processed, we do not need it anymore. So no datastructures except for a counter to keep track of no. of correct passwords.
    - Set "valid_password_counter" to 0
    - Split the line by spaces to fill 3 variables:
    -- (1) pos1-pos2 pair -- includes '-' sign
    -- (2) character to be searched
    -- (3) password
    -
    - Split the pos1-pos2 pair by '-' to fill 2 variables:
    -- (1) pos_1
    -- (2) pos_2
    - Then,
    -- if password[pos_1 -1] XOR password[pos_2-1] returns True, increment "valid_password_counter" by 1
    - At the end of loop, print "valid_password_counter"
"""
def execute():
    valid_password_counter = 0

    for line in lines:    
        ps1_pos2_pair, character, password = line.split()
        pos_1, pos_2 = map(int, ps1_pos2_pair.split('-'))
        character = character[0]
    
        """print(min_count, max_count, character, password)"""

        if (password[pos_1 - 1] == character)^(password[pos_2 - 1] == character):
            valid_password_counter += 1
    
    print(valid_password_counter)

lines = open('input-2.txt', 'r')

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-1.py 
313

real    0m0.039s
user    0m0.031s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-1.py 
313

real    0m0.039s
user    0m0.035s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-1.py 
313

real    0m0.043s
user    0m0.042s
sys     0m0.001s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ 
````

## Attempt 2: Time Complexity O(n)

I give another try using *regex* here:

````python
import re
""" After a line is read and processed, we do not need it anymore. So no datastructures except for a counter to keep track of no. of correct passwords.
    - Set "valid_password_counter" to 0
    - Split the line by spaces to fill 3 variables:
    -- (1) pos1-pos2 pair -- includes '-' sign
    -- (2) character to be searched
    -- (3) password
    -
    - Split the pos1-pos2 pair by '-' to fill 2 variables:
    -- (1) pos_1
    -- (2) pos_2
    - Then,
    -- if password[pos_1 -1] XOR password[pos_2-1] returns True, increment "valid_password_counter" by 1
    - At the end of loop, print "valid_password_counter"
"""
def execute():
    valid_password_counter = 0

    for line in lines:    
        ps1_pos2_pair, character, password = line.split()
        pos_1, pos_2 = map(int, ps1_pos2_pair.split('-'))
        character = character[0]
    
        """ print(pos_1, pos_2, character, password) """

        regex_1 = '.{' + str(pos_1 - 1) + '}' + character + '.*'
        regex_2 = '.{' + str(pos_2 - 1) + '}' + character + '.*'

        pattern_1 = re.compile(regex_1)
        pattern_2 = re.compile(regex_2)
        """ print(pattern_1)
        print(pattern_2) """
        
        if (pattern_1.match(password) is not None)^(pattern_2.match(password) is not None):
            valid_password_counter += 1
    
    print(valid_password_counter)

lines = open('input-2.txt', 'r')

execute()
````

Aha! That made me give a good read of Python's regex module and [its gotchas](https://docs.python.org/3/howto/regex.html#the-backslash-plague). Now I wonder if I gained anything with this eyesore piece of code?! Aaargh!

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-2.py 
313

real    0m0.067s
user    0m0.059s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-2.py 
313

real    0m0.069s
user    0m0.065s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ time python3 part-2-attempt-2.py 
313

real    0m0.066s
user    0m0.049s
sys     0m0.017s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-2$ 
````

That's a loss indeed.

Do you have a better approach? [Tweet as a reply](https://twitter.com/SVRSN_Shashank/status/1334606106325291008) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. Python's `r'` (raw strings) save us from backslash hell in regexes
