# Google CodeJam 2020 - Qualification Round - Problem 1 - Vestigium

In the first shot at the [problem](https://codingcompetitions.withgoogle.com/codejam/round/000000000019fd27/000000000020993c#problem) on paper, it becomes apparent that in a _Natural Latin Square_, the _trace_ always turns out to be `n*(n+1)/2`

The elements in such a _main diagonal_ may be either completely distinct from 1 to n or all same as the midpoint n/2 or (n+1)/2

## Attempt 1: Time Complexity O(n<sup>2</sup>)

By the end of this attempt, we arrive at no. of repeated rows

````python3
# Read rows and sum them
## If the sum doesn't add up to n(n+1)/2, call it a repeated row
### n^2 visits
# Read columns and sum them
## If the sum doesn't add up to n(n+1)/2, call it a repeated column
### n^2 visits again. total = 2*(n^2) visits

# if repeated_row and repeated_column are zeroes, directly print that
#  the trace is n*(n+1)/2

# else sum the diagonals
# n visits again. Total = 2*(n^2)+n visits

def execute():
    test_case_count = int(next(lines))

    for case_num in range(test_case_count):
        n = int(next(lines))
        expected_sum = n*(n+1)/2
        
        repeated_row_count = 0
        repeated_col_count = 0
        row_arr = []

        for row_id in range(n):
            row = next(lines)
            row_sum = sum(map(int, row.split()))
            if row_sum != expected_sum:
                repeated_row_count += 1
            row_arr.append(row)
        print("repeated row count: " + str(repeated_row_count))

lines = open('1_in', 'r')
execute()
````
