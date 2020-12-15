# Advent of code 2020 - Day 8 (Part 2)

[Day - 8](https://twitter.com/SVRSN_Shashank/status/1338968817658187782) Part 2 - I call this **Self-repair algorithm**. If you call [Part 1](advent-of-code-2020-day-8-part-1.md) an **infinite loop detection module** in a *static analysis tool*, this would be the **suggestion to remove infinite loops module**

In Attempt 1, you'll see my *failing code* and why I arrived at that it is an invalid approach for this problem

## Attempt 1: Time Complexity - N/A

Remember, I said the below in [Part 1](advent-of-code-2020-day-8-part-1.md)?

    Unlike in the previous challenges, since this has random jumps, we read all lines at once and proceed to process the lines later.

That tells us that I cannot *detect* (and thereby *prevent*) a *boot loop* instruction until it is encountered.

1. The challenge tells us that we are allowed to change *exactly* one # instruction
2. By trial (see the program next in this *Attempt 1*), we found that replacing a `jmp` with `nop` we ran into another loop which never existed prior to these changes
3. We further can make a case that, there could be many other hidden boot loops that never could have been encountered in the original execution path but after *repairs* like the one showed above, could be uncovered

The following is my *failing code*. Read along after the code to see my next attempt.
Here's my `__main__.py` used:

````python
""" Program to self-repair a repeated call to a certain instruction ("boot loop") in given boot code and complete execution

    Read all lines into an array.

    Setup an array that comes to store instruction index (from boot code) as it completes execution
    Setup 2 global variables 'acc' and 'pc' (Program Counter or Instruction Pointer)

    If an encountered instruction index is already found in "bitmap" and "length of history array" is not same as "length of boot code"-
    - repair boot code
    - reset all counters
    - repeat execution
    Else, we process the instruction at index given by "pc". If applicable, increment "acc"
    
    If "length of history array" is same as "length of boot code", we return the value of the global variable "acc".
"""
from containers import processor

with open('input-8.txt', 'r') as fp:
    processor_obj = processor()

    processor_obj.set_boot_code(fp.readlines())
    processor_obj.process_boot_code()
    processor_obj.print_acc()
````

Here's the `containers.py` with self-repair in naive attempt *i.e.* an extension of solution I designed in [Part 1](advent-of-code-2020-day-8-part-1.md)

````python
""" Module to maintain datastructures
"""


class processor:
    """ A 'Processor' abstraction

        Instance Variables
            boot_code		Set of instructions in boot code
            pc_history_map	A bitmap storing instruction index in 'boot_code' as it finishes execution
            acc				Accumulator
            pc				Program Counter
    """

    def __init__(self):
        print("Initializing..")
        self.boot_code = list()
        self.pc_history_map = list()
        self.acc = 0
        self.pc = 0

    def set_boot_code(self, lines):
        self.boot_code = lines

    def process_boot_code(self):
        """ Processes instructions one by one

            If "pc_history_map" contains True at an index, stops processing
        """
        while self.pc not in self.pc_history_map and len(self.pc_history_map) != len(self.boot_code):
            self.pc_history_map.append(self.pc)
            # print(self.pc_history_map)

            self.process_line(self.boot_code[self.pc])

        if len(self.pc_history_map) != len(self.boot_code):
            problem_instruction_index = self.pc_history_map[-1]
            print(self.boot_code[problem_instruction_index])

            problem_instruction = self.boot_code[problem_instruction_index]
            if 'jmp' in problem_instruction:
                self.boot_code[problem_instruction_index] = problem_instruction.replace(
                    'jmp', 'nop')
                print(self.boot_code[problem_instruction_index])
            elif 'nop' in problem_instruction:
                self.boot_code[problem_instruction_index] = problem_instruction.replace(
                    'nop', 'jmp')
                print(self.boot_code[problem_instruction_index])
            else:
                print('Wrong state!')

            self.pc_history_map = list()
            self.acc = 0
            self.pc = 0

            self.process_boot_code()

    def process_line(self, instruction):
        """    Processes the received "instruction"
        """
        operation, argument = instruction.split()

        if operation == 'nop':
            self.pc += 1
            return
        if operation == 'jmp':
            self.pc += int(argument)
            return
        if operation == 'acc':
            self.acc += int(argument)
            self.pc += 1
            return

    def print_acc(self):
        print(self.acc)
````

Here's the error I received:

````bash
[...]

jmp -5

jmp -5

nop -5

nop -5

jmp -5

jmp -5

nop -5

nop -5

jmp -5

jmp -5

nop -5

nop -5

jmp -5

jmp -5

nop -5

Traceback (most recent call last):
  File "/usr/lib/python3.8/runpy.py", line 194, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/usr/lib/python3.8/runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "part-2-attempt-1/__main__.py", line 22, in <module>
    processor_obj.process_boot_code()
  File "part-2-attempt-1/containers.py", line 56, in process_boot_code
    self.process_boot_code()
  File "part-2-attempt-1/containers.py", line 56, in process_boot_code
    self.process_boot_code()
  File "part-2-attempt-1/containers.py", line 56, in process_boot_code
    self.process_boot_code()
  [Previous line repeated 989 more times]
  File "part-2-attempt-1/containers.py", line 38, in process_boot_code
    print(self.boot_code[problem_instruction_index])
RecursionError: maximum recursion depth exceeded while calling a Python object
nop -5
````

As a result, I come to conclusion that this *self-repair* algorithm has to have these cases all built-in

1. Start attempt *n*
2. Repair single instruction (*i.e.* replace `jmp` with `nop` or vice-versa), if not marked *invalid repair spot*
3. Re-execute the boot code
   1. If another loop is encountered, mark that the *repair spot* is invalid and hence not to be attempted again
   2. Repeat. This time attempt *n+1*

## Attempt 2: Time Complexity O(???)

I considered avoiding having to reinitialize an object everytime by introducing a `reset()` method at first.

However, this appropach is inadequate since now I have to additionally remember which instruction I modified in this attempt so I can set it back before modifying another instruction.

To keep things less complex, the driver program just gets a new object everytime, remembers in its loop which instruction it is modifying so that next time, in a new object, it modifies some other line

Here's the slightly modified `__main__.py`:

````python
""" Program to self-repair a repeated call to a certain instruction ("boot loop") in given boot code and complete execution

    Read all lines into an array.

    Setup an array that comes to store instruction index (from boot code) as it completes execution
    Setup 2 global variables 'acc' and 'pc' (Program Counter or Instruction Pointer)

    If an encountered instruction index is already found in "bitmap" and "length of history array" is not same as "length of boot code"-
    - repair boot code
    - reset all counters
    - repeat execution
    Else, we process the instruction at index given by "pc". If applicable, increment "acc"

    If the next instruction index to execute ("pc") exceeds "length of boot code", we return the value of the global variable "acc".
"""
from containers import processor

with open('input-8.txt', 'r') as fp:
    original_boot_code = fp.readlines()

    jmp_or_nop_indices = [line_index for line_index, line in enumerate(
        original_boot_code) if 'jmp' in line or 'nop' in line]
    repair_iter = iter(jmp_or_nop_indices)
    # print(jmp_or_nop_indices)

    processor_obj = processor()
    while processor_obj.pc < len(original_boot_code):
        processor_obj = processor()
        processor_obj.set_boot_code(original_boot_code)
        processor_obj.repair_boot_code(next(repair_iter))

        processor_obj.process_boot_code()

    processor_obj.print_acc()
````

The `containers.py` posed an interesting challenge. I expected that by means of preserving `fp.readlines()` into `original_boot_code`, I can go back to the true copy after each repair attempt but I was wrong. The `set_boot_code()` in this `containers.py` was indeed passing a reference and as a result, every repair attempt was persisting forever. The trick was to use the *slicing operator* [:]

````python
""" Module to maintain datastructures
"""


class processor:
    """ A 'Processor' abstraction

        Instance Variables
            boot_code		Set of instructions in boot code
            pc_history_map	A bitmap storing instruction index in 'boot_code' as it finishes execution
            acc				Accumulator
            pc				Program Counter
    """

    def __init__(self):
        """ Constructor
        """

        # print("Initializing..")
        self.reset()

    def reset(self):
        """ Resets all properties except self.boot_code for a fresh execution
        """

        self.pc_history_map = list()
        self.acc = 0
        self.pc = 0

    def set_boot_code(self, lines):
        """ Initializes self.boot_code
        """

        self.boot_code = lines[:]
        # print(self.boot_code)

    def process_boot_code(self):
        """ Processes instructions one by one

            If "pc_history_map" contains True at an index, stops processing
        """

        while self.pc not in self.pc_history_map and self.pc < len(self.boot_code):
            self.pc_history_map.append(self.pc)
            # print(self.pc_history_map)
            # print('self.pc is ' + str(self.pc))

            self.process_line(self.boot_code[self.pc])

        if self.pc >= len(self.boot_code):
            print("Reached end successfully")
        else:
            # print("Detected boot loop")
            pass

    def process_line(self, instruction):
        """    Processes the received "instruction"
        """

        operation, argument = instruction.split()

        if operation == 'nop':
            self.pc += 1
            return
        if operation == 'jmp':
            self.pc += int(argument)
            return
        if operation == 'acc':
            self.acc += int(argument)
            self.pc += 1
            return

    def repair_boot_code(self, index_to_repair):
        """ Resets one of the instructions from *jmp* to *nop* or vice-versa
        """

        # print(self.boot_code[index_to_repair])

        problem_instruction = self.boot_code[index_to_repair]
        if 'jmp' in problem_instruction:
            self.boot_code[index_to_repair] = problem_instruction.replace(
                'jmp', 'nop')
            # print(self.boot_code[index_to_repair])
        elif 'nop' in problem_instruction:
            self.boot_code[index_to_repair] = problem_instruction.replace(
                'nop', 'jmp')
            # print(self.boot_code[index_to_repair])
        else:
            print('Wrong state!')

    def print_acc(self):
        print(self.acc)
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-2-attempt-1/
Reached end successfully
969

real    0m0.065s
user    0m0.056s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-2-attempt-1/
Reached end successfully
969

real    0m0.070s
user    0m0.066s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-2-attempt-1/
Reached end successfully
969

real    0m0.068s
user    0m0.063s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ 
````

Is there a better way than my brute force approach? Please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1338968817658187782) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. When copying iterables, use *slicing operator* [:] to create a new copy
   1. However, that does not mean [*deep copy*](https://stackoverflow.com/questions/2612802/list-changes-unexpectedly-after-assignment-how-do-i-clone-or-copy-it-to-prevent)
2. With all `print()`s enabled, my `time` showed `4+ seconds`, while with those disabled, it showed `0.07 seconds` at max
