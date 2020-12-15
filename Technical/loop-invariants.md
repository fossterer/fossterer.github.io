# Invariants in computer science (and Loop Invariants)

While following [this debate](https://news.ycombinator.com/item?id=25417421) that brought up [RAII](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization), I ended up reading about [Loop Invariants](https://en.wikipedia.org/wiki/Loop_invariant)

So in essence, in the cases,

````java
while (i <= 10) {
        doSomething();
}
````

and

````c
for(i = 0; i <= n; i++) {
    do_something();
}
````

the *condition* we say

- is passing, as long as loop executes
 
and the very condition that we say

- failed when the looping action terminates

in formal terms is a *loop invariant*


Extending this idea, if you see some Java logic with `Assert()` statements, the implementation can be said to be confirming *invariance*
