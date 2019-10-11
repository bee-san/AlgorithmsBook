::: {.section .post-full-content}
::: {.post-content}
Divide and conquer algorithms aren\'t really taught in programming
textbooks, but it\'s something every programmer should know. Divide and
conquer algorithms are the backbone of concurrency and multi-threading.

Often I\'ll hear about how you can optimise a for loop to be faster or
how switch statements are slightly faster than if statements. Most
computers have more than one core, with the ability to support multiple
threads. Before worrying about optimising for loops or if statements try
to attack your problem from a different angle.

Divide and Conquer is one of the ways to attack a problem from a
different angle. Throughout this article, I\'m going to talk about
creating a divide and conquer solutions and what it is. Don\'t worry if
you have **zero** experience or knowledge on the topic. This article is
designed to be read by someone with very little programming knowledge.

I\'m going to explain this using 3 examples. The first will be a simple
explanation. The second will be some code. The final will get into the
mathematical core of divide and conquer techniques. (Don\'t worry, I
hate maths too).

------------------------------------------------------------------------

What is divide and conquer? 🌎 {#what-is-divide-and-conquer-}
-----------------------------

Divide and conquer is where you divide a large problem up into many
smaller, much easier to solve problems. The rather small example below
illustrates this.

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-14-.png){.kg-image}

We take the equation \"3 + 6 + 2 + 4\" and we cut it down into the
smallest  possible set of equations, which is \[3 + 6, 2 + 4\]. It could
also be \[2 + 3, 4 + 6\]. The order doesn\'t matter, as long as we turn
this one long equation into many smaller equations.

Let's say we have 8 numbers:

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-43-.png){.kg-image}

And we want to add them all together. We first divide the problem into 8
equal sub-problems. We do this by breaking the addition up into
individual numbers.

![](https://skerritt.blog/content/images/2019/03/image-33.png){.kg-image}

We then begin to add 2 numbers at a time.

![](https://skerritt.blog/content/images/2019/03/image-34.png){.kg-image}

Then 4 numbers into 8 numbers which is our resultant.

![](https://skerritt.blog/content/images/2019/03/image-35.png){.kg-image}

Why do we break it down to individual numbers at stage 1? Why don\'t we
just start from stage 2? Because while this list of numbers is even if
the list was odd you would need to break it down to individual numbers
to better handle it.

A divide and conquer algorithm tries to break a problem down into as
many little chunks as possible since it is easier to solve with little
chunks. It typically does this with recursion.

Formally the technique is, as defined in the famous [Introduction to
Algorithms](https://www.amazon.co.uk/Introduction-Algorithms-Thomas-H-Cormen/dp/0262033844/ref=as_li_ss_tl?keywords=Introduction+to+Algorithms&qid=1551954553&s=gateway&sr=8-1&linkCode=ll1&tag=brandon0fe-21&linkId=72a63dce0d8099988383cc3767340d40&language=en_GB)
by Cormen, Leiserson, Rivest, and Stein, is:

1.  Divide

If the problem is small, then solve it directly. Otherwise, divide the
problem into smaller subsets of the same problem.

2\. Conquer

Conquer the smaller problems by solving them recursively. If the
subproblems are small enough, recursion is not needed and you can solve
them directly.

Recursion is when a function calls itself. It\'s a hard concept to
understand if you\'ve never heard of it before. This page provides a
good explanation. In short, a recursive function is one like this:

``` {.language-python}
n = 6

def recur_factorial(n):
   if n == 1:
       return n
   else:
       return n * recur_factorial(n-1)

print(recur_factorial(n))
```

I\'ll fully explain the code in a second.

3\. Combine

Take the solutions to the sub-problems and merge them into a solution to
the original problem.

With the code from above, some important things to note. The Divide part
is also the recursion part. We divide the problem up at
`return n * recur_factorial(n-1)`.

Specifically, the `recur_factorial(n-1)` part is where we divide the
problem up.

The conquer part is the recursion part too, but also the if statement.
If the problem is small enough, we solve it directly (by returning n).
Else, we perform `return n * recur_factorial(n-1)`.

Combine. We do this with the multiplication symbol. Eventually, we
return the factorial of the number. If we didn\'t have the symbol there,
and it was `return recur_factorial(n-1)` it wouldn\'t combine and it
wouldn\'t output anything remotely similar to the factorial. (It\'ll
output 1, for those interested).

We\'re going to explore how divide and conquer works in some famous
algorithms, Merge Sort and the solution to the Towers of Hanoi.

------------------------------------------------------------------------

### Merge Sort 🤖 {#merge-sort-}

Merge Sort is a sorting algorithm. The algorithm works as follows:

-   Divide the sequence of n numbers into 2 halves
-   Recursively sort the two halves
-   Merge the two sorted halves into a single sorted sequence

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-48-.png){.kg-image}

In this image, we break down the 8 numbers into separate digits. Just
like we did earlier. Once we\'ve done this, we can begin the sorting
process.

It compares 51 and 13. Since 13 is smaller, it puts it in the left-hand
side. It does this for (10, 64), (34, 5), (32, 21).

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-49-.png){.kg-image}

It then merges (13, 51) with (10, 64). It knows that 13 is the smallest
in the first list, and 10 is the smallest in the right list. 10 is
smaller than 13, therefore we don\'t need to compare 13 to 64. We\'re
comparing & merging two **sorted** lists.

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-50-.png){.kg-image}

