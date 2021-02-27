# Renaming files without leaving shell

After my first day of [Adevnt of code 2020 adventures](https://twitter.com/SVRSN_Shashank/status/1334265716921528323), I am left with multiple files just for one day.

In a bid to organize things well, I made up a folder `day-1` so I may move files inside it. Now, it doesn't make sense to qualify the files themselves with this prefix. I wanted to remove the suffix from all the files at once.

Here's how I did it without leaving my shell:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ ls
day-1-part-1-attempt-1.py  day-1-part-1-attempt-2.py  day-1-part-1-attempt-3.py  day-1-part-2-attempt-1.py  day-1-part-2-attempt-2.py  input-1.txt
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ for file in *; do echo $file; done
day-1-part-1-attempt-1.py
day-1-part-1-attempt-2.py
day-1-part-1-attempt-3.py
day-1-part-2-attempt-1.py
day-1-part-2-attempt-2.py
input-1.txt
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ for file in *
> do
> mv $file ${file/day-1-/}
> done
mv: 'input-1.txt' and 'input-1.txt' are the same file
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ ls
input-1.txt  part-1-attempt-1.py  part-1-attempt-2.py  part-1-attempt-3.py  part-2-attempt-1.py  part-2-attempt-2.py
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/programming-challenges/advent-of-code/2020/day-1$ 
````

## Update 1

Here's another way of doing it. Helps further if you have to find and replace a particular pattern:

````bash
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/JSON-java/src/test/resources$ for f in *; do echo $f; done;
Issue537.json
Issue537.xml
jsonpointer-testdoc.json
JSONUtilsTestIn1.json
JSONUtilsTestIn2_1.json
JSONUtilsTestIn2_2.json
JSONUtilsTestIn2_3.json
JSONUtilsTestOut1.json
JSONUtilsTestOut2_1_1.json
JSONUtilsTestOut2_1_2.json
JSONUtilsTestOut2_1_3.json
JSONUtilsTestOut2_2_1.json
JSONUtilsTestOut2_2_2.json
JSONUtilsTestOut2_3_1.json
JSONUtilsTestOut2_3_2.json
JSONUtilsTestOut2.json
JSONUtilsTestOut3.json
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/JSON-java/src/test/resources$ for f in *
> do
> mv $f "$(echo $f | sed 's/JSONUtils/JSONObjWithArrays/')"
> done
mv: 'Issue537.json' and 'Issue537.json' are the same file
mv: 'Issue537.xml' and 'Issue537.xml' are the same file
mv: 'jsonpointer-testdoc.json' and 'jsonpointer-testdoc.json' are the same file
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/JSON-java/src/test/resources$ ls
Issue537.json                  JSONObjWithArraysTestIn2_1.json  JSONObjWithArraysTestOut1.json      JSONObjWithArraysTestOut2_1_3.json  JSONObjWithArraysTestOut2_3_1.json  JSONObjWithArraysTestOut3.json
Issue537.xml                   JSONObjWithArraysTestIn2_2.json  JSONObjWithArraysTestOut2_1_1.json  JSONObjWithArraysTestOut2_2_1.json  JSONObjWithArraysTestOut2_3_2.json  jsonpointer-testdoc.json
JSONObjWithArraysTestIn1.json  JSONObjWithArraysTestIn2_3.json  JSONObjWithArraysTestOut2_1_2.json  JSONObjWithArraysTestOut2_2_2.json  JSONObjWithArraysTestOut2.json
shashank@shashank-HP-ENVY-Notebook:~/Projects/personal/JSON-java/src/test/resources$ 
````
