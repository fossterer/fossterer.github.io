# Advent of code 2020 - Day 3 (Part 2)

[Day - 3 of the series](https://twitter.com/SVRSN_Shashank/status/1335109439360278528) - At first I simply expected the solution to Part-1 to extend to this case except for a loop to handle varying step counts. Turned out I was horribly wrong

## Attempt 1: Time Complexity O(m*n)

Simply extending the previous [Part-1](advent-of-code-2020-day-3-part-1.md) meant in the final case (Right 1, down 2), horizontal position in line 2 comes out to be (2 * 1 = ) 2 when it should be 1!

````python
""" Let's say that our toboggan starts at (0,0).
    The co-ordinates we can visit are (1,3), (2, 6), (3, 9) and so on.
    
    i.e on  1st line, check if position 3 is #, on 2nd line check if position 6 is # and so on.

    As we can repeat each line horizontally as it is even if not provided in input, we simply divide with total length of line to arrive at the position.
    The remainder gives the position inside given input

    However, this approach works only when y_offset (i.e no. of lines to go down) is 1.
    As soon as y_offset becomes something else, we cannot rely on the line_number to determine the position on line

    So we externalize the position as an 'x_offset' and increment it by the 'count of right' for every successful line encountered (as a result of 'count of down')
"""
def execute():

    tree_count_product = 1
    with open('input-3.txt', 'r') as lines:
        line_length = 31
        for right, down in [(1, 1), (3, 1), (5, 1), (7, 1), (1, 2)]:
            # print(right, down)

            tree_counter = 0
            x_offset = -right
            for line_num, line in enumerate(lines):
                if line_num % down == 0:
                    x_offset += right
                    if line[x_offset % line_length] == '#':
                        # print(line_num)
                        tree_counter += 1

            tree_count_product *= tree_counter
            lines.seek(0, 0)
            # print("")
    print(tree_count_product)

execute()
````

Here's the time taken in 3 consecutive runs:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-2-attempt-1.py 
1574890240

real    0m0.018s
user    0m0.009s
sys     0m0.009s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-2-attempt-1.py 
1574890240

real    0m0.031s
user    0m0.020s
sys     0m0.012s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ time python3 part-2-attempt-1.py 
1574890240

real    0m0.019s
user    0m0.016s
sys     0m0.004s
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-3$ 
````

I don't see any better way of solving this. Do you? If so, please [Tweet your reply](https://twitter.com/SVRSN_Shashank/status/1335109439360278528) or [Open an Issue](https://github.com/fossterer/fossterer.github.io/issues) referencing the title.

## Lessons Learned

1. If you are ever asked to extend the first solution you proposed to a more bigger case, do not naively commit to use the same approach
   1. Always look at every extended question as a new first problem. Of course, in the process of arriving at the solution you can make use of ideas you proposed already