In recursion we use the term *base case* to refer to the absolute
smallest value we can deal with. With Merge Sort, the base case is 1.
That means we split the list up until we get sub-lists of length 1.
That\'s also why we go down all the way to 1 and not 2. If the base case
was 2, we would stop at the 2 numbers.

If the length of the list (n) is larger then 1, then we divide the list
and each sub-list by 2 until we get sub-lists of size 1. If n = 1, the
list is already sorted so we do nothing.

Merge Sort is an example of a divide and conquer algorithm. Let\'s look
at one more algorithm to really understand how divide and conquer works.

------------------------------------------------------------------------

### Towers of Hanoi 🗼 {#towers-of-hanoi-}

The Towers of Hanoi is a mathematical problem which consists of 3 pegs
and in this instance, 3 discs. This problem is mostly used to teach
recursion, but it does have some [real-world
uses.](https://www.ibm.com/developerworks/community/blogs/jfp/entry/towers_of_hanoi_at_large1?lang=en)

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-57-.png){.kg-image}

Each disc is a different size. We want to move all discs to peg C so
that the largest is on the bottom, second largest on top of the largest,
third largest (smallest) on top of all of them. There are some rules to
this game:

1.  We can only move 1 disc at a time.
2.  A disc cannot be placed on top of other discs that are smaller than
    it.

We want to use the smallest number of moves possible. If we have 1 disc,
we only need to move it once. If we have 2 discs, we need to move it 3
times.

To solve the above example we want to store the smallest disc in a
buffer peg (1 move). See below for a gif on solving Tower of Hanoi with
3 pegs and 3 discs.

