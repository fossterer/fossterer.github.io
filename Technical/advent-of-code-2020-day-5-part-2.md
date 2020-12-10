# Advent of code 2020 - Day 5 (Part 2)

[Day - 5](https://twitter.com/SVRSN_Shashank/status/1336852189671723009) of the series - part 2. Interestingly or otherwise, I only had to modify the driver program `__main__.py`.
The `processors.py` remians the same.

## Attempt 1: Time Complexity O(n)

Detailed description is directly in the comments. Here's the updated `__main__.py`:

````python
""" Program to determine the missing seat ID from given boarding passes

    We are given that certain no. of seats are by defualt missing from the front and the back.
    So we cannot assume the seat IDs start from 0 and end at 2^10 -1 (10 since our bitmap has 10 bits)

    We further see that the equation for seat ID (row_id*8 + column_id) tells us that it is just 8 seats in row.
    As such the seats are indeed in sequence.

    This is same as the interview problem I faced for current job.
    We simply sum every number (seat ID here) as we see it.
    We know that sum of n numbers in series is (n/2)*(a + l)

    By obtaining the difference, we find the missing number

    Read every line and determine seat ID

    If current seat ID is higher than the existing "highest seat ID", save the current seat ID into "highest seat ID".
    Similarly, if it is lower than the existing "lowest seat ID", save the current seat ID into "lowest seat ID".
"""
from processors import seat_id


highest_seat_id = -1
lowest_seat_id = 99999999

seat_id_sum_actual = 0

line_count = 0

with open('input-5.txt', 'r') as lines:
    for line_id, line in enumerate(lines):
        current_seat_id = seat_id(line)
        #print(current_seat_id)

        highest_seat_id = max(highest_seat_id, current_seat_id)
        lowest_seat_id = min(lowest_seat_id, current_seat_id)

        seat_id_sum_actual += current_seat_id

        line_count = line_id + 1

seat_id_sum_expected = ((line_count + 1)/2)*(lowest_seat_id + highest_seat_id)

print(seat_id_sum_expected - seat_id_sum_actual)
````

Here're the `processors.py` module - unchanged:

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
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-2-attempt-1/
552.0

real    0m0.029s
user    0m0.025s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-2-attempt-1/
552.0

real    0m0.028s
user    0m0.022s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ time python3 part-2-attempt-1/
552.0

real    0m0.030s
user    0m0.026s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-5$ 
````

If you are curious about the choices made here or have suggestions to improve this even further, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1336852189671723009) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. Starting from at least Python 3, the types such as `int` are unbounded. As such, there's no meaningful `sys.maxint` and `sys.minint`
   1. `sys.maxsize` used by some gives the current machine *word size*
