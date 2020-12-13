# Advent of code 2020 - Day 7 (Part 1)

[Day - 7](https://twitter.com/SVRSN_Shashank/status/1337977124687400961) Part 1 - where the first solution that comes to mind is the one with regex!

Possibly since this looks immediately like a *pattern matching problem* - a *natural language processing problem* is it?

## Attempt 1: Time Complexity O(n<sup>2</sup>)

Despite what it looks like, at first sight, is a construction of 'a dictionary of the form `<outer (each), inner (set)>`, we need to be able to search what "other inner bags" are contained by "outer bags".

So we need to flip the dictionary construction as <`inner (each), outer (set)>`

Here's my `__main__.py` used to solve this challenge:

````python
""" Program to determine how many bags can contain a required "shiny gold bag"

    At every line, we don't know if the contents "inside" it can potentially in the future "can contain" our required bag.
    This can be at 3d level, 4th level and so on too. So we cannot throw away any "contains inside" information.

    Though this likes a "graph", we are seing it simply as "dictionary" that can hold "sets"

    Once the entire ruleset is processed, we process to find out if any "content" of a set can become a key and then if it "terminally contains" our required bag.
    If found, we "union" such sets
"""
from processors import process_line
from containers import fill_container, get_containers

with open('input-7.txt', 'r') as lines:
    for line in lines:
        outer, inner = process_line(line)
        fill_container(outer, inner)

    # [print(key + ': ' + str(value)) for key, value in bag_mapping.items()]

print(len(get_containers('shiny gold')))
````

Here're the `processors.py` module used:

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
    Group 1: Colour of single "inner" bag(s)
"""
inner_bag_regex = r'\d+ (.*) bags?'


def process_line(line):
    """    Processes the rule string to return "outer" bag and set of "inner bags"
    """

    line_parts = (re.match(line_parts_regex, line)).groups()
    # print(line_parts)

    """
    'no other bags' returns an empty set
    Otherwise, extract the 'group' that matches just the colour from "each inner" bag

    Populate the 'bag_mapping' dictionary in the form <str, List>
    """

    outer = line_parts[0]
    inner = set(
        (re.match(inner_bag_regex, s)).groups()[0] for s in line_parts[1].split(', ') if not s == 'no other bags'
    )

    return outer, inner
````

This time, the `containers.py` has some utility methods too as there is some logic worth encapsulating. A likely candidate that can benefit from being implemented a class

````python
""" Module to maintain datastructures that hold bag mappings
"""

"""
    We choose 'dict' which later comes to holds 'set' as value
"""
bag_mapping = {}


def fill_container(outer, inner):
    for bag in inner:
        if bag not in bag_mapping:
            bag_mapping[bag] = set()
        bag_mapping[bag].add(outer)


def get_containers(item_of_interest):
    """    Returns a set holding 'item of interest' from this container
    """

    set_of_interest = bag_mapping[item_of_interest]
    # print(set_of_interest)

    previous_len = -1
    while previous_len != len(set_of_interest):
        previous_len = len(set_of_interest)

        for key in set_of_interest:
            if key in bag_mapping:
                set_of_interest = set_of_interest.union(bag_mapping[key])
            # set_of_interest.discard('shiny gold')
    return set_of_interest
````

As this involves a union operation inside, the operation is again linearly dependent on length of set(s). So this amounts to O(n<sup>2</sup>)

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-1-attempt-1/
208

real    0m0.057s
user    0m0.053s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-1-attempt-1/
208

real    0m0.058s
user    0m0.054s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ time python3 part-1-attempt-1/
208

real    0m0.059s
user    0m0.048s
sys     0m0.012s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-7$ 
````

Do you have a better solution? Please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1337977124687400961) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. You [cannot `group of group`](https://stackoverflow.com/a/37004214) in regex. *i.e.*
    while the right part in

    ````regex
    (.*) contain (.*bags?),? (.* bags?).
    ````

    matches 2 single *bag(s)* occurrences, to match more than one such occurrence there is nothing like

    ````regex
    (.*) contain ((.*bags?),?)*.
    ````

    You would have to repeat a group *as many times as you expect it occur*
    *i.e.*

    ````regex
    (.*) contain (.*bags?),? (.* bags?),? (.* bags?),?.
    ````

    for matching/capturing 3 occurrences

2. You [can name groups](https://docs.python.org/3/library/re.html#re.Match.groupdict) in regex. For example,

    ````regex
    regex = r'(?P<outer>.*) contain (?P<inner>.*bags?),?.'
    ````

    lets you refer to *captured groups* later by names `outer` and `inner`

3. [Here](https://regexr.com/5ibl5) is a resource I used to come up with a suitable regex for my sample data

4. Modifying an iterable while iterating over it is still undesirable per my experience today
   1. Keeping track of length so as to compare *previous* to *current* to arrive at terminating condition for `while` loop is one approach I used today

5. Python allows you to return multiple values as `return valA, valB, valC` so you can capture it into 3 other values at the site of caller

6. Looking at my approach overall, I seem to be *writing it first all* as I solve the problem and then *proceeding to modularize*
   1. The process of *modularization* is such that there is an *imperative shell* that itself *manages all the dependencies itself*.
   2. All those depndencies are not aware of others. For example, see how `processors.py` and `containers.py` are completely unaware of each other. They do not *import* each other
   3. Only the *driver program* `__main.py__` is aware of them both
