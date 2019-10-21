This is a book I wrote on Algorithmic Design Paradigms (Greedy, Divide & Conquer, and Dynamic Programming). Gifted to you, for free ✨
Want it in a nicely formatted, typeset PDF?
With extra examples, beautiful graphs + more. **All for free?**
Sign up to my email list below and I'll gift it to you 😁

# Greedy Algorithms
**Greedy algorithms** aim to make the optimal choice at that given moment. Each step it chooses the optimal choice, without knowing the future. It attempts to find the globally optimal way to solve the entire problem using this method.

## Why Are Greedy Algorithms Called Greedy?

Algorithms are called _greedy_ when they utilise the greedy property. The greedy property is:

> At that exact moment in time, what is the optimal choice to make?

Greedy algorithms are greedy. They do not look into the future to decide the global optimal solution. They are only concerned with the optimal solution locally. This means that the overall optimal solution may be different from the solution the algorithm chooses.

They never look backwards at what they've done to see if they could optimise globally. This is the main difference between Greedy and [Dynamic Programming](https://skerritt.blog/dynamic-programming/).

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## What Are Greedy Algorithms Used For?

Greedy algorithms are very fast. A lot faster than the two other alternatives (Divide & Conquer, and Dynamic Programming). They're used because they're fast.

Most of the popular algorithms using Greedy have shown that Greedy gives the global optimal solution every time. Some of these include:

