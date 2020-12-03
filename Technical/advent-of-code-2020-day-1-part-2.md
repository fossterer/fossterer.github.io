# Advent of code 2020 - Day 1 (Part 2)

[Day - 1 of the series](https://twitter.com/SVRSN_Shashank/status/1334265716921528323) - The challenge is a variation on *3-sum problem* that goes like this:

You have an *array*. Find *3 values* such that their sum equals a *target value*

Click [here](advent-of-code-2020-day-1-part-1.md) for part-1

## Attempt 1: Time Complexity O(n<sup>2</sup>)

Having solved the [Part-1 (Attempt-1)](advent-of-code-2020-day-1-part-1.md) earlier I quickly jumped to extend it with an array of arrays as below

````python
""" We are going to build an upper triangular matrix where
    - for each new number seen
    -- We first check if any of the existing entries in non-first row add up to give required number
    -- If that fails, then we append a new list of size "one" higher than current
    - Every row (or list) holds sum of its first entry with all other firsts in order from top to bottom
    -
    - a   b   c   d   e   f   g
    -    a+b a+c a+d a+e a+f a+g
    -        b+c b+d b+e b+f b+g
    -            c+d c+e c+f c+g
    -                d+e d+f d+g
    -                    e+f e+g
    -                        f+g
"""
def execute():
    counter = -1

    for line in lines:
        """     
        for i in arr:
            print(i)
        
        print("") """
    
        val = int(line)
        counter += 1
    
        if counter == 0:
            arr.append([])
            arr[0].append([])
            arr[0][0] = val
            continue
    
        if counter == 1:
            arr.append([])
            arr[1].append([])
            arr[1][0] = val
    
            arr[1].append(arr[0][0] + arr[1][0])
            continue
    
        arr_enum_iterable = enumerate(iter(arr))
        next(arr_enum_iterable)
    
        for row_idx, row in arr_enum_iterable:
    
            row_enum_iterable = enumerate(iter(row))
            next(row_enum_iterable)
    
            for sum_idx, sum in row_enum_iterable:
                if val + sum == 2020:
                    print("Result: " + str(arr[row_idx][0] * arr[sum_idx - 1][0] * val))
                    return
    
        arr.append([])
    
        arr[counter].append([])
        arr[counter][0] = val
    
        for item in range(counter):
            arr[counter].append(arr[item][0] + val)

lines = open('input-1.txt', 'r')

arr = []

execute()
````

However, by the time of writing this blog post, I already assimilated into me that *position-less* maps/sets are to be preferred to an array. So I start thinking of maintaining 2 datastructures instead.

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-1.py 
Result: 85555470

real    0m0.089s
user    0m0.081s
sys     0m0.008s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-1.py 
Result: 85555470

real    0m0.091s
user    0m0.087s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-1.py 
Result: 85555470

real    0m0.093s
user    0m0.089s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ 
````

## Attempt 2: Time Complexity O(n)

We can't do with *sets* of course and need *hashMap* since after finding the target sum (2020 - value), we need to locate which 2 items led to that sum *originally*

````python
""" We are going to leverage 2 separate datastructures:
    (1) an array so we can retrieve numbers read via their sum (stored in the next datastructure)
    (2) a hashmap where
        - 'key' is a sum of 2 values and
        - 'value' is an ordered pair (a tuple) representing indices of the two values (from previous datastructure) that led to this sum
    - Now for the process,
    -- for each new value seen
    -- We first check if (2020 - value) exists in 'hashmap' as 'key'
    --- If 'yes', we retrieve its value for positions
    ---- Thereafter, retrieval of the 2 items from 'array' for multiplication are simply constant time operations. Then, we multiply, print and break
    -- If 'no', then
    --- (1) for each element already in array, we calculate sum and place it in our 'hashMap' along with positions (the second entry in position tuple being total size of set) and then
    --- (2) append this value to 'array' so it appears at that (1 + total older size of set)
    -
    - (1) array:        a   b   c   d   e
    - (2) hashmap:      a+b (0,1)
    -                   a+c (0,2)
    -                   b+c (1,2)
    -                   a+d (0,3)
    -                   b+d (1,3)
    -                   c+d (2,3)
    -                   a+e (0,4)
    -                   b+e (1,4)
    -                   c+e (2,4)
    -                   d+e (3,4)
"""
def execute():
    arr = []
    hashmap = {}

    for line in lines:
        
        """
        print("arr:")
        print(arr)
        
        print("hashmap:")
        print(hashmap)
        
        print("") """
    
        val = int(line)
    
        if len(arr) == 0:
            arr.append(val)
            continue
    
        if len(arr) == 1:
            hashmap[arr[0] + val] = (0, 1)
            arr.append(val)
    
            continue
    
        if (2020 - val) in hashmap:
            position_1, position_2 = hashmap[2020 - val]
            print("Result: " + str(arr[position_1] * arr[position_2] * val))
            return
    
        hashmap = {**hashmap, **{(entry + val): (index, len(arr)) for index, entry in enumerate(arr)}}
    
        arr.append(val)

lines = open('input-1.txt', 'r')

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-2.py 
Result: 85555470

real    0m0.027s
user    0m0.027s
sys     0m0.000s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-2.py 
Result: 85555470

real    0m0.028s
user    0m0.020s
sys     0m0.009s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ time python3 part-2-attempt-2.py 
Result: 85555470

real    0m0.047s
user    0m0.038s
sys     0m0.009s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ 
````

This renewed approach reduced runtime by a factor of 3 (and often only by a factor of 2; still better)

## Attempt 3: Reduced Space Usage; Time Complexity O(n)

Yet, another hole surfaces up this time. Why are we storing indices in one datastructure (hashmap) and later using those to *retrieve* from another datastructure (array)?

We could just be storing the values themselvves inside the 'positional pairs' and avoid array altogether. Right?

No. When we read in, say, a 10th item, we still need to generate its sum from values 0 to 9. If we don't have an array of *all seen so far* values, we cannot obtain this sum

We still need to have the 2 datastructure as separate. Do you have a better approach? [Tweet as a reply](https://twitter.com/SVRSN_Shashank/status/1334265716921528323) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. Storing *indices* from an array vs Storing *values* directly is a trade-off sometimes
   1. If you need repeated access, prefer having a position-base storage i.e separate array
