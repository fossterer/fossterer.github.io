# Advent of code 2020 - Day 8 (Part 1)

[Day - 8](https://twitter.com/SVRSN_Shashank/status/1338968817658187782) Part 1 - Now that's the rudimentary computer model - A Program Counter (PC), an Accumulator (ACC) and a set of instructions under the name "boot code"

## Attempt 1: Time Complexity O(???)

Unlike in the previous challenges, since this has random jumps, we read all lines at once and proceed to process the lines later.
After all, that [kid's handheld](https://adventofcode.com/2020/day/8) device held all instructions in it. Why not our powerful Python runtimes eh?

Here's my `__main__.py` used to solve this challenge:

````python
""" Program to determine a repeated call to a certain instruction in given boot code

    Read all lines into an array.

    Setup a True/False bitmap of size same as no. of instructions in boot code, initially set to None/False
    Setup 2 global variables 'acc' and 'pc' (Program Counter or Instruction Pointer)

    If an encountered instruction index has True in "bitmap", we return the value of the global variable "acc".
    Else, we process the instruction at index given by "pc". If applicable, increment "acc"
"""
from containers import set_boot_code, process_boot_code, print_acc

with open('input-8.txt', 'r') as fp:
    set_boot_code(fp.readlines())
    process_boot_code()
    print_acc()
````

There's no separate `processors.py` this time. Of course I encountered a 'circular imports' complaint from Python.
The `containers.py` has all the required utility methods encapsulated inside it. A likely candidate that can benefit from being implemented a class

````python
""" Module to maintain datastructures
"""

boot_code = list()
pc_history_map = list()

acc = 0
pc = 0


def set_boot_code(lines):
    global boot_code, pc_history_map

    boot_code = lines
    pc_history_map = [False] * len(boot_code)


def process_boot_code():
    """    Processes instructions one by one

            If "pc_history_map" contains True at an index, stops processing
    """
    while pc_history_map[pc] == False:
        pc_history_map[pc] = True
        # print(pc_history_map)

        process_line(boot_code[pc])


def process_line(instruction):
    """    Processes the received "instruction"
    """
    global pc, acc
    operation, argument = instruction.split()

    if operation == 'nop':
        pc += 1
        return
    if operation == 'jmp':
        pc += int(argument)
        return
    if operation == 'acc':
        acc += int(argument)
        pc += 1
        return


def print_acc():
    print(acc)
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-1/
1810

real    0m0.025s
user    0m0.025s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-1/
1810

real    0m0.028s
user    0m0.024s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-1/
1810

real    0m0.049s
user    0m0.034s
sys     0m0.015s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ 
````

## Attempt 2: Time Complexity O(???)

I finally did it! OOP in Python.
Classes are a beautiful abstraction for one to have in their programs. See for ourself

Here's the slightly modified `__main__.py`:

````python
""" Program to determine a repeated call to a certain instruction in given boot code

    Read all lines into an array.

    Setup a True/False bitmap of size same as no. of instructions in boot code, initially set to None/False
    Setup 2 global variables 'acc' and 'pc' (Program Counter or Instruction Pointer)

    If an encountered instruction index has True in "bitmap", we return the value of the global variable "acc".
    Else, we process the instruction at index given by "pc". If applicable, increment "acc"
"""
from containers import processor

with open('input-8.txt', 'r') as fp:
    processor_obj = processor()

    processor_obj.set_boot_code(fp.readlines())
    processor_obj.process_boot_code()
    processor_obj.print_acc()
````

The `containers.py` contains instance variables and logic neatly encapsulated inside a *Processor* class

````python
""" Module to maintain datastructures
"""


class processor:
	""" A 'Processor' abstraction

		Instance Variables
			boot_code		Set of instructions in boot code
			pc_history_map	A boolean bitmap maintaining execution history of an instruction at its index in 'boot_code'
			acc				Accumulator
			pc				Program Counter
	"""

	def __init__(self):
		# print("Initializing..")
		self.boot_code = list()
		self.pc_history_map = list()
		self.acc = 0
		self.pc = 0

	def set_boot_code(self, lines):
		global boot_code, pc_history_map

		boot_code = lines
		pc_history_map = [False] * len(boot_code)

	def process_boot_code(self):
		""" Processes instructions one by one

			If "pc_history_map" contains True at an index, stops processing
		"""
		while pc_history_map[self.pc] == False:
			pc_history_map[self.pc] = True
			# print(pc_history_map)

			self.process_line(boot_code[self.pc])

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

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-2
1810

real    0m0.026s
user    0m0.022s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-2
1810

real    0m0.027s
user    0m0.023s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ time python3 part-1-attempt-2
1810

real    0m0.025s
user    0m0.021s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-8$ 
````

No more messy `global` declarations. No siginificant overhead due to OOP. Let's try to use OOP as much as we can as long as it makes sense hereafter

In a logic such as this where the user-provided JMP instructions totally determine how long the program runs, do we have a definite way for determining time complexity? Please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1338968817658187782) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. You cannot import 2 modules in each other. Python throws a *circular import` error
   1. However, the error is thrown only if you use the `from A import a` syntax
2. Python allows you to import inside a function just where necessary! This has a positive effect of *importing just when required*