- [Dijkstra's Algorithm](https://www.cs.cmu.edu/~deva/papers/tracking2011.pdf)
- [Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm) 
- [Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)
- [Huffman trees](https://en.wikipedia.org/wiki/Huffman_tree)

We're going to explore greedy algorithms using a famous example - counting change. There isn't much to this paradigm.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## How Do I Create a Greedy Algorithm?

Your algorithm needs to follow this property:

> At that exact moment in time, what is the optimal choice to make?

And that's it. There isn't much to it. Greedy algorithms are generally easier to code than [Divide & Conquer](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga) or [Dynamic Programming](https://skerritt.blog/dynamic-programming/).

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
# Counting Change Using Greedy

Imagine you're a vending machine. Someone gives you £1 and buys a drink for £0.70p. There's no 30p coin in [pound sterling](https://en.wikipedia.org/wiki/Pound_sterling), how do you calculate how much change to return?

For reference, this is the denomination of each coin in the UK:

<!--kg-card-begin: code-->
```
1p, 2p, 5p, 10p, 20p, 50p, £1
```
<!--kg-card-end: code-->

The greedy algorithm starts from the highest denomination and works backwards. Our algorithm starts at £1. £1 is more than 30p, so it can't use it. It does this for 50p. It reaches 20p. 20p < 30p, so it takes 1 20p.

The algorithm needs to return change of 10p. It tries 20p again, but 20p > 10p. It then goes to 10p. It chooses 1 10p, and now our return is 0 we stop the algorithm.

We return 1x20p and 1x10p.

This algorithm works quite well in real life. Let's use another example, this time we have the denomination next to how many of that coin is in the machine, `(denomination, how many)`.

<!--kg-card-begin: code-->
```python
(1p, 10), (2p, 3), (5p, 1), (10p, 0), (20p, 1p), (50p, 19p), (100p, 16)
```
<!--kg-card-end: code-->

The algorithm is asked to return change of 30p again. 100p (£1) is no. Same for 50. 20p, we can do that. We pick 1x 20p. We now need to return 10p. 20p has run out, so we move down 1.

10p has run out, so we move down 1.

We have 5p, so we choose 1x5p. We now need to return 5p. 5p has run out, so we move down one.

We choose 1 2p coin. We now need to return 3p. We choose another 2p coin. We now need to return 1p. We move down one.

We choose 1x 1p coin.

In total, our algorithm selected these coins to return as change:

<!--kg-card-begin: code-->
```python
# (value, how many we return as change)
(10, 1)
(5, 1)
(2, 2)
(1, 1)
```
<!--kg-card-end: code-->

Let's code something. First, we need to define the problem. We'll start with the denominations.

<!--kg-card-begin: code-->
```python
denominations = [1, 2, 5, 10, 20, 50, 100]
# 100p is £1
```
<!--kg-card-end: code-->

Now onto the core function. Given denominations and an amount to give change, we want to return a list of how many times that coin was returned.

If our `denominations` list is as above, then `[6, 3, 0, 0, 0, 0, 0]` represents taking 6 1p coins and 3 2p coins, but 0 of all other coins.

<!--kg-card-begin: code-->
```python
denominations = [1, 2, 5, 10, 20, 50, 100]
# 100p is £1

def returnChange(change, denominations):
	toGiveBack = [0] * len(denominations)
	for pos, coin in reversed(list(enumerate(denominations))):

```
<!--kg-card-end: code-->

We create a list, the size of denominations long and fill it with 0's.

We want to loop backwards, from largest to smallest. `Reversed(x)` reverses x and lets us loop backwards. Enumerate means "for loop through this list, but keep the position in another variable". In our example when we start the loop. `coin = 100` and `pos = 6`.

Our next step is repeatedly choosing a coin for as long as we can use that coin. If we need to give `change = 40` we want our algorithm to choose 20, then 20 again until it can no longer use 20. We do this using a for loop.

<!--kg-card-begin: code-->
```python
denominations = [1, 2, 5, 10, 20, 50, 100]
# 100p is £1

def returnChange(change, denominations):
	# makes a list size of length denominations filled with 0
	toGiveBack = [0] * len(denominations)

	# goes backwards through denominations list
	# and also keeps track of the counter, pos.
	for pos, coin in enumerate(reversed(denominations)):
		# while we can still use coin, use it until we can't
		while coin <= change:

```
<!--kg-card-end: code-->

While the coin can still fit into change, add that coin to our return list, `toGiveBack` and remove it from change.

<!--kg-card-begin: code-->
```python
denominations = [1, 2, 5, 10, 20, 50, 100]
# 100p is £1

def returnChange(change, denominations):
	# makes a list size of length denominations filled with 0
	toGiveBack = [0] * len(denominations)

	# goes backwards through denominations list
	# and also keeps track of the counter, pos.
	for pos, coin in enumerate(reversed(denominations)):
		# while we can still use coin, use it until we can't
		while coin <= change:
			change = change - coin
			toGiveBack[pos] += 1
	return(toGiveBack)
			
print(returnChange(30, denominations))
# returns [0, 0, 0, 1, 1, 0, 0]
# 1x 10p, 1x 20p
```
<!--kg-card-end: code-->

The [runtime](https://skerritt.blog/you-need-to-understand-big-o-notation-now/)of this algorithm is dominated by the 2 loops, thus it is O(n^2).

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## Is Greedy Optimal? Does Greedy Always Work?

It is optimal locally, but sometimes it isn't optimal globally. In the change giving algorithm, we can force a point at which it isn't optimal globally.

The algorithm for doing this is:

- Pick 3 denominations of coins. 1p, x, and less than 2x but more than x.

We'll pick 1, 15, 25.

- Ask for change of 2 \* second denomination (15)

We'll ask for change of 30. Now, let's see what our Greedy algorithm does.

<!--kg-card-begin: code-->
```
[5, 0, 1]
```
<!--kg-card-end: code-->

It choses 1x 25p, and 5x 1p. The optimal solution is 2x 15p.

Our Greedy algorithm failed because it didn't look at 15p. It looked at 25p and thought "yup, that fits. Let's take it."

It then looked at 15p and thought "that doesn't fit, let's move on".

This is an example of where Greedy Algorithms fail.

To get around this, you would either have to create currency where this doesn't work or to brute-force the solution. Or use Dynamic Programming.

Greedy Algorithms are sometimes globally optimal. From earlier, we saw these algorithms are globally optimal:

- [Dijkstra's Algorithm](https://www.cs.cmu.edu/~deva/papers/tracking2011.pdf)
- [Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm) 
- [Prim's algorithm](https://en.wikipedia.org/wiki/Prim%27s_algorithm)
- [Huffman trees](https://en.wikipedia.org/wiki/Huffman_tree)

There are other globally optimal solutions, but Greedy is faster and easier to program than these solutions.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
# Dijkstra's Algorithm

Dijkstra's algorithm finds the shortest path from a node to every other node in the graph. In our example, we'll be using a [weighted directed graph](https://skerritt.blog/graph-theory/). Each edge has a direction, and each edge has a weight.

Dijkstra's algorithm has many uses. It can be very useful within road networks where you need to find the fastest route to a place. The algorithm is also used for:

- [IP Routing](https://ieeexplore.ieee.org/document/5381955)
- [A\* Algorithm](https://dev.to/brandonskerritt/a-primer-on-search-algorithms-1768-temp-slug-3978406)
- [Telephone networks](https://www.sciencedirect.com/science/article/pii/S0020019006003498)

The algorithm follows these rules:

1. Every time we want to visit a new node, we will choose the node with the smallest known distance.
2. Once we've moved to the node, we check each of its neighbouring nodes. We calculate the distance from the neighbouring nodes to the root nodes by summing the cost of the edges that lead to that new node.
3. If the distance to a node is less than a known distance, we'll update the shortest distance.


![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-33-.png)



Our first step is to pick the starting node. Let's choose A. All the distances start at infinity, as we don't know their distance until we reach a node that does know the distance.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-35-.png)



We mark off A on our unvisited nodes list. The distance from A to A is 0. The distance from A to B is 4. The distance from A to C is 2. We updated our distance listing on the right-hand side.

We then pick the smallest edge where the vertex hasn't been chosen. The smallest edge is A -> C, and we haven't chosen C yet. We visit C.

Notice how we're picking the smallest distance from our current node to a node we haven't visited yet. We're being greedy. In this case, the greedy method is the global optimal solution.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-36-.png)

<!--kg-card-end: image-->

We can get to B from C. We now need to pick a minimum. min(4, 2 + 1) = 3.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-37-.png)

<!--kg-card-end: image-->

Since A -> C -> B is smaller than A -> B, we update B with this information. We then add in the distances from the other nodes we can now reach.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-40-.png)

<!--kg-card-end: image-->

Our next smallest vertex with a node we haven't visited yet is B, with 3. We visit B.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-41-.png)

<!--kg-card-end: image-->

We do the same for B. Then we pick the smallest vertex we haven't visited yet, D.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-42-.png)

<!--kg-card-end: image-->

We don't update any of the distances this time. Our last node is then E.



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-43-.png)

<!--kg-card-end: image-->

There are no updates again. To find the shortest path from A to the other nodes, we walk back through our graph.

We pick A first, then C, then B. If you need to create the shortest path from A to every other node as a graph, you can run this algorithm using a table on the right hand side.

<!--kg-card-begin: html-->

![img](https://skerritt.blog/content/images/2019/06/Screenshot_2019-06-23-Greedy-Algorithms.png)

<!--kg-card-end: html-->

Using this table it is easy to draw out the shortest distance from A to every other node in the graph:



![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/Blank-Diagram-44-.png)


* * *
<!--kg-card-end: hr-->
## Fractional Knapsack Problem Using Greedy Algorithm

Imagine you are a thief. You break into the house of Judy Holliday - [1951 Oscar winner for Best Actress](https://en.wikipedia.org/wiki/23rd_Academy_Awards). Judy is a hoarder of gems. Judy's house is lined to the brim with gems.

You brought with you a bag - a knapsack if you will. This bag has a weight of 7. You happened to have a listing of all of Judy's items, from some insurance paper. The items read as:

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/Screenshot_2019-06-23-Greedy-Algorithms-1-.png

<!--kg-card-end: html-->

The first step to solving the fractional knapsack problem is to calculate value/weight for each item.

<!--kg-card-begin: html-->

![img](https://skerritt.blog/content/images/2019/06/Screenshot_2019-06-23-Greedy-Algorithms-2-.png)

<!--kg-card-end: html-->

And now we greedily select the largest ones. To do this, we can sort them according to value/weight} in descending order. Luckily for us, they are already sorted. The largest one is 3.2.

<!--kg-card-begin: code-->
```
knapsack value = 16
knapsack total weight = 5 (out of 7)
```
<!--kg-card-end: code-->

Then we select Francium (I know it's not a gem, but Judy is a bit strange 😉)

<!--kg-card-begin: code-->
```
knapsack value = 19
knapsack weight = 6
```
<!--kg-card-end: code-->

Now, we add Sapphire. But if we add Sapphire, our total weight will come to 8.

In the fractional knapsack problem, we can cut items up to take fractions of them. We have a weight of 1 left in the bag. Our sapphire is weight 2. We calculate the ratio of:

Weight of knapsack left / weight of item

And then multiply this ratio by the value of the item to get how much value of that item we can take.

1/2 * 6 = 3

<!--kg-card-begin: code-->
```
knapsack value = 21
knapsack weight = 7
```
<!--kg-card-end: code-->

The greedy algorithm can optimally solve the fractional knapsack problem, but it cannot optimally solve the {0, 1} knapsack problem. In this problem instead of taking a fraction of an item, you either take it {1} or you don't {0}. To solve this, you need to use [Dynamic Programming](https://skerritt.blog/dynamic-programming/).

The [runtime](https://skerritt.blog/you-need-to-understand-big-o-notation-now/)for this algorithm is O(n log n). Calculating value/weight is O(1). Our main step is sorting from largest value/weight, which takes O(n log n) time.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## Greedy vs Divide & Conquer vs Dynamic Programming
<!--kg-card-begin: html-->


<!--kg-card-end: html-->

![Greedy Algorithms](https://skerritt.blog/content/images/2019/06/image-10.png)

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## Conclusion

Greedy algorithms are very fast, but may not provide the optimal solution. They are also easier to code than their counterparts.

# Divide & Conquer
Divide and conquer algorithms aren't really taught in programming textbooks, but it's something every programmer should know. Divide and conquer algorithms are the backbone of concurrency and multi-threading.

Often I'll hear about how you can optimise a for loop to be faster or how switch statements are slightly faster than if statements. Most computers have more than one core, with the ability to support multiple threads. Before worrying about optimising for loops or if statements try to attack your problem at a different angle.

Divide and Conquer is one of the ways to attack a problem from a different angle. Throughout this article, I'm going to talk about creating a divide and conquer solutions and what it is. Don't worry if you have **zero** experience or knowledge on the topic. This article is designed to be read by someone with very little programming knowledge.

I'm going to explain this using 3 examples. The first will be a simple explanation. The second will be some code. The final will get into the mathematical core of divide and conquer techniques. (Don't worry, I hate maths too).

**No time to read this?** [Sign](https://pages.convertkit.com/f01a7359c0/8d197d69e0) up to my email list to get this in PDF form. You'll also get some extra content that isn't in this post ✨ [Sign up here.](https://pages.convertkit.com/f01a7359c0/8d197d69e0)

## What is divide and conquer? 🌎

Divide and conquer is where you divide a large problem up into many smaller, much easier to solve problems. The rather small example below illustrates this.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-14-.png)

<!--kg-card-end: image-->

We take the equation "3 + 6 + 2 + 4" and we cut it down into the smallest  possible set of equations, which is [3 + 6, 2 + 4]. It could also be [2 + 3, 4 + 6]. The order doesn't matter, as long as we turn this one long equation into many smaller equations.

Let’s say we have 8 numbers:

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-43-.png)

<!--kg-card-end: image-->

And we want to add them all together. We first divide the problem into 8 equal sub-problems. We do this by breaking the addition up into individual numbers.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/image-33.png)

<!--kg-card-end: image-->

We then begin to add 2 numbers at a time.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/image-34.png)

<!--kg-card-end: image-->

Then 4 numbers into 8 numbers which is our resultant.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/image-35.png)

<!--kg-card-end: image-->

Why do we break it down to individual numbers at stage 1? Why don't we just start from stage 2? Because while this list of numbers is even if the list was odd you would need to break it down to individual numbers to better handle it.

A divide and conquer algorithm tries to break a problem down into as many little chunks as possible since it is easier to solve with little chunks. It typically does this with recursion.

Formally the technique is, as defined in the famous [Introduction to Algorithms](https://www.amazon.co.uk/Introduction-Algorithms-Thomas-H-Cormen/dp/0262033844/ref=as_li_ss_tl?keywords=Introduction+to+Algorithms&qid=1551954553&s=gateway&sr=8-1&linkCode=ll1&tag=brandon0fe-21&linkId=72a63dce0d8099988383cc3767340d40&language=en_GB) by Cormen, Leiserson, Rivest, and Stein is:

1. Divide

If the problem is small, then solve it directly. Otherwise, divide the problem into smaller subsets of the same problem.

2. Conquer

Conquer the smaller problems by solving them recursively. If the subproblems are small enough, recursion is not needed and you can solve them directly.

Recursion is when a function calls itself. It's a hard concept to understand if you've never heard of it before. This page provides a good explanation. In short, a recursive function is one like this:


```python
n = 6

def recur_factorial(n):
   if n == 1:
       return n
   else:
       return n * recur_factorial(n-1)

print(recur_factorial(n))
```

I'll fully explain the code in a second.

3. Combine

Take the solutions to the sub-problems and merge them into a solution to the original problem.

With the code from above, some important things to note. The Divide part is also the recursion part. We divide the problem up at `return n * recur_factorial(n-1)`.

Specifically, the `recur_factorial(n-1)` part is where we divide the problem up.

The conquer part is the recursion part too, but also the if statement. If the problem is small enough, we solve it directly (by returning n). Else, we perform `return n * recur_factorial(n-1)`.

Combine. We do this with the multiplication symbol. Eventually, we return the factorial of the number. If we didn't have the symbol there, and it was `return recur_factorial(n-1)` it wouldn't combine and it wouldn't output anything remotely similar to the factorial. (It'll output 1, for those interested).

We're going to explore how divide and conquer works in some famous algorithms, Merge Sort and the solution to the Towers of Hanoi.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
### Merge Sort 🤖

Merge Sort is a sorting algorithm. The algorithm works as follows:

- Divide the sequence of n numbers into 2 halves
- Recursively sort the two halves
- Merge the two sorted halves into a single sorted sequence

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-48-.png)

<!--kg-card-end: image-->

In this image, we break down the 8 numbers into separate digits. Just like we did earlier. Once we've done this, we can begin the sorting process.

It compares 51 and 13. Since 13 is smaller, it puts it in the left-hand side. It does this for (10, 64), (34, 5), (32, 21).

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-49-.png)

<!--kg-card-end: image-->

It then merges (13, 51) with (10, 64). It knows that 13 is the smallest in the first list, and 10 is the smallest in the right list. 10 is smaller than 13, therefore we don't need to compare 13 to 64. We're comparing & merging two **sorted** lists.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-50-.png)

<!--kg-card-end: image-->

In recursion we use the term _base case_ to refer to the absolute smallest value we can deal with. With Merge Sort, the base case is 1. That means we split the list up until we get sub-lists of length 1. That's also why we go down all the way to 1 and not 2. If the base case was 2, we would stop at the 2 numbers.

If the length of the list (n) is larger then 1, then we divide the list and each sub-list by 2 until we get sub-lists of size 1. If n = 1, the list is already sorted so we do nothing.

Merge Sort is an example of a divide and conquer algorithm. Let's look at one more algorithm to really understand how divide and conquer works.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
### Towers of Hanoi 🗼

The Towers of Hanoi is a mathematical problem which consists of 3 pegs and in this instance, 3 discs. This problem is mostly used to teach recursion, but it does have some [real world uses.](https://www.ibm.com/developerworks/community/blogs/jfp/entry/towers_of_hanoi_at_large1?lang=en)

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-57-.png)

<!--kg-card-end: image-->

Each disc is a different size. We want to move all discs to peg C so that the largest is on the bottom, second largest on top of the largest, third largest (smallest) on top of all of them. There are some rules to this game:

1. We can only move 1 disc at a time.
2. A disc cannot be placed on top of other discs that are smaller than it.

We want to use the smallest number of moves possible. If we have 1 disc, we only need to move it once. If we have 2 discs, we need to move it 3 times.

The number of moves is powers of 2 minus 1. If we have 4 discs, we calculate the minimum number of moves as 2^4 = 16 - 1 = 15.

To solve the above example we want to store the smallest disc in a buffer peg (1 move). See below for a gif on solving Tower of Hanoi with 3 pegs and 3 discs.

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/gify-1.gif)

<!--kg-card-end: image-->

Notice how we need to have a buffer to store the discs.

We can generalise this problem. If we have n discs: move n-1 from A to B recursively, move largest from A to C, move n-1 from B to C recursively.

If there is an even number of pieces the first move is always into the middle. If there are an odd number of pieces the first move is always to the other end.

Let's begin to code the algorithm for ToH, in pseudocode.

<!--kg-card-begin: code-->
```
function MoveTower(disk, source, dest, spare):
    if disk == 0, then:
        move disk from source to dest
```
<!--kg-card-end: code-->

We start with a base case, `disk == 0`. `source` is the peg you're starting at. `dest` is the final destination peg. `spare` is the spare peg.

<!--kg-card-begin: code-->
```
FUNCTION MoveTower(disk, source, dest, spare):
IF disk == 0, THEN:
    move disk from source to dest
ELSE:
    MoveTower(disk - 1, source, spare, dest) // Step 1
    move disk from source to dest // Step 2
    MoveTower(disk - 1, spare, dest, source) // Step 3
END IF
```
<!--kg-card-end: code-->

Notice that with step 1 we switch `dest` and `source`. We do not do this for step 3.

With recursion, we can be sure of 2 things:

1. It always has a base case (if it doesn't, how does the algorithm know to end?)
2. The function calls itself.

The algorithm gets a little confusing with steps 1 and 3. They both call the same function. This is where multi-threading comes in. You can run steps 1 and 3 on different threads - at the same time.

Since 2 is more than 1, we move it down one more level again. So far you've seen what the divide and conquer technique is. You should understand how it works and what code looks like. Next, let's learn how to formally define an algorithm to a problem using divide and conquer. This part is the most important in my opinion. Once you know this, it'll be exponentially easier to create divide and conquer algorithms.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
### Fibonacci Numbers 🐰

The Fibonacci numbers can be found in nature. The way [rabbits produce](http://www.oxfordmathcenter.com/drupal7/node/487)is in the style of the Fibonacci numbers. You have 2 rabbits that make 3, 3 rabbits make 5, 5 rabbits make 9 and so on.

The numbers start at 1 and the next number is the current number + the previous number. Here it’s 1 + 0 = 1. Then 1 + 1 = 2. 2 + 1 = 3 and so on.

We can describe this relation using a recursion. A recurrence is an equation which defines a function in terms of its smaller inputs. Recurrence and recursion sound similar and are similar.

With Fibonacci numbers if n = 0 or 1, it results in 1. Else, recursively add f(n-1) + f(n -2) until you reach the base case. Let's start off by creating a non-recursive Fibonacci number calculator.

We know that if n = 0 or 1, return 1.

```python
def f(n):
    if n == 0 or n == 1:
        return 1
```

The Fibonacci numbers are the last two numbers added together.

```python
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

Now we've seen this, let's turn it into recursion using a recurrence.

![Formula](https://skerritt.blog/content/images/2019/03/image-36.png)

When creating a recurrence, we always start with the base case. The base case here is if n == 0 or 1, return n.

If we don't return n, but instead return 1 this leads to a bug. For example, F(0) would result in 1. When really, it should result in 0.

Next, we have the formula. If n isn't 0 or 1, what do we do? We calculate F(n - 1) + F(n - 2). In the end, we want to merge all the numbers together to get our final result. We do this using addition.

![formula](https://skerritt.blog/content/images/2019/03/image-37.png)

This is the formal definition of the Fibonacci numbers. Normally, recurrences are used to talk about the running time of a divide and conquer algorithm. My algorithms professor and I think it's actually a good tool to create divide and conquer algorithms.

```python
def F(n):
  if n == 0 or n == 1:
    return n
  else:
    return F(n-1)+F(n-2)
```

With knowledge of divide and conquer, the above code is cleaner and easier to read.

We often calculate the result of a recurrence using an execution tree. Computer overlords 🤖 don't need to do this, but it's useful for humans to see how your divide and conquer algorithm works. For F(4) this looks like:

<!--kg-card-begin: image-->

![A Gentle Introduction to Divide and Conquer Algorithms](https://skerritt.blog/content/images/2019/03/Blank-Diagram-30-.png)

<!--kg-card-end: image-->

n is 4, and n is larger than 0 or 1. So we do f(n-1) + f(n-2). We ignore the addition for now. This results in 2 new nodes, 3 and 2. 3 is larger than 0 or 1 so we do the same. Same for 2. We do this until we get a bunch of nodes which are either 0 or 1. We then add all the nodes together. 1 + 1 + 0 + 0 + 1 = 3, which is the right answer.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## Conclusion 📕

Once you've identified how to break a problem down into many smaller pieces, you can use concurrent programming to execute these pieces at the same time (on different [threads](https://www.wikiwand.com/en/Thread_(computing))) thereby speeding up the whole algorithm.

Divide and conquer algorithms are one of the fastest and perhaps easiest ways to increase the speed of an algorithm and are incredibly useful in everyday programming. Here are the most important topics we covered in this article:

- What is divide and conquer?
- Recursion
- MergeSort
- Towers of Hanoi
- Coding a divide and conquer algorithm
- Recurrences
- Fibonacci numbers

The next step is to explore multithreading. Choose your programming language of choice and Google, as an example, "Python multithreading". Figure out how it works and see if you can attack any problems in your own code from this new angle.

You can also learn about how to solve recurrences (finding out the asymptotic running time of a recurrence), which is the next article I'm going to write. If you don't want to miss it, or you liked this article do consider subscribing to my email list 😁✨

# Dynamic Programming
Dynamic programming (DP) is breaking down an optimisation problem into smaller sub-problems, and storing the solution to each sub-problems so that each sub-problem is only solved once.

It is both a [mathematical optimisation](https://en.wikipedia.org/wiki/Mathematical_optimization) method and a computer programming method.

_Optimisation problems_ seek the maximum or minimum solution. Dynamic programming is often used for optimisation problems. The general rule is that if you encounter a problem where the initial algorithm is solved in 2<sup>n</sup> time, it might be better solved using DP.
<a href="https://b.ck.page"><img src="https://skerritt.blog/content/images/2019/06/Copy-of-technologiclly-clairvoyant.png"></img></a>
* * *
<!--kg-card-end: hr-->
### Why Is Dynamic Programming Called Dynamic Programming?


[Richard Bellman](https://en.wikipedia.org/wiki/Richard_E._Bellman) invented DP in the 1950s. [Bellman named it Dynamic Programming](https://en.wikipedia.org/wiki/Dynamic_programming#History) because at the time, RAND (his employer) disliked mathematical research and didn't want to fund it. He named it Dynamic Programming to hide the fact he was really doing mathematical research.

Bellman explains the reasoning behind the term dynamic programming in his autobiography, Eye of the Hurricane: An Autobiography (1984, page 159). He explains:

> "I spent the Fall quarter (of 1950) at RAND. My first task was to find a name for multistage decision processes. An interesting question is, Where did the name, dynamic programming, come from? The 1950s were not good years for mathematical research. We had a very interesting gentleman in Washington named Wilson. He was Secretary of Defense, and he actually had a pathological fear and hatred of the word research. I’m not using the term lightly; I’m using it precisely. His face would suffuse, he would turn red, and he would get violent if people used the term research in his presence. You can imagine how he felt, then, about the term mathematical. The RAND Corporation was employed by the Air Force, and the Air Force had Wilson as its boss, essentially. Hence, I felt I had to do something to shield Wilson and the Air Force from the fact that I was really doing mathematics inside the RAND Corporation. What title, what name, could I choose? In the first place I was interested in planning, in decision making, in thinking. But planning, is not a good word for various reasons. I decided therefore to use the word “programming”. I wanted to get across the idea that this was dynamic, this was multistage, this was time-varying. I thought, let's kill two birds with one stone. Let's take a word that has an absolutely precise meaning, namely dynamic, in the classical physical sense. It also has a very interesting property as an adjective, and that is it's impossible to use the word dynamic in a pejorative sense. Try thinking of some combination that will possibly give it a pejorative meaning. It's impossible. Thus, I thought dynamic programming was a good name. It was something not even a Congressman could object to. So I used it as an umbrella for my activities."

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## What are Sub-Problems?
![cover photo image](https://skerritt.blog/content/images/2019/06/undraw_process_e90d.svg)

Sub-problems are smaller versions of the original problem. Let's see an example. With the equation below:

1 + 2 + 3 + 4

We can break this down to:

1 + 2

3 + 4

Once we solve these two smaller problems, we can add the solutions to these problems to find the solution to the overall problem.

Notice how this sub-problem breaks down the original problem into components that build up the solution. While this is a small example, it illustrates the beauty of DP well. If we expand the problem to adding 100's of numbers it becomes clearer as to why we need DP. Take this example:

6 + 5 + 3 + 3 + 2 + 4 + 6 + 5

We have 6 + 5 twice. We work out what 6 + 5 is the first time. When we see it the second time we think to ourselves:

> "Ah, 6 + 5. I've seen this before. It's 11!"

In DP we store the solution to the problem in memory so we do not need to recalculate it. By finding the solutions for every single sub-problem, we can tackle the original problem itself.

The act of storing a solution is called _memoisation_.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## What is Memoisation in Dynamic Programming?
![cover photo](https://skerritt.blog/content/images/2019/06/undraw_task_31wc.svg)

First, let's see why storing answers to solutions make sense. We're going to look at a famous divide and conquer problem, _[Fibonacci sequence](https://www.mathsisfun.com/numbers/fibonacci-sequence.html)_. [Divide and conquer](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga) is dynamic programming, but without storing the solution.

There are 3 main parts to [divide and conquer](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga):

1. **Divide** the problem into smaller sub-problems of the same type.
2. **Conquer** - solve the sub-problems recursively.
3. **Combine** - Combine all the sub-problems to create a solution to the original problem.

Dynamic programming has one extra step added to step 2. This is memoisation.

The Fibonacci sequence is a sequence of numbers. It's the last number + the current number. We start at 1.

1 + 0 = 1

1 + 1 = 2

2 + 1 = 3

3 + 2 = 5

5 + 3 = 8

In Python, this is common programmed as:

<!--kg-card-begin: code-->
```python
def F(n):
  if n == 0 or n == 1:
    return n
  else:
return F(n-1)+F(n-2)

```
<!--kg-card-end: code-->

If you're not familiar with recursion I have a [blog post written for you that you should read first.](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga)

Let's calculate F(4). In an execution tree, this looks like:

<!--kg-card-begin: markdown-->

![What Is Dynamic Programming With Python Examples](https://skerritt.blog/content/images/2019/06/image-8.png)

<!--kg-card-end: markdown-->

We calculate F(2) twice. This may be a small example, but on bigger inputs (such as F(10)) the repetition builds up. The purpose of dynamic programming is to not calculate the same thing twice.

Instead of calculating F(2) twice, we store the solution somewhere and only calculate it once.

We'll store the solution in an array. F(2) = 1. Our array will then look like memo[2] = 1. Below is some Python code to calculate the Fibbonacci sequence using DP.

```python
def fibonacciVal(n):
	memo[0], memo[1] = 0, 1
	for i in range(2, n+1):
		memo[i] = memo[i-1] + memo[i-2]
	return memo[n]
```

The examples set out here are in Python. I’ll do my best to keep the code agnostic. Meaning that if you want to program this in Java, it shouldn’t be too hard to convert it over.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## How to Identify Dynamic Programming Problems
![cover photo image](https://skerritt.blog/content/images/2019/06/undraw_file_searching_duff.svg)
In theory, every problem can be solved dynamically. The question is then:

> "When should I solve this problem with dynamic programming?"

We should use dynamic programming for problems that are on the border between _tractable_ problems and _intracable_ problems.

_[Tractable problems](https://www.britannica.com/technology/tractable-problem)_ are those that can be solved in polynomial time. That's just a fancy way of saying we can solve it in a fast manner. Binary search and sorting,are all fast. _[Intractable problems](https://www.umsl.edu/~siegelj/information_theory/classassignments/Lombardo/04_intractableproblems.html)_ are those that run in exponential time. They're slow. Generally speaking, intractable problems are those that can only be solved by bruteforcing through every single combination ([NP hard](https://www.youtube.com/watch?v=YX40hbAHx3s)).

When we see terms such as:

> "shortest/longest, minimized/maximized, least/most, fewest/greatest, biggest/smallest"

We know it's an optimisation problem.

Another cool thing with DP algorithms is that their proof of correctness is usually self-evident. Other algorithmic strategies are often much harder to prove correct, and therefore more error-prone.

For instance, greedy algorithms may seem conceptually simpler, and usually, run faster, but they’re much harder to prove correct because they require making a lot of implicit assumptions about the structure of the input.  
  
When we see these kinds of terms, the problem may ask for a specific number ( "find the minimum number of edit operations") or it may ask for a result ( "find the longest common subsequence"). The latter type of problem is harder to recognize as a dynamic programming problem. If something sounds like optimisation, it could be solved by DP.  
  
Now, imagine we've found a problem that's an optimisation problem, but we're not sure if it can be solved with DP. First, identify what we're optimising for. Once we realize what we're optimising for, we have to decide how easy it is to perform that optimisation. Sometimes, the [greedy approach](https://brilliant.org/wiki/greedy-algorithm/) is sufficient for an optimal solution.   
  
Dynamic programming takes the brute force approach. It Identifies repeated work, and eliminates the repetition.

Before we even start to formulate the problem as a dynamic programming problem, we think about what the brute force solution might look like. Could there possibly be repeated substeps in the brute force solution? If so, we try to formulate the problem as a dynamic programming problem.

Mastering dynamic programming is all about understanding the problem. List all the inputs that can affect the answers. Once we've identified all the inputs and outputs, try to identify whether the problem can be broken into subproblems. If we can identify subproblems, we can probably use DP.   
  
List all inputs that affect the answer, and worry about reducing the size of that set later.  Once we have identified the inputs and outputs, we try to identify whether the problem can be broken into smaller subproblems. If we can identify smaller subproblems, then we can probably apply dynamic programming to solve the problem. Then, figure out what the recurrence is and solve it. When we're trying to figure out the recurrence, remember that whatever recurrence we write has to help us find the answer. Sometimes the answer will be the result of the recurrence, and sometimes we will have to obtain the result by looking  at a few results from the recurrence  
  
Just because a problem can be solved with dynamic programming does not mean there isn't a more efficient solution out there. Solving a problem with dynamic programming feels like magic, but remember that dynamic programming is merely a clever brute force.  Sometimes it pays off well,  and sometimes it helps only a little.
![flowchart](https://skerritt.blog/content/images/2019/06/dynamic-Programming-Process.png)

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## How to Solve Problems using Dynamic Programming
![img](https://skerritt.blog/content/images/2019/06/undraw_problem_solving_ft81.svg)

Now we have an understanding of what dynamic programming is and how it generally works, let's look at how we'll create a dynamic programming solution to a problem. We're going to explore the process of dynamic programming using the _[Weighted Interval Scheduling Problem](https://courses.cs.washington.edu/courses/cse521/13wi/slides/06dp-sched.pdf)_.

Pretend you're the owner of a dry cleaner. You have _n_ customers come in and give you clothes to clean. You can only clean one customers pile of clothes (PoC) at a time. Each pile of clothes, _i_, must be cleaned at some pre-determined start time s_i and some predetermined finish time f\_i.

Each pile of clothes has an assiocated value, v\_i, based on how important it is to your business. For example, some customers may pay more to have their clothes cleaned faster. Or some may be repeating customers and you want them to be happy.

As the owner of this dry cleaners you must determine the optimal schedule of clothes that maximises the total value of this day. This problem is a re-wording of the _Weighted Interval scheduling problem_.

You will now see 4 steps to solving a DP problem. Sometimes, you can skip a step. Sometimes, your problem is already well defined and you don't need to worry about the first few steps.

## Step 1. Write the Problem out
![img](https://skerritt.blog/content/images/2019/06/undraw_typewriter_i8xd.svg)

Grab a piece of paper. Write out the problem. Specifically:

- What is the problem?
- What are the subproblems?
- What would the solution roughly look like?

In the dry cleaner problem, let's put down into words the subproblems. What we want to determine is the maximum value schedule for each pile of clothes such that the clothes are sorted by start time.

Why sorted by start time? Good question! We want to keep track of processes which are currently running. If we sort by finish time, it doesn't make much sense in our heads. We could have 2 with similar finish times, but entirely different start times. Time moves in a linear fashion. If we have piles of clothes that start at 1 pm, we know that. If we have a pile of clothes that finishes at 3 pm, it's kind of seeing it backwards. Doesn't make so much sense.

We can find the maximum value schedule for piles n-1 through to n. And then for n - 2 through to n. And so on. By finding the solution to every single sub-problem, we can tackle the original problem itself. The maximum value schedule for piles 1 through _n_. Since the sub-problems are smaller versions of the original problem, sub-problems can be used to solve the original problem.

With the interval scheduling problem, the only way we can solve it is by brute-forcing all subsets of the problem until we find an optimal one. What we're saying is that instead of brute-forcing one by one, we divide it up. We brute force from n-1 through to n. Then we do the same for n-2 through to n. Eventually, we have loads of smaller problems, which we can solve dynamically. We want to build the solutions to our sub-problems such that each sub-problem builds on the previous problems.

## 2. Mathematical Recurrences
![cover photo](https://skerritt.blog/content/images/2019/06/undraw_mathematics_4otb.svg)
I know, mathematics sucks. If you'll bare with me here you'll find that this isn't that hard. [Mathematical recurrences](https://en.wikipedia.org/wiki/Recurrence_relation) are used to:

> Define the running time of a divide and conquer (dynamic programming) technique

But, between you and me, they can also be used to define a problem. If it's difficult to turn your subproblems into maths, then it may be the wrong subproblem.

There are 2 steps to creating a mathematicla recurrence:

### 1: Define the Base Case

Base cases are the smallest possible denomination of a problem.

When creating a recurrence, ask yourself these questions:

> "What decision do I make at step 0?"

It doesn't have to be 0. The base case is the smallest possible denomination of a problem. We saw this with the Fibonacci sequence. The base was was:

- If n == 0 or n == 1, return 1

It's important to know where the base case lies, so we can create the recurrence. In our problem, we have one decision to make:

- Put that pile of clothes on to be washed

or

- Don’t wash that pile of clothes today

If n is 0, that is, if we have 0 PoC then we do nothing. Our base case is then:

> if n == 0, return 0

### 2: What Decision Do I Make at Step n?

Now we know what the base case is, if we're at step n what do we do? For each pile of clothes that is compatible with the schedule so far (compatible means that the start time is after the finish time of the pile of clothes currently being washed), the algorithm chooses two options.

Now we know what happens at the base case, and what happens else. We now need to find out what information the algorithm needs to go backwards (or forwards).

> "If my algorithm is at step i, what information would it need to decide what to do in step i+1?"

To decide between the two options, the algorithm needs to know the next compatible PoC (pile of clothes). The next compatible PoC for a given pile, p, is the PoC, n, such that s\_n (the start time for PoC n) happens after f\_p (the finish time for PoC p). The difference between s\_n and f\_p should be minimised.

In English, imagine we have one washing machine. We put in a pile of clothes at 13:00. Our next pile of clothes starts at 13:01. We can't just open the washing machine and put them in. Our next compatible pile of clothes is the one that starts after the finish time of the one currently being washed.

> "If my algorithm is at step i, what information did it need to decide what to do in step i-1?"

The algorithm needs to know about future decisions. The ones made for PoC _i_ through _n_ in order to decide to run or not to run PoC _i-1_.

Now that we’ve answered these questions, perhaps we’ve started to form a  recurring mathematical decision in our mind. If not, that’s also okay,  it becomes easier to write recurrences as we get exposed to more problems.

Here’s our recurrence:

![If you're reading this on Dev using a screenreader, it's much better for you to visit my blog https://skerritt.blog/dynamic-programming/ . There's a large amount of maths in this document that are images. On my blog, I'm using MathJax which is accessibility friendly.](https://skerritt.blog/content/images/2019/06/Screenshot_2019-06-23-What-Is-Dynamic-Programming-With-Python-Examples.png)

Let's explore in detail what makes this mathematical recurrence. OPT(i) represents the maximum value schedule for PoC _i_ through to _n_ such that PoC are sorted by start times. OPT(i) is our subproblem from earlier.

We start with the base case. All recurrences need somewhere to stop. If we call OPT(0) we'll be returned with 0.

To determine the value of OPT(i), we consider two options. We want to take the maximum of these options to meet our goal. Our goal is the _maximum value schedule_ for all piles of clothes. Once we choose the option that gives the maximum result at step _i,_ we memoize its value as OPT(i).

The two options — to run or not to run PoC i — are represented mathematically as follows:

v\_i + OPT(next[n])

This represents the decision to run PoC i. It adds the value gained from PoC i to OPT(next[n]), where next[n] represents the next compatible pile of clothing following PoC i. When we add these two values together, we get the maximum value schedule from i through to n such that they are sorted by start time if i is ran.

Sorted by start time here because next[n] is the one immediately after v\_i, so by default, they are sorted by start time.

OPT(i + 1)

If we decide not to run _i_, our value is then OPT(i + 1). The value is not gained. OPT(i + 1) gives the maximum value schedule for i+1 through to n, such that they are sorted by start times.

## 3. Determine the Dimensions of the Memoization Array and the Direction in Which It Should Be Filled

The solution to our DP problem is OPT(1). We can write out the solution as the maximum value schedule for PoC 1 through n such that PoC are sorted by start time. This goes hand in hand with "maximum value schedule for PoC i through to n". Our solution can be written as OPT(1).

From step 2:

![img](https://skerritt.blog/content/images/2019/06/image-13.png)

Going back to our Fibonacci numbers earlier, our DP solution relied on the fact that the Fibonacci numbers for 0 through to n - 1 were already memoised. That is, to find F(5) we already memoised F(0), F(1), F(2), F(3), F(4). We want to do the same thing here.

The problem we have is figuring out how to fill out a memoisation table. In the scheduling problem, we know that OPT(1) relies on the solutions to OPT(2) and OPT(next[1]). PoC 2 and next[1] have start times after PoC 1 due to sorting. We need to fill our memoisation table from OPT(n) to OPT(1).

We can clearly see our array is one dimensional, from 1 to n. But, if we couldn't clearly see that we can work it out another way. The dimensions of the array are equal to the number and size of the variables on which OPT(x) relies. In our algorithm, we have OPT(i) - one variable, i. This means our array will be 1-dimensional and its size will be n, as there are n piles of clothes.

If we know that _n_ = 5, then our memoization array might look like this:

memo = [0, OPT(1), OPT(2), OPT(3), OPT(4), OPT(5)]

0 is also the base case. memo[0] = 0, per our recurrence from earlier.

## 4. Coding Our Solution

Personally, when I am coding a Dynamic Programming solution, I like to read the recurrence and try to recreate it. Our first step is to initialise the array to size (n + 1). In Python, we don't need to do this. But you may need to do it if you're using a different language.

Our second step is to set the base case.

To find the profit with the inclusion of job[i]. we need to find the latest job that doesn’t conflict with job[i].  The idea is to use Binary Search to find the latest non-conflicting job. I've copied the code from [here](https://www.geeksforgeeks.org/weighted-job-scheduling-log-n-time/)but edited slightly.

First, let's define what a "job" is. As we saw, a job consists of 3 things:

<!--kg-card-begin: code-->
```python
# Class to represent a job 
class Job: 
	def __init__ (self, start, finish, profit): 
		self.start = start 
		self.finish = finish 
		self.profit = profit 
```
<!--kg-card-end: code-->

Start time, finish time, and the total profit (benefit) of running that job.

The next step we want to program is the schedule.

<!--kg-card-begin: code-->
```python
# The main function that returns the maximum possible 
# profit from given array of jobs
def schedule(job): 
	# Sort jobs according to start time 
	job = sorted(job, key = lambda j: j.start) 

	# Create an array to store solutions of subproblems. table[i] 
	# stores the profit for jobs till arr[i] (including arr[i]) 
	n = len(job) 
	table = [0 for _ in range(n)] 

	table[0] = job[0].profit;
```
<!--kg-card-end: code-->

Earlier, we learnt that the table is 1 dimensional. We sort the jobs by start time, create this empty table and set table[0] to be the profit of job[0]. Since we've sorted by start times, the first compatiable job is always job[0].

Our next step is to fill in the entries using the recurrence we learnt earlier. To find the next compatiable job, we're using Binary Search. In the full code posted later, it'll include this. For now, let's worry about understanding the algorithm.

If the next compatiable job returns -1, that means that all jobs before index, i, conflict with it (so cannot be used).  Inclprof means we're including that item in the maximum value set. We then store it in table[i], so we can use this calculation again later.

<!--kg-card-begin: code-->
```python
	# Fill entries in table[] using recursive property 
	for i in range(1, n): 

		# Find profit including the current job 
		inclProf = job[i].profit 
		l = binarySearch(job, i) 
		if (l != -1): 
			inclProf += table[l]; 

		# Store maximum of including and excluding 
		table[i] = max(inclProf, table[i - 1]) 
```
<!--kg-card-end: code-->

Our final step is then to return the profit of all items up to n-1.

<!--kg-card-begin: code-->
```python
	return table[n-1] 
```
<!--kg-card-end: code-->

The full code can be seen below:

<!--kg-card-begin: code-->
```python
# Python program for weighted job scheduling using Dynamic 
# Programming and Binary Search 

# Class to represent a job 
class Job: 
	def __init__ (self, start, finish, profit): 
		self.start = start 
		self.finish = finish 
		self.profit = profit 

# A Binary Search based function to find the latest job 
# (before current job) that doesn't conflict with current 
# job. "index" is index of the current job. This function 
# returns -1 if all jobs before index conflict with it. 
def binarySearch(job, start_index): 
	# https://en.wikipedia.org/wiki/Binary_search_algorithm

	# Initialize 'lo' and 'hi' for Binary Search 
	lo = 0
	hi = start_index - 1

	# Perform binary Search iteratively 
	while lo <= hi: 
		mid = (lo + hi) // 2
		if job[mid].finish <= job[start_index].start: 
			if job[mid + 1].finish <= job[start_index].start: 
				lo = mid + 1
			else: 
				return mid 
		else: 
			hi = mid - 1
	return -1

# The main function that returns the maximum possible 
# profit from given array of jobs 
def schedule(job): 
	# Sort jobs according to start time 
	job = sorted(job, key = lambda j: j.start) 

	# Create an array to store solutions of subproblems. table[i] 
	# stores the profit for jobs till arr[i] (including arr[i]) 
	n = len(job) 
	table = [0 for _ in range(n)] 

	table[0] = job[0].profit; 

	# Fill entries in table[] using recursive property 
	for i in range(1, n): 

		# Find profit including the current job 
		inclProf = job[i].profit 
		l = binarySearch(job, i) 
		if (l != -1): 
			inclProf += table[l]; 

		# Store maximum of including and excluding 
		table[i] = max(inclProf, table[i - 1]) 

	return table[n-1] 

# Driver code to test above function 
job = [Job(1, 2, 50), Job(3, 5, 20), 
	Job(6, 19, 100), Job(2, 100, 200)] 
print("Optimal profit is"), 
print(schedule(job))

```
<!--kg-card-end: code-->

Congrats! 🥳 We've just written our first dynamic program!  Now that we’ve wet our feet,  lets walk through a different type of dynamic programming problem.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
# Knapsack Problem

Imagine you are a criminal. Dastardly smart. You break into Bill Gate’s mansion. Wow, okay!?!? How many rooms is this? His washing machine room is larger than my entire house??? Ok, time to stop getting distracted. You brought a small bag with you. A knapsack - if you will.

You can only fit so much into it. Let’s give this an arbitrary number. The bag will support weight 15, but no more. What we want to do is maximise how much money we'll make, b.

The greedy approach is to pick the item with the highest value which can fit into the bag. Let's try that. We're going to steal Bill Gate's TV. £4000? Nice. But his TV weighs 15. So... We leave with £4000.

Bill Gate's has a lot of watches. Let's say he has 2 watches. Each watch weighs 5 and each one is worth £2250. When we steal both, we get £4500 with a weight of 10.

In the greedy approach, we wouldn't choose these watches first. But to us as humans, it makes sense to go for smaller items which have higher values. The Greedy approach cannot optimally solve the {0,1} Knapsack problem. The {0, 1} means we either take the item whole {1} or we don't {0}. Dynamic programming can however optimally solve the {0, 1} knapsack problem.

The simple solution to this problem is to consider all the subsets of all items. For every single combination of Bill Gate's stuff, we calculate the total weight and value of this combination.

We consider only those with weight less than W\_max. We then pick the combination which has the highest value. This is a disaster! How long would this take? Bill Gate's would come back home far before you're even 1/3rd of the way there! In Big O, this algorithm takes O(n^2) time.

You can see we already have a rough idea of the solution and what the problem is, without having to write it down in maths!

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
## Maths

Imagine we had a listing of every single thing in Bill Gate's house. Maybe we stole it from some insurance papers. Now, think about the future. What is the optimal solution to this problem?

We have a subset, L, which is the optimal solution. L is a subset of S, the set containing all of Bill Gate's stuff.

Let's pick a random item, N. L either contains N or it doesn't. If it doesn't use N, the optimal solution for the problem is the same as {1, 2, ..., N-1}. This is assuming that Bill Gate's stuff is sorted by value / weight.

Suppose that the optimum of the original problem is not optimum of the sub-problem. if we have sub-optimum of the smaller problem then we have a contradiction - we should have an optimum of the whole problem.

If L contains N, then the optimal solution for the problem is the same as {1, 2, 3, ..., N-1}. We know the item is in, so L already contains N. To complete the computation we focus on the remaining items. We find the optimal solution to the remaining items.

But, we now have a new maximum allowed weight of W\_max - W\_n. If item N is contained in the solution, the total weight is now the max weight take away item N (which is already in the knapsack).

These are the 2 cases. Either item N is in the optimal solution or it isn't.

If the weight of item N is greater than W\_max, then it cannot be included so case 1 is the only possibility.

To more precisely define this recursive solution, let S\_k = {1, 2, ..., k} and S\_0 = ∅

Let B[k, w] be the _maximum total benefit_ obtained using a subset of S\_k. Having total weight at most w.

Then we define B[0, w] = 0 for each w \le W\_max, and:

Our desired solution is then B[n, W\_max].

![img](https://skerritt.blog/content/images/2019/06/image-14.png)

### Tabulation of Knapsack Problem

Okay, pull out some pen and paper. No, really. Things are about to get confusing real fast. This memoisation table is 2-dimensional. We have these items:

<!--kg-card-begin: code-->
```
(1, 1), (3, 4), (4, 5), (5, 7)
```
<!--kg-card-end: code-->

Where the tuples are `(weight, value)`.

We have 2 variables, so our array is 2-dimensional. The first dimension is from 0 to 7. Our second dimension is the values.

And we want a weight of 7 with maximum benefit.

<!--kg-card-begin: html-->

![img](https://skerritt.blog/content/images/2019/06/image-15.png)

<!--kg-card-end: html-->

The weight is 7. We start counting at 0 (not a DP thing, just a programming thing). We put each tuple on the left-hand side. Ok. Now to fill out the table!

<!--kg-card-begin: html-->

![skerritt.blog](https://skerritt.blog/content/images/2019/06/image-16.png)

<!--kg-card-end: html-->

The columns are weight. At weight 0, we have a total weight of 0. At weight 1, we have a total weight of 1. Obvious, I know. But this is an important distinction to make which will be useful later on.

When our weight is 0, we can't carry anything no matter what. The total weight of everything at 0 is 0.

<!--kg-card-begin: html-->

![skerritt.blog](https://skerritt.blog/content/images/2019/06/image-17.png)

<!--kg-card-end: html-->

If our total weight is 1, the best item we can take is (1, 1). As we go down through this array, we can take more items. At the row for (4, 3) we can either take (1, 1) or (4, 3). But for now, we can only take (1, 1). Our maximum benefit for this row then is 1.

<!--kg-card-begin: html-->

![skerritt.blog](https://skerritt.blog/content/images/2019/06/image-18.png)

<!--kg-card-end: html-->

If our total weight is 2, the best we can do is 1. We only have 1 of each item. We cannot duplicate items. So no matter where we are in row 1, the absolute best we can do is (1, 1).

Let's start using (4, 3) now. If the total weight is 1, but the weight of (4, 3) is 3 we cannot take the item yet until we have a weight of at least 3.

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-19.png)

<!--kg-card-end: html-->

Now we have a weight of 3. Let's compare some things. We want to take the max of:

MAX(4 + T[0][0], 1)

If we're at 2, 3 we can either take the value from the last row or use the item on that row. We go up one row and count back 3 (since the weight of this item is 3).

Actually, the formula is whatever weight is remaining when we minus the weight of the item on that row. The weight of (4, 3) is 3 and we're at weight 3. 3 - 3 = 0. Therefore, we're at T[0][0]. T[previous row's number][current total weight - item weight].

MAX(4 + T[0][0], 1)

The 1 is because of the previous item. The max here is 4.

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-20.png)

<!--kg-card-end: html-->

max(4 + t[0][1], 1)

Total weight is 4, item weight is 3. 4 - 3 = 1. Previous row is 0. t[0][1].

<!--kg-card-begin: html-->
![](https://skerritt.blog/content/images/2019/06/image-21.png)

<!--kg-card-end: html-->

I won't bore you with the rest of this row, as nothing exciting happens. We have 2 items. And we've used both of them to make 5. Since there are no new items, the maximum value is 5.

<!--kg-card-begin: html-->
![](https://skerritt.blog/content/images/2019/06/image-22.png)

<!--kg-card-end: html-->

Onto our next row:

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-23.png)

<!--kg-card-end: html-->

Here's a little secret. Our tuples are ordered by weight! That means that we can fill in the previous rows of data up to the next weight point. We know that 4 is already the maximum, so we can just fill it in. This is where memoisation comes into play! We already have the data, why bother re-calculating it?

We go up one row and head 4 steps back. That gives us:

max(4 + T[2][0], 5).

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-24.png)

<!--kg-card-end: html-->

Now we calculate it for total weight 5.

max(5 + T[2][1], 5) = 6

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-25.png)

<!--kg-card-end: html-->

We just do the same thing again:

max(5 + T[2][2], 5) = 6

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-26.png)
<!--kg-card-end: html-->

Now we have total weight 7. We choose the max of:

max(5 + T[2][3], 5) = max(5 + 4, 5) = 9

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-27.png)

<!--kg-card-end: html-->

If we had total weight 7 and we had the 3 items (1, 1), (4, 3), (5, 4) the best we can do is 9.

Since our new item starts at weight 5, we can just copy from the previous row until we get to weight 5.

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-28.png)

<!--kg-card-end: html-->

We then do another max.

Total weight - new item's weight. This is 5 - 5 = 0. We want previous row at position 0.

max(7 + T[3][0], 6)

The 6 comes from the best on the previous row for that total weight.

max(7 + 0, 6) = 7

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-29.png)

<!--kg-card-end: html-->

max(7 + T[3][1], 6) = 8

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-30.png)

<!--kg-card-end: html-->

max(7+T[3][2], 9) = 9

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-31.png)

<!--kg-card-end: html-->

9 is the maximum value we can get by picking items from the set of items such that the total weight is \le 7.

### Finding the Optimal Set for {0, 1} Knapsack Problem Using Dynamic Programming

Now, what items do we actually pick for the optimal set? We start from this item:

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-32.png)

<!--kg-card-end: html-->

We want to know where the 9 comes from. It's coming from the top because the number directly above 9 on the 4th row is 9. Since it's coming from the top, the item (7, 5) is not used in the optimal set.

Where does this 9 come from?

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-33.png)

<!--kg-card-end: html-->

This 9 is not coming from the row above it. **Item (5, 4) must be in the optimal set.**

We now go up one row, and go back 4 steps. 4 steps because the item, (5, 4), has weight 4.

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-34.png)

<!--kg-card-end: html-->

4 does not come from the row above. The item (4, 3) must be in the optimal set.

The weight of item (4, 3) is 3. We go up and we go back 3 steps and reach:

<!--kg-card-begin: html-->

![](https://skerritt.blog/content/images/2019/06/image-35.png)

<!--kg-card-end: html-->

As soon as we reach a point where the weight is 0, we're done. Our two selected items are (5, 4) and (4, 3). The total weight is 7 and our total benefit is 9. We just add the two tuples together to find this out.

Let's begin coding this.

<!--kg-card-begin: hr-->
* * *
<!--kg-card-end: hr-->
### Coding It

Now we kn0w how it works, and we've derived the recurrence for it - it shouldn't be too hard to code it. If our two-dimensional array is i (row) and j (column) then we have:

<!--kg-card-begin: code-->
```python
if j < wt[i]:
```
<!--kg-card-end: code-->

If our weight j is less than the weight of item i (i does not contribute to j) then:

<!--kg-card-begin: code-->
```python
if j < wt[i]:
	T[i][j] = T[i - 1][j]
else:
	// weight of i >= j
    T[i][j] = max(val[i] + t[i - 1][j-wt(i), t[i-1][j])
    // previous row, subtracting the weight of the item from the total weight or without including ths item
```
<!--kg-card-end: code-->

This is what the core heart of the program does. I've copied some code from [here](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)to help explain this. I'm not going to explain this code much, as there isn't much more to it than what I've already explained. If you're confused by it, leave a comment (can be left anonymously) below or email me 😁

<!--kg-card-begin: code-->
```python
# Returns the maximum value that can be put in a knapsack of 
# capacity W 
def knapSack(W , wt , val , n): 
  
    # Base Case 
    if n == 0 or W == 0: 
        return 0
  
    # If weight of the nth item is more than Knapsack of capacity 
    # W, then this item cannot be included in the optimal solution 
    if (wt[n-1] > W): 
        return knapSack(W , wt , val , n-1) 
  
    # return the maximum of two cases: 
    # (1) nth item included 
    # (2) not included 
    else: 
        return max(val[n-1] + knapSack(W-wt[n-1] , wt , val , n-1), 
                   knapSack(W , wt , val , n-1)) 
  
  
# To test above function 
val = [60, 100, 120] 
wt = [10, 20, 30] 
W = 50
n = len(val) 
print(knapSack(W , wt , val , n))
# output 220
```
* * *
<!--kg-card-end: hr-->
## Time Complexity of a Dynamic Programming Problem

In DP, [time complexity](https://skerritt.blog/you-need-to-understand-big-o-notation-now/) is calculated as:

Number of unique states \* time taken per state

For our original problem, the Weighted Interval Scheduling Problem, we had n piles of clothes. Each pile of clothes is solved in constant time. The time complexity is:

O(n) + O(1) = O(n)

[I've written a post about Big O notation](https://skerritt.blog/you-need-to-understand-big-o-notation-now/)if you want to learn more about time complexities.

With our Knapsack problem, we had n number of items. The table grows depending on the total capacity of the knapsack, our time complexity is:

O(nw)

Where n is the number of items, and w is the capactity of the knapsack.

I'm going to let you in on a little secret. It's possible to work out the time complexity of an algorithm from its recurrence. You can use something called the Master Theorem to work it out. This is the theorem in a nutshell:

<!--kg-card-begin: image-->

![What Is Dynamic Programming With Python Examples](https://skerritt.blog/content/images/2019/05/image-16.png)<figcaption>Taken from <a href="https://medium.com/@randerson112358/master-theorem-909f52d4364">here</a></figcaption>

<!--kg-card-end: image-->

Now, I'll be honest. The master therom deserves a blog post of its own. For now, I've found this video to be excellent:

{% youtube OynWkEj0S-s %}
* * *
<!--kg-card-end: hr-->
### Dynamic Programming vs Divide & Conquer vs Greedy

Dynamic Programming & [Divide and Conquer](https://dev.to/brandonskerritt/a-gentle-introduction-to-divide-and-conquer-algorithms-1ga) are incredibly similar. Dynamic Programming is based on Divide and Conquer, except we memoise the results.

Greedy, on the other hand, is different. It aims to optimise by making the best choice at that moment. Sometimes, this doesn't optimse for the whole problem. Take this question as an example. We have 3 coins:

1p, 15p, 25p

[And someones wants us to give change of 30p](https://en.wikipedia.org/wiki/Change-making_problem). With Greedy, it would select 25, then 5 \* 1 for a total of 6 coins. The optimal solution is 2 \* 15. Greedy works from largest to smallest. At the point where it was at 25, the best choice would be to pick 25.

<!--kg-card-begin: html-->



![What Is Dynamic Programming With Python Examples](https://skerritt.blog/content/images/2019/05/image-15.png)

* * *
<!--kg-card-end: hr-->
## Tabulation (Bottom-Up) vs Memoisation (Top-Down)

There are 2 types of dynamic programming. Tabulation and Memoisation.

### Memoisation (Top-Down)

We've computed all the subproblems but have no idea what the optimal evaluation order is. We would then perform a recursive call from the root, and hope we get close to the optimal solution or obtain a proof that we will arrive at the optimal solution. Memoisation ensures you never recompute a subproblem because we cache the results, thus duplicate sub-trees are not recomputed.

<!--kg-card-begin: image-->

![What Is Dynamic Programming With Python Examples](https://skerritt.blog/content/images/2019/05/image-8.png)

<!--kg-card-end: image-->

From our Fibonacci sequence earlier, we start at the root node. The subtree F(2) isn't calculated twice.

This starts at the top of the tree and evaluates the subproblems from the leaves/subtrees back up towards the root. **Memoisation is a top-down approach.**

### Tabulation (Bottom-Up)

We've also seen Dynamic Programming being used as a 'table-filling' algorithm. Usually, this table is multidimensional. This is like memoisation, but with one major difference. We have to pick the exact order in which we will do our computations. The knapsack problem we saw, we filled in the table from left to right - top to bottom. We knew the exact order of which to fill the table.

Sometimes the 'table' is not like the tables we've seen. It can be a more complicated structure such as trees. Or specific to the problem domain, such as cities within flying distance on a map.

### Tabulation & Memosation - Advantages and Disadvantages

Generally speaking, memoisation is easier to code than tabulation. Wecan write a 'memoriser' wrapper function that automatically does it for we. With tabulation, we have to come up with an ordering.

Memoisation has memory concerns. If we're computing something large such as F(10^8), each computation will be delayed as we have to place them into the array. And the array will grow in size very quickly.

Either approach may not be time-optimal if the order we happen (or try to) visit subproblems is not optimal, specifically if there is more than one way to calculate a subproblem (normally caching would resolve this, but it's theoretically possible that caching might not in some exotic cases). Memoization will usually add on our time-complexity to our space-complexity (e.g. with tabulation we have more liberty to throw away calculations, like using tabulation with Fib lets us use O(1) space, but memoization with Fib uses O(N) stack space).

<!--kg-card-begin: html-->

![What Is Dynamic Programming With Python Examples](https://skerritt.blog/content/images/2019/05/image-17.png)

<!--kg-card-end: image-->
## Conclusion

Most of the problems you'll encounter within Dynamic Programmg already exist in one shape or another. Often, your problem will build on from the answers for previous problems. [Here's a list of common problems that use Dynamic Programming.](https://en.wikipedia.org/wiki/Dynamic_programming#Algorithms_that_use_dynamic_programming)

I hope that whenever you encounter a problem, you think to yourself "can this problem be solved with DP?" and try it.