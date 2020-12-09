# Advent of code 2020 - Day 4 (Part 2)

[Day - 4](https://twitter.com/SVRSN_Shashank/status/1336509402405347328). After [resolving](it-was-my-editor-newline-problem.md) troubles in Part 1, it took me very long to get back to work on Part 2

## Attempt 1: Time Complexity O(n)

As I saw the different conditionals (for the lack of switch-case in Python) going to make my code unreadable I started by vreaking down the existing code from [Part 1](advent-of-code-2020-day-4-part-1.md) beforehand

Here I implement *switch-case construct* using a *dictionary and method implementation* approach

````python
""" Program to process passport fields against certain validation rules

    Read every line until you encounter a blank line

    As soon as a line is read, split to detemrine keys
    For expected keys, apply validation rules to their values via a switch-case style construct
    When validations pass, set the flag against keys such as (byr:|iyr:| etc.) in a map to 'True'

    Once a blank line is encountered, count if flags on all fields excluding the optional 'cid' sum to 7.
    If the result is true, increment valid_passport_counter by 1
"""
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
valid_hcl_chars = set('0123456789abcdef')
valid_ecl_vals = ['amb', 'blu', 'brn', 'gry', 'grn', 'hzl', 'oth']


def process_byr_field(byr):
    return byr.isdigit() and int(byr) >= 1920 and int(byr) <= 2002


def process_iyr_field(iyr):
    return iyr.isdigit() and int(iyr) >= 2010 and int(iyr) <= 2020


def process_eyr_field(eyr):
    return eyr.isdigit() and int(eyr) >= 2020 and int(eyr) <= 2030


def process_hgt_field(hgt):
    if hgt.endswith('cm'):
        hgt = int(hgt[:-2])
        return hgt >= 150 and hgt <= 193
    if hgt.endswith('in'):
        hgt = int(hgt[:-2])
        return hgt >= 59 and hgt <= 76
    return False


def process_hcl_field(hcl):
    if hcl.startswith('#'):
        hcl = hcl[1:]
        return len(hcl) == 6 and all(char in valid_hcl_chars for char in hcl)
    return False


def process_ecl_field(ecl):
    return ecl in valid_ecl_vals


def process_pid_field(pid):
    return pid.isdigit() and len(pid) == 9


def default(param):
    pass


validators = {
    'byr': process_byr_field,
    'iyr': process_iyr_field,
    'eyr': process_eyr_field,
    'hgt': process_hgt_field,
    'hcl': process_hcl_field,
    'ecl': process_ecl_field,
    'pid': process_pid_field
}


def execute():
    with open('input-4.txt', 'r') as lines:
        for line in lines:
            if not line.strip():
                process_passport()
            else:
                process_fields(line)

    print(valid_passport_counter)


def process_passport():
    global valid_passport_counter
    valid_field_count = 0

    for key in field_keys:
        if key != 'cid' and fields[key] == True:
            # print(key)
            valid_field_count += 1

    if valid_field_count >= 7:
        valid_passport_counter += 1
        # print('Accepted')

    reset_fields()


def process_fields(line):
    for pair in line.split():
        key, val = pair.split(':')

        if key in field_keys:
            if validators.get(key, default)(val):
                fields[key] = True


def reset_fields():
    for key in field_keys:
        fields[key] = False


execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.029s
user    0m0.015s
sys     0m0.015s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.021s
user    0m0.017s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.022s
user    0m0.018s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ 
````

This is still too much logic to put in a single file. I wonder if I should have taken an object-oriented approach wrapping logic for validation of fields etc. into a 'Passport' class.

Possibly defining such a class in its own file. It would be interesting to compare time taken across these 2 approaches.

## Attempt 2: Time Complexity O(n)

Unlike originally intended, after giving it a thought, an object-oriented approach just to count and throw away the data seems expensive. Instead, I took a module-based approach to still deliver on the "readability" of code. Here is the same above logic split into 3 modules:

````python
""" Program to process passport fields against certain validation rules

    Read every line and process fields until you encounter a blank line

    Once a blank line is encountered, process the passport as a whole and increment valid_passport_counter by 1 if valid

    Reset fields to proceed to read the next passport
"""
from processors import process_fields, process_passport, reset_fields


def execute():
    valid_passport_counter = 0

    with open('input-4.txt', 'r') as lines:
        for line in lines:
            if not line.strip():
                valid_passport_counter += process_passport()
                reset_fields()

            else:
                process_fields(line)

    print(valid_passport_counter)


execute()
````

````python
""" Module to process fields in passport and passport as a whole as input in "Passport Processing" - Day 4 - Advent of Code 2020

    As soon as a line is read, split to determine keys
    For expected keys, apply validation rules to their values via a switch-case style construct
    When validations pass, set the flag against keys such as (byr:|iyr:| etc.) in a map to 'True'
"""
from validators import validators, default, fields, field_keys


def process_passport():
    """    Processes the passport by counting no. of valid fields

    Count if flags on all fields excluding the optional 'cid' sum to 7.
    If the result is true, increment valid_passport_counter by 1
    """

    valid_field_count = 0

    for key in field_keys:
        if key != 'cid' and fields[key] == True:
            # print(key)
            valid_field_count += 1

    return 1 if valid_field_count >= 7 else 0


def process_fields(line):
    """    Processes the line read by breaking into fields and marking them valid as applicable

    As soon as a line is read, split to detemrine keys
    For expected keys, apply validation rules to their values via a switch-case style construct
    When validations pass, set the flag against keys such as (byr:|iyr:| etc.) in a map to 'True'
    """

    for pair in line.split():
        key, val = pair.split(':')

        if key in field_keys:
            if validators.get(key, default)(val):
                fields[key] = True


def reset_fields():
    for key in field_keys:
        fields[key] = False
````

````python
""" Module with validators and structures holding them to apply to inputs in "Passport Processing" - Day 4 - Advent of Code 2020
"""

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
valid_hcl_chars = set('0123456789abcdef')
valid_ecl_vals = ['amb', 'blu', 'brn', 'gry', 'grn', 'hzl', 'oth']


def process_byr_field(byr):
    return byr.isdigit() and int(byr) >= 1920 and int(byr) <= 2002


def process_iyr_field(iyr):
    return iyr.isdigit() and int(iyr) >= 2010 and int(iyr) <= 2020


def process_eyr_field(eyr):
    return eyr.isdigit() and int(eyr) >= 2020 and int(eyr) <= 2030


def process_hgt_field(hgt):
    if hgt.endswith('cm'):
        hgt = int(hgt[:-2])
        return hgt >= 150 and hgt <= 193
    if hgt.endswith('in'):
        hgt = int(hgt[:-2])
        return hgt >= 59 and hgt <= 76
    return False


def process_hcl_field(hcl):
    if hcl.startswith('#'):
        hcl = hcl[1:]
        return len(hcl) == 6 and all(char in valid_hcl_chars for char in hcl)
    return False


def process_ecl_field(ecl):
    return ecl in valid_ecl_vals


def process_pid_field(pid):
    return pid.isdigit() and len(pid) == 9


def default(param):
    pass


validators = {
    'byr': process_byr_field,
    'iyr': process_iyr_field,
    'eyr': process_eyr_field,
    'hgt': process_hgt_field,
    'hcl': process_hcl_field,
    'ecl': process_ecl_field,
    'pid': process_pid_field
}
````

With this breaking down, I find the pieces of code are readily recognizable and the documentation over them to be to the point.

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.029s
user    0m0.015s
sys     0m0.015s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.021s
user    0m0.017s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ time python3 part-2-attempt-1.py
145

real    0m0.022s
user    0m0.018s
sys     0m0.005s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-4$ 
````

I don't think this modularization significantly affected anything. I still look forward to an opportunity where I can do similar coparison for a non-OOP implementation and an OOP implementation.

Do you have better suggestions to organize the code better? If so, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1336509402405347328) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. Python has no switch-case construct by default
   1. Instead, it is surprisingly simple to define your *cases* as *functions* and then constructing a *dictionary* with values of form *<case-identifier, method-name(without parantheses)>*
2. It is argued that to find length of an integer, the obvious `len(str(x))` is 3 times slower than `math.log10(x) + 1` [Ref](https://stackoverflow.com/questions/2189800/how-to-find-length-of-digits-in-an-integer)
   1. However, floating-point related errors are to be watched for in the case of huge numbers which require additional looping implementations
   2. The *math.log10()* doesn't work for hexadecimals, octals etc.
3. Modularization not only offers reusability and readability but further enhances latter with to the point documentation
