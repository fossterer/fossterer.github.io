# Advent of code 2020 - Day 7 (Part 2)

[Day - 7](https://twitter.com/SVRSN_Shashank/status/1337977124687400961) Part 2

I: I am unable to crack this

(After a bath and some scribbling on paper)

I: I solved it

My friend: Cool! What was the mistake?

I: Not writing on paper

## Attempt 1: Time Complexity O(???)

A total reversal of [Part 1](advent-of-code-2020-day-7-part-1.md), we now construct the map in given order `<outer (each), inner (set)>` and then use recursion to traverse all possible sub-trees

Here's my `__main__.py` unchanged except for comments. Wait! haven't you been reading my comments inside programs all this time?:

````python
""" Program to determine how many bags go inside a required "shiny gold bag"

    At every line, we don't know if the contents "inside" it can potentially in the future "some to include" more bags in them.
    This can be at 3d level, 4th level and so on too. So we cannot throw away any "contains inside" information.

    Though this likes a "graph", we are seing it simply as "dictionary" that can hold "sets"

    Once the entire ruleset is processed, we process to find out if any "content" of a set can become a key and then if it "terminally contains" our required bag.
    When found, we "multiply" the numbers associated with such sets
"""
from processors import process_line
from containers import fill_container, get_container_count, print_container

with open('input-7.txt', 'r') as lines:
    for line in lines:
        outer, inner = process_line(line)
        fill_container(outer, inner)

    # print_container()
print(get_container_count('shiny gold'))
````

My `processors.py` module with obvious deviations from that in [Part 1]():

````python
""" Module to process rule strings and produce bag mappings
"""
import re

"""
    Group 1: Colour of single "outer" bag
    Group 2: The "inner" bag(s) string entirely
"""
line_parts_regex = r'(.*) bags contain (.*bags?)'

"""
    Group 1: Count and colour of each "inner" bag(s)
"""
inner_bag_regex = r'(\d+) (.*) bags?'


def process_line(line):
    """    Processes the rule string to return "outer" bag and set of "inner bags"
    """

    line_parts = (re.match(line_parts_regex, line)).groups()
    # print(line_parts)

    """
    'no other bags' returns an empty set
    Otherwise, extract the 'groups' that match both the count and colour from "each inner" bag

    Populate the 'bag_mapping' dictionary in the form <str, List>
    """

    outer = line_parts[0]
    inner = set(
        (re.match(inner_bag_regex, s)).groups() for s in line_parts[1].split(', ') if not s == 'no other bags'
    )

    return outer, inner
````

The neatly updated (with `print_()` method) `containers.py`:

````python
""" Module to maintain datastructures that hold bag mappings
"""

"""
    We choose 'dict' which later comes to holds 'set' as value
"""
bag_mapping = {}


def fill_container(outer, inner):
    bag_mapping[outer] = inner


def print_container():
    [print(key + ': ' + str(value)) for key, value in bag_mapping.items()]


def get_container_count(item_of_interest):
    """    Returns a set that goes inside 'item of interest' in this container
    """

    total = 0
    set_of_interest = bag_mapping[item_of_interest]
    # print(set_of_interest)

    if len(set_of_interest) > 0:
        for count, key in set_of_interest:
            total += int(count) * (get_container_count(key) + 1)
            # print(total)
        return total
    else:
        return 0
````

Figuring out recursion was the toughest part which I cracked only after drawing a tree and writing how the equations would look at each level, starting from the bottom and traversing across a level from left to right
Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-2-attempt-1/
1664

real    0m0.061s
user    0m0.051s
sys     0m0.009s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-2-attempt-1/
1664

real    0m0.060s
user    0m0.056s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-2-attempt-1/
1664

real    0m0.057s
user    0m0.045s
sys     0m0.012s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ 
````

How do I calculate time complexity for a recursive function? Is it any different than usual loop counting? Please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1337977124687400961) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. While recursing over trees, likely you might want to return only after finishing a given level completely
   1. When in the same level, aggregate into a variable and only at the end of that level, return that variable
