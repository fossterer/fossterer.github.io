# Advent of code 2020 - Day 4 (Part 1)

Day - 4 of the series took me very long as I did not fully understanding the way to detect empty line in Python

## Attempt 1: Time Complexity O(n)

I have no words (as of this writing) as to why my initial solution didn't account for the final passport case resulting in 1 less than the expected answer. My repeated submission eventually took me to the system saying something on the lines of "This is your 7th incorrect submission. You will have to wait 10 minutes before you submit another answer".

*Note:* You have to put 2 blank lines (Not 1) at the end of input file for this code to return correct result

````python
import re
""" Read every line until you encounter a blank line

    As soon as a line is read, split to detemrine keys.
    Using them, set the flag against keys such as (byr:|iyr:| etc.) in a map to 'True'

    Once a blank line is encountered, count if flags on all fields excluding the optional 'cid' sum to 7.
    If the result is true, increment valid_passport_counter by 1
"""
def execute():
    valid_passport_counter = 0

    fields = {
                'byr': False,
                'iyr': False,
                'eyr': False,
                'hgt': False,
                'hcl': False,
                'ecl': False,
                'pid': False,
                'cid': False
            }

    field_keys = fields.keys()
    valid_field_count = 0
    
    with open('input-4.txt', 'r') as lines:        
        for line in lines:
            if not line.strip():

                valid_field_count = 0
                for key in field_keys:
                    if key != 'cid' and fields[key] == True:
                        # print(key)
                        valid_field_count += 1

                if valid_field_count >= 7:
                    valid_passport_counter += 1
                    # print('Accepted')
                
                for key in field_keys:
                    fields[key] = False

            else:
                for pair in line.split():
                    key, val = pair.split(':')
                    if key in field_keys:
                        fields[key] = True
   
    print(valid_passport_counter)

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-1-attempt-1.py 
246

real    0m0.028s
user    0m0.020s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-1-attempt-1.py 
246

real    0m0.026s
user    0m0.026s
sys     0m0.001s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-1-attempt-1.py 
246

real    0m0.045s
user    0m0.040s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ 
````

Do you see why I had to put an additional blank line in input file at the end for this code to work? If so, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1334606106325291008) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) or [Respond on Reddit](https://www.reddit.com/r/adventofcode/comments/k85av2/day_04_part_1_what_am_i_doing_wrong/) referencing the title.

## Lessons Learned

1. Supposedly, the piece `if not line.strip():` is not checking for *just a blank line* but is indeed checking for *a blank line + a carriag return*? I still am not sure
   1. So always check if terminating cases such as checking for blank line are functioning properly in your programs