![](https://skerritt.blog/content/images/2019/03/gify-1.gif){.kg-image}

Notice how we need to have a buffer to store the discs.

We can generalise this problem. If we have n discs: move n-1 from A to B
recursively, move largest from A to C, move n-1 from B to C recursively.

If there is an even number of pieces the first move is always into the
middle. If there are an odd number of pieces the first move is always to
the other end.

Let\'s begin to code the algorithm for ToH, in pseudocode.

    function MoveTower(disk, source, dest, spare):
        if disk == 0, then:
            move disk from source to dest

We start with a base case, `disk == 0`. `source` is the peg you\'re
starting at. `dest` is the final destination peg. `spare` is the spare
peg.

    FUNCTION MoveTower(disk, source, dest, spare):
    IF disk == 0, THEN:
        move disk from source to dest
    ELSE:
        MoveTower(disk - 1, source, spare, dest)   // Step 1
        move disk from source to dest              // Step 2
        MoveTower(disk - 1, spare, dest, source)   // Step 3
    END IF

Notice that with step 1 we switch `dest` and `source`. We do not do this
for step 3.

With recursion, we can be sure of 2 things:

1.  It always has a base case (if it doesn\'t, how does the algorithm
    know to end?)
2.  The function calls itself.

The algorithm gets a little confusing with steps 1 and 3. They both call
the same function. This is where multi-threading comes in. You can run
steps 1 and 3 on different threads - at the same time.

Since 2 is more than 1, we move it down one more level again. So far
you\'ve seen what the divide and conquer technique is. You should
understand how it works and what code looks like. Next, let\'s learn how
to formally define an algorithm to a problem using divide and conquer.
This part is the most important in my opinion. Once you know this,
it\'ll be exponentially easier to create divide and conquer algorithms.

------------------------------------------------------------------------

### Fibonacci Numbers 🐰 {#fibonacci-numbers-}

The Fibonacci numbers can be found in nature. The way [rabbits
produce](http://www.oxfordmathcenter.com/drupal7/node/487) is in the
style of the Fibonacci numbers. You have 2 rabbits that make 3, 3
rabbits make 5, 5 rabbits make 9 and so on.

The numbers start at 1 and the next number is the current number + the
previous number. Here it's 1 + 0 = 1. Then 1 + 1 = 2. 2 + 1 = 3 and so
on.

We can describe this relation using a recursion. A recurrence is an
equation which defines a function in terms of its smaller inputs.
Recurrence and recursion sound similar and are similar.

With Fibonacci numbers if n = 0 or 1, it results in 1. Else, recursively
add f(n-1) + f(n -2) until you reach the base case. Let\'s start off by
creating a non-recursive Fibonacci number calculator.

We know that if n = 0 or 1, return 1.

``` {.language-python}
def f(n):
    if n == 0 or n == 1:
        return 1
```

The Fibonacci numbers are the last two numbers added together.

``` {.language-python}
def f(n):
    if n == 0 or n == 1:
        return 1
    else:
        fibo = 1
        fibroPrev = 1
        for i in range (2, n):
            temp = fibo
            fibo = fibo + fiboPrev
            fiboPrev = temp
        return fibo
```

Now we\'ve seen this, let\'s turn it into recursion using a recurrence.


When creating a recurrence, we always start with the base case. The base
case here is if n == 0 or 1, return n.

If we don\'t return n, but instead return 1 this leads to a bug. For
example, F(0) would result in 1. When really, it should result in 0.

Next, we have the formula. If n isn\'t 0 or 1, what do we do? We
calculate F(n - 1) + F(n - 2). In the end, we want to merge all the
numbers together to get our final result. We do this using addition.



This is the formal definition of the Fibonacci numbers. Normally,
recurrences are used to talk about the running time of a divide and
conquer algorithm. My algorithms professor and I think it\'s actually a
good tool to create a divide and conquer algorithm.

``` {.language-python}
def F(n):
    if n == 0 or n == 1:
        return n
    else:
        return F(n-1)+F(n-2)
```

With knowledge of divide and conquer, the above code is cleaner and
easier to read.

We often calculate the result of a recurrence using an execution tree.
Computer overlords 🤖 don\'t need to do this, but it\'s useful for humans
to see how your divide and conquer algorithm works. For F(4) this looks
like:

![](https://skerritt.blog/content/images/2019/03/Blank-Diagram-30-.png){.kg-image}

n is 4, and n is larger than 0 or 1. So we do f(n-1) + f(n-2). We ignore
the addition for now. This results in 2 new nodes, 3 and 2. 3 is larger
than 0 or 1 so we do the same. Same for 2. We do this until we get a
bunch of nodes which are either 0 or 1. We then add all the nodes
together. 1 + 1 + 0 + 0 + 1 = 3, which is the right answer.

------------------------------------------------------------------------

Conclusion 📕 {#conclusion-}
------------

Once you\'ve identified how to break a problem down into many smaller
pieces, you can use concurrent programming to execute these pieces at
the same time (on different
[threads](https://www.wikiwand.com/en/Thread_(computing))) thereby
speeding up the whole algorithm.

Divide and conquer algorithms are one of the fastest and perhaps easiest
ways to increase the speed of an algorithm and are incredibly useful in
everyday programming. Here are the most important topics we covered in
this article:

-   What is divide and conquer?
-   Recursion
-   MergeSort
-   Towers of Hanoi
-   Coding a divide and conquer algorithm
-   Recurrences
-   Fibonaccinumbers

The next step is to explore multithreading. Choose your programming
language of choice and Google, as an example, \"Python multithreading\".
Figure out how it works and see if you can attack any problems in your
own code from this new angle.

You can also learn about how to solve recurrences (finding out the
asymptotic running time of a recurrence), which is the next article I\'m
going to write. If you don\'t want to miss it, or you liked this article
do consider subscribing to my email list 😁✨
:::
:::
