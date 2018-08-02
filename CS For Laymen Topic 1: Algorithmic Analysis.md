About this series: This nontechnical tutorial series is intended for people who have no mathematical maturity, which is why it contains no mathematical definitions or proofs. If you have mathematical maturity I suggest reading TAOCP instead. The idea here is to focus on gaining intuitive understanding. Once you have gained the intuition you can seek out the proofs later on. I think this way is better for beginners who have a tendency to "miss the forest for the trees" i.e get stuck on the details of proofs. 

- [Introduction](#introduction)
- [A word of thanks](#a-word-of-thanks)
- [What is algorithmic analysis?](#what-is-algorithmic-analysis)
- [Motivation](#motivation)
- [The RAM model of computation](#the-ram-model-of-computation)
- [Big O Notation](#big-o-notation)
    - [Exercises 0](#exercises-0)
- [Best case, average case, and worst case analysis](#best-case-average-case-and-worst-case-analysis)
    - [Exercises 1](#exercises-1)
- [Empirical "analysis"](#empirical-%22analysis%22)
- [Space complexity](#space-complexity)
- [Non-recursive algorithms](#non-recursive-algorithms)
    - [Exercises 2](#exercises-2)
- [Amortized runtime analysis](#amortized-runtime-analysis)
    - [Exercises 3](#exercises-3)
- [Lower bounds](#lower-bounds)
- [Recursive algorithms](#recursive-algorithms)
    - [Order of growth of geometric sums](#order-of-growth-of-geometric-sums)
    - [The master theorem](#the-master-theorem)
        - [Examples of how to use the master theorem](#examples-of-how-to-use-the-master-theorem)
            - [Binary search](#binary-search)
            - [Mergesort, maximum subarray, and FFT](#mergesort-maximum-subarray-and-fft)
            - [Fast heap construction](#fast-heap-construction)
            - [Stooge sort](#stooge-sort)
            - [Karatsuba multiplication](#karatsuba-multiplication)
            - [Strassen's multiplication](#strassens-multiplication)
            - [Max-heapify](#max-heapify)
            - [Examples from CLRS3](#examples-from-clrs3)
            - [CLRS Exercise 4.5-1](#clrs-exercise-45-1)
            - [More examples](#more-examples)
    - [[Not done yet]](#not-done-yet)


# Introduction

This is cobbled together from a whole bunch of sources. 

This tutorial only shows very basic applications of the methods introduced here. Later on, we will use some of the methods introduced here to analyze some common algorithms, but each algorithm's analysis will be in the tutorial that covers that algorithm and not here, so this tutorial is not comprehensive in any way shape or form, it's just a basic introduction to some methods and doesn't really even cover how they are used. 

# A word of thanks

The first thing they teach you in any decent CS 101 class is that just about every interesting property of computer programs is uncomputable due to the halting problem (if you can't even determine if a program will ever halt, clearly you can't compute its time complexity), thus guaranteeing [full employment](https://en.wikipedia.org/wiki/Full_employment_theorem) for computer scientists and mathematicians - thanks Turing!

# What is algorithmic analysis? 

Determining the cost (mainly time and space complexity) of algorithms. 

Algorithmic analysis is largely an art. As mentioned earlier there is no general method to determine the time and space complexity of an arbitrary program (because it's uncomputable). For example I could write a program to compute Collatz conjecture and you'd have to solve that conjecture in order to determine the program's time complexity. All we have is a handful of methods that apply to a very small class of programs. Most of the time we can't apply these methods and have to proceed on a case-by-case basis. See also: [full employment theorem](https://en.wikipedia.org/wiki/Full_employment_theorem). 

# Motivation

All this talk of asymptotic complexity (as if the inputs to our algorithms are really unbounded, even though we live in a finite universe) may seem like intellectual masturbation to you. The truth is, we don't really care about asymptotic complexity for its own sake (except some academics maybe), we care about real world performance, and asymptotic complexity just happens to capture real world performance often enough to be useful. 

In reality, constant factors matter. If the algorithm is mainly going to be running on small inputs - compare Karatsuba and Schönhage–Strassen, the latter has better asymptotic performance but due to high constant factors only runs faster once input size exceeds tens of thousands of digits (this is the so-called **crossover point**, where one algorithm becomes faster than another...there can be multiple crossover points). Ease of implementation also matters. So, asymptotic complexity is not the be-all and end-all in deciding which algorithm to use. It is just one concern out of many. 

# The RAM model of computation

There is a nice and concise description of the RAM model of computation in The Algorithm Design Manual by Skiena (page 43). In short, in the RAM model, basic operations (arithmetic, logic, jumps, memory access i.e read & write) are O(1). The "RA" in RAM stands for [random access](https://en.wikipedia.org/wiki/Random_access) meaning accessing any location in RAM takes O(1) time. This is very important as it is the reason that we can access any element in an array in constant time (as opposed to linear time for a linked list). Note however that in the real world, sequential access (e.g looping through an array) is typically faster than random access (e.g accessing each element in an array in random order) due to hardware optimizations (e.g [data prefetching](https://bitbashing.io/memory-performance.html)).

There are more complicated models that take into account things like cache hierarchy, which is important on real computers. Like all models, the RAM model is a simplification of reality, and you have to know when it is appropriate to use. For example, arithmetic operations are no longer constant time when dealing with really big numbers (e.g from fast growing sequences like Fibonacci or factorial). On the other hand, when comparing algorithms, it is rarely useful to include details from assuming that the basic arithmetic operations are not O(1), e.g if you say hash table lookup is O(log n) because hashing is O(log n) then you have to say binary tree lookup is O(log^2 n) because comparison would also be O(log n) - not enlightening, but it sure does make things more inconvenient.

# Big O Notation

Big O, Big Theta, and Big Omega are mathematical notations which describe the **asymptotic** growth rates of mathematical functions (they don't necessarily have anything to do with algorithms). Things like "the number of positions in chess" is constant, so an algorithm visiting every possible position in chess is O(1). 

As an aside: Big O notation has been in use since the 19th century but Don Knuth proposed it for use in computing in a 1976 ACM SIGACT newsletter ("Big Omicron and big Omega and big Theta").

Big O gives an asymptotic upper bound, Big Omega gives an asymptotic lower bound, and Big Theta is an asymptotically tight bound (the Big Theta of f is both a Big O and a Big Omega of f). Hence, each function can have infinitely many Big Os and Big Omegas but it can only have one (meaningful) Big Theta. Basically, you get the Big Theta of a function by dropping non-dominant terms (i.e everything except the most dominant i.e fastest growing term) and constant factors (do not drop non-constant factors). For precise definitions see CLRS3 page 45. There is also little o and little omega notation (which are rarely used) which are similar: whereas big O is like a >= relation, little o is like a > relation, i.e if f is o(g) then it means f grows strictly slower than g. So f(n) = n is O(n) but is NOT o(n), but it is o(n^2). And little omega is just the inverse. For precise definitions see CLRS3 page 50. 

Here's a brief list of terms in descending order of dominance, where c is a constant:
1. infinity
2. c^(c^n) (double exponential)
3. n^n 
4. n!
5. c^n (for c > 1, e.g 3^n dominates 2^n, which dominates 1.6^n)
6. n^c (n^2 dominates n, which dominates n^0.5 and so on)
7. log n
8. log log n
9. log* n (iterated log)
10. inverse ackerman's function
11. 1
12. c^n (for 0 < c < 1)

If you plot these on a graph then you can see the asymptotic dominance hierarchy visually. 

As an aside: These notations describe sets of functions. For example, O(n) is the set of mathematical functions whose growth is upper-bounded by n. We say that the function f(n) = 2n + 3 belongs to [the set] O(n). It also belongs to O(n^2), O(n^3), O(2^n), O(n!) and so on. Likewise f(n) belongs to Big Omega(sqrt(n)), Big Omega(log n), Big Omega(log log n) and so on. Instead of writing ∈ we sometimes use "=" or "is" instead. This is an abuse of notation but everyone knows what it means so it's ok.

As an aside: In fact, the thing inside the parentheses of the Big Oh is an anonymous function, so we should use lambda notation: i.e we should really say f ∈ O(λn.n^3) instead of f ∈ O(n^3). Nobody does that though, it's just inconvenient. 

Every algorithm's runtime in any case is Big O(infinity), since no algorithm can take more than an infinite number of steps, and Big Omega(0), since no algorithm can take fewer than 0 steps. When people in "industry" say Big O they usually mean Big Theta, so from now on, whenever I write O(n) it means Big Theta(n) unless explicitly noted otherwise. Also, I may make some off-by-one errors with my silly i = 1->n notation: these usually will not affect the Big Theta. 

We have the results (for the proofs see KT page 39): 
- Reflexivity, symmetry, and transpose symmetry. 
- Transitivity: if f is O(g) and g is O(h) then f is O(h). 
- Sums of functions: if f is O(h) and g is O(h) then f + g = O(h).
- if f is O(a) and g is O(b) and O(a) dominates O(b) then f+g is O(a).
- O(f) * O(g) = O(f*g)

Note that if there are multiple variables, e.g some algorithms take multiple parameters, then in the general case (unless you have special knowledge) you need to keep all the variables - you only drop the non-dominant terms for each variable. For example, f(A,B) = A^2 + A + B^2 + B is O(A^2 + B^2), but if you know that B is always smaller than A then you can simplify that to O(A^2). 

In practice we most frequently use the following rules (from wikipedia):

1. If f(x) is a sum of several terms, if there is one with largest growth rate, it can be kept, and all others omitted.
2. If f(x) is a product of several factors, any constants (terms in the product that do not depend on x) can be omitted.

For competitive programming tasks you are normally given the input range, and that will suggest the maximum time complexity of the algorithm [you can use (since the time limit is only a few seconds)](https://web.archive.org/web/20170224221329/https://www.topcoder.com/community/data-science/data-science-tutorials/computational-complexity-section-1/):
```
+------------+-------------+
| complexity |  maximum N  |
+------------+-------------+
| O(log N)   |       10^18 |
| O(N)       | 100 000 000 |
| O(N log N) |  40 000 000 |
| O(N^2)     |      10 000 |
| O(N^3)     |         500 |
| O(N^4)     |          90 |
| O(2^N)     |          20 |
| O(N!)      |          11 |
+------------+-------------+
```

## Exercises 0

Question: Do the functions f = log (n ^ (log 2)) and g = log (2 ^ (log n)) have the same asymptotic growth rate? 

Solution: Yes because log a^b = b log a. So f = log 2 log n and g = log n log 2. They're the same. 

Question: Arrange the following terms in order of growth: [source](https://www.geeksforgeeks.org/practice-set-recurrence-relations/)
```
f1(n) = n^√n
f2(n) = 2^n
f3(n) = (1.000001)^n
f4(n) = n^(10)*2^(n/2)
```
Solution: 
```
2^(n/2) = (2^0.5)^n ~= 1.414^n
So we have: f4 > f3
2^n = 2^(n/2)*2^(n/2)
So we have: f2 > f4
log (n^√n) = n^0.5 * log n
log (1.000001^n) = n log 1.000001 > n^0.5 * n^0.5 
So we have: f3 > f1
Putting it all together: f2 > f4 > f3 > f1
```
# Best case, average case, and worst case analysis

Observe that the runtime of an algorithm depends not only on the **size** of the input but also on the **content** of the input (e.g is it sorted or not). This is where the ideas of best case, average case, and worst case analysis come in handy. For example, quicksort takes O(n log n) in the best case and O(n^2) in the worst case (when the input is sorted). Compare with insertion sort which has best case O(n) (when the input is sorted). 

Suppose we have this function:
```python
def f(n):
    if n%2 run n^2 steps
    else run n^4 steps
```
Here the best case time complexity is O(n^2) and worst case is O(n^4). 

The ideas of Big O, Big Theta, and Big Omega are orthogonal to the ideas of best case, average case, and worst case analysis. Big O is used to describe the results of runtime analysis - i.e we say that insertion sort best case runtime is O(n) and worst case runtime is O(n^2). According to Cormen it would also be correct to just say that insertion sort is O(n^2) since that is true for both best and worst case. So if an algorithm has different best and worst case time complexities you can still use just one Big Oh to describe it (the asymptotic time complexity of the algorithm in the worst case) but you can't use one Big Theta to describe its running time since its Big Thetas would be different for best and worst case inputs. Thus when people say an algorithm is O(x) that usually means its the worst case Big Theta is x - otherwise they would have clarified whether they're talking about the best or worst or average case and use Big Theta notation instead. However you can't say something like: "O-notation gives us an upper limit of the execution time i.e. the execution time in the worst case" which is WRONG! O-notation just gives you the asymptotic growth rate of a function, whether or not it refers to worst case time complexity depends completely on context! 

Usually we care about average case (to get representative idea of the algorithm's performance) and worst case (e.g for safety critical systems, and when the distribution over the inputs may not be known). Best case is usually pretty useless since it's fairly easy to check if the input is best case and just return something trivial. Sometimes the worst case is not reflective of how the algorithm performs on a typical real life input, for example the simplex algorithm is exponential worst case but in practice is polynomial. 

You may perform an average case analysis when you know the distribution of the inputs, in that case the analysis is reflective of reality. However, even if you do not know the distribution of inputs, it may be possible to ensure that the average case analysis almost always applies, for example in randomized algorithms, e.g by randomly permuting the input array - in such a scheme we can minimize the chance (perhaps to negligible) that the randomizer generates a pathological input. For example randomized quicksort is still worst case O(n^2) although that does not describe how the algorithm performs in practice. 

As an aside: In computational complexity theory, exponential = slow/intractable and polynomial = fast/tractable (see theory of NP-completeness). The simplex algorithm shows the former is not true in practice. And many polynomial algorithms are too slow for practical use, for example people don't use the polynomial-time AKS primality test despite it being deterministic because it's too slow, they use the probabilistic tests such as Miller-Rabin instead. For a readable, non-technical, and enlightening discussion of this and related topics, see the 1987 Turing Award lecture by Tarjan (aptly titled "Algorithm Design"). 

## Exercises 1

From Algorithms Unlocked page 16:  
What's the time complexity of linear search with sentinel?  
```python
def sentinel_linear_search(A,x):
    n = len(A)
    last = A[n]
    A[n] = x
    i = 1
    while A[i] != x:
        i++
    A[n] = last
    if i < n or A[n] == x:
        return i
    return NOTFOUND
```
Solution: Although this is faster than a normal for loop (because in a for loop the check is done every iteration whereas this version avoids it), it's still O(n) in the worst case where there are still n comparisons. 

What's the time complexity of f?
```python
def f(n):
    for i = 0 -> n:
        for j = 0 -> i:
            do something in O(1)
```
Solution: The inner loop gets run n times, but each time the number of steps in the inner loop increases, from 0 to n. So the total number of steps is 0 + 1 + ... + n. The sum is (n^2 + n) / 2, which is O(n^2). You find the proof for sum of arithmetic series online. 

Example 3 from [Computational Complexity: Section 1 by misof](https://web.archive.org/web/20170224221329/https://www.topcoder.com/community/data-science/data-science-tutorials/computational-complexity-section-1/)  
Give the time complexity of the following algorithm:
```cpp
int j=0;
for (int i=0; i<N; i++) {
  while ( (j<N-1) && (A[i]-A[j] > D) )
    j++;
  if (A[i]-A[j] == D) return 1;
}
```
Solution: The outer loop executes N times. The while loop will, the first time it's run, increment j to N-1. After that, the value of j will be stuck on N-1 forever. So the next iteration of the outer for loop, the while loop will fail the condition check. Thus j is incremented exactly N-1 times. So overall the algorithm is O(N). 

Question 3.12h from Data Structures & Algorithm Analysis 3rd Edition by Clifford A. Shaffer 
What's the time complexity of f? Assume array A contains a random permutation of the values from 0 to n-1.
```python
def f(n):
    for i = 0->n:
        for (j = 0, A[j] != i, j++):
            do something in O(1)
```
Solution: The outer loop executes n times and the inner loop has a worst case of n steps (in case i happens to be the last element of the array), so an upper bound is O(n^2). Can we get any tighter (lower)? Imagine a permutation of 1, 2, 3, ..., n. On that input, the inner loop first tries to find 1, then 2, then 3...and finds it in exactly that many steps. So the number of steps of the inner loop is 1, 2, 3, ..., n. So the total number of steps is 1 + 2 + 3 + ... + n which we know is O(n^2). So n^2 is indeed a tight bound on the worst case running time. 

AHU87 (Data Structures & Algorithms Aho, Hopcraft, Ullman) Example 1.8:    
What is the best and worse case time complexity of f?
```python
def f(n):
    g(n)
    h(n)
def g(n):
    if n%2 run n^4 steps
    else run n^2 steps
def h(n)
    if n%2 run n^2 steps
    else run n^3 steps
```
Solution: There are 2 cases: n is even or odd. If n is even then g(n) runs n^4 steps and h(n) runs n^2 steps so it's O(n^4). If n is odd then g(n) runs n^2 times but h(n) runs n^3 steps so it's O(n^3). Therefore best case is O(n^3) and worst case is O(n^4). We say that the steps g(n) and h(n) in f(n) are **incommensurate**: neither is larger than the other, nor are they equal. 

Example from [Algorithms_Measuring_Algorithms_Perfromance_01.cpp By Eng Mostafa Saad](https://www.dropbox.com/s/mtacipbdxezvudp/Algorithms_Measuring_Algorithms_Perfromance_01.cpp):
```cpp
void what_is_my_tight_Order()	// ?
{
	for (int i = 0; i < n; ++i) {
		int cnt = 0;
		for (int j = i; j < n && arr[i] == arr[j]; ++j) {
			cnt++;
		}
		i = i + cnt - 1;
	}
}
```
Solution: Notice that on each iteration of the outer for loop, the number of times that the inner for loop ran is recorded in the cnt variable, which is then added to the value of i, causing it to "skip" exactly that many iterations. Thus the sum of the number of iterations of the inner and outer for loops is just N, so the function is overall O(N). 

# [Empirical "analysis"](http://cs.lmu.edu/~ray/notes/alganalysis/)

You can augment a theoretical runtime analysis with an empirical analysis by simply timing the algorithm on inputs of various sizes. Then compare the actual time to some known time complexities. Like this:
```python
for i = 0->1000000000:
    t = time f(i) 
    print t / i^2, t / i^3, t / log(i), ...
```
This prints out a bunch of ratios. If the ratio gets smaller as the input gets bigger then obviously the algorithm grows faster than that function, and vice versa. If the ratio appears to converge then it's probably the right function or at least very close to it. 

Obviously this method is going to be affected by the characteristics of your platform (hardware, OS etc), but it provides an easy way to roughly estimate the time complexity of your algorithm. 

# Space complexity

When talking about space complexity we usually don't include the memory occupied by (reading in) the problem instance itself. That's why you see that some algorithms are said to use O(1) space. 

There is often a tradeoff between time and space - sometimes you can make an algorithm run faster by using more space (e.g. by using a lookup table), and vice versa. You will also find that making small changes to the requirements can allow you to use significantly faster algorithms - or the opposite. 

Don't forget that stack space in function calls also take up space. We can assume each function call takes O(1) stack space, so n function calls (nested) would take O(n) space, e.g:

```cpp
int f(n){
    if (n==0) return 0;
    return f(n-1);
}
```
This takes O(n) time and space because of the recursive calls stack up to a depth of n. 

# Non-recursive algorithms

In non-nested for loops you just add up the time complexities:
```python
for i = 0 -> a do something in O(1)
for i = 0 -> b do something in O(1)
```
Simply add up the for loops, you get O(a+b). 

In nested for loops you multiply the time complexities:
```python
for i = 0 -> a:
    for j = 0 -> b do something in O(1)
```
Here you multiply the for loops, obtaining O(a*b).

For algorithms that multiplicatively reduce the problem space with each step, such as binary search, you can end up with a O(log n) runtime (as in the case of binary search). As it turns out, the base of the log doesn't matter because different log bases only differ by a constant factor. You can look up the change-of-base formula online. 

Useful things to know (some are from Sedgewick Algorithms 4th): 
- Arithmetic sum: 1 + 2 + 3 + ... + n is O(n^2)   
- Polynomial sum: 1^k + 2^k + 3^k + ... + n^k is O(n^(k+1))
- Geometric sum: 2^0 + 2^1 + 2^2 + ... + 2^n = 2^(n+1) - 1      
- Geometric sum: x^0 + x^1 + x^2 + ... + x^n is O(x^n)   
- Geometric sum: x^0 + x^1 + x^2 + ... + n is O(n)   
- Geometric sum: 1 + 1/k^1 + 1/k^2 + ... + 1/k^n = O(1)
- Above Harmonic sum: 1/1^0.999 + 1/2^0.999 + ... + 1/n^0.999 is (n^0.001)   
- Harmonic sum: 1/1 + 1/2 + 1/3 + ... + 1/n is O(log n)    
- Below Harmonic sum: 1/1^1.001 + 1/2^1.001 + ... + 1/n^1.001 is O(1)    
- Stirling's approximation: log n! = log 1 + log 2 + ... + log n is O(n log n)   
- Number of ways to choose k elements out of an n-element set: O(n^k) (Binomial coefficients: n choose k when k is a constant) 
- Number of permutations of a n-element set: n!
- Number of subsets of a n-element set: 2^n
- Number of strings of length k from an alphabet of size a: a^k
- It takes log n number of digits to represent the number n 
- (a + b) mod m = ((a mod m) + (b mod m)) mod m
- (ab) mod m = ((a mod m)(b mod m)) mod m
- log (n^k) = k log n

The input size is usually the number of bits needed to represent the problem instance. Binary encoding is fine, unary is not. The reason is that in binary it takes log n number of bits to represent a value of n, whereas in unary it takes n bits to represent the value n - exponentially more inefficient. If you want to know more about problem encodings and model of computation (and why it doesn't matter), read any good computational complexity textbook (e.g Sipser page 287, Computational Complexity: A Modern Approach, Papadimitriou). 

## Exercises 2

Example 8 from CTCI 6:   </br>
What's the runtime of this algorithm:
```
given an array of strings, sort each string and then sort the full array. 
```
<details>
  <summary>Solution: (Click to expand)</summary>
  The algorithm has 2 stages: first is sorting each string, second is sorting the array. Sorting a string of length s is O(s log s), sorting an array of n elements is O(n log n). First stage is O(n * s log s) altogether, since there are n strings and sorting each string takes O(s log s). Second stage is O(n log n) because sorting requires O(n log n) comparisons, assuming that each comparison operation is O(1). But of course the comparison operation is not O(1) but O(s) because each element is length s. So since there are O(n log n) comparisons and each comparison is O(s), sorting the elements takes O(s * n log n). Add up the 2 stages you get O(n * s log s + s * n log n). Simplify and we get O(s * n * (log s + log n)). 
</details>
</br>

Door in a wall (from Introduction to The Design and Analysis of Algorithms 3rd Edition by Anany Levitin)    </br>
You are facing a wall that stretches infinitely in both directions. There is a door in the wall, but you know neither how far away nor in which direction. You can see the door only when you are right next to it. Design an algorithm that enables you to reach the door by walking at most O(n) steps where n is the (unknown to you) number of steps between your initial position and the door.

<details>
  <summary>Solution: (Click to expand)</summary>
Go alternately left and right repeatedly until you find the door. First go 1 step left, then 1 step back to origin, then 2 steps right, then 2 steps back to origin, then 4 steps to the left, then 4 steps back to origin, then 8 steps right and so on. Basically, for i=0->infinity, make 2^i steps in one direction, then go back to origin, change direction and repeat. The overall runtime is:    </br>
1 + 1 + 2 + 2 + 4 + 4 + ... + n    </br>
= 2 + 4 + 8 + ... + n = O(n)    </br>
Thus, it takes O(n) steps to reach any distance n from left or right. Thus the algorithm is O(n) where n is the distance of the door from where you started.     
</details>
</br>


OpenDSA Example 8.8.5:   </br>
What is the time complexity of this algorithm:
```python
for (k=1; k<=n; k*=2)    // Do log n times
   for j = 1->k:  // Do k times
      do something in O(1)
```
<details>
  <summary>Solution: (Click to expand)</summary>
      It's O(n). The values of k are 1, 2, 4, ..., n. The inner loop therefore runs 1, 2, 4, ..., n times. So the total number of steps is the geometric progression 1 + 2 + 4 + ... + n, which is O(n). 

</details>
</br>

Example from [Data Structures and Algorithms Solving Recurrence Relations Lecture Notes by Chris Brooks](http://www.cs.cmu.edu/~rweba/algf09/solverecurrencesSF.pdf):   </br>
What is the time complexity of this algorithm:
```python
for i = 1->n^2:
    for j = 1->i:
        do something in O(1)
```

<details>
  <summary>Solution: (Click to expand)</summary>
We can solve this using substitution. Replace n^2 with k and we get the arithmetic sum 1 + 2 + ... + k = O(k^2). Substitute n^2 back in and we get O((n^2)^2) = O(n^4) which is the answer. 


</details>
</br>
   
Example 1.23 from Als16 (Design Techniques and Analysis Revised Edition by M. H. Alsuwaiyel):  </br>
What is the time complexity of this algorithm:
```python
def COUNT2(n):
    for i = 1->n:
        for j = 1->n/i:
            do something in O(1)
```
<details>
  <summary>Solution: (Click to expand)</summary>
The inner loop runs n/1, n/2, n/3, ..., n/n times. Factor out the n and we get the sum: n * (1/1 + 1/2 + 1/3 + ... + 1/n). Notice that the part inside the brackets is just the harmonic series, which is O(log n). Simply multiply that by n to get the time complexity of COUNT2, which is O(n log n). 


</details>
</br>

Example from KT page 53:  </br>
What is the time complexity of this algorithm, where k is a constant and G is a graph of size n:
```python
For each subgraph S of size k in graph G:
    Check whether S constitutes an independent set 
```
Given that the inner loop takes O(k^2) time (since we have to test if each pair of nodes in S is connected).   

<details>
  <summary>Solution: (Click to expand)</summary>
Since k is a constant then the inner loop takes O(1) time. There are n choose k = O(n^k) subgraphs in G. So the total running time is O(n^k). 


</details>
</br>

Example 1.26 from Als16:   </br>
What is the time complexity of this algorithm:
```python
def f(n):
    j = 2
    while j <= n:
        j *= j     //squaring a number doubles its power
```
<details>
  <summary>Solution: (Click to expand)</summary>
    The value of j squares every iteration of the while loop: 2, 4, 16, 256, ..., n. We can rewrite it as follows: 2^1, 2^2, 2^4, 2^8...So we see that the power doubles every iteration. This is a double exponential series. We can write it as: 2^2^0 + 2^2^1 + 2^2^2 + 2^2^3 + ... + 2^2^k, where k is the number of terms in the sequence, which is what we want to find out, as it is the time complexity of the whole algorithm. Now we have:</br>
    2^2^k = n</br>
    2^k = log n</br>
    k = log log n</br>
    Thus, the algorithm has O(log log n) time complexity. 

</details>
</br>

Example 1.22 from Als16:   </br>
What is the time complexity of this algorithm:
```python
def COUNT1(n):
    k = sqrt(n)
    for i = 1->k:
        for j = 1->i^2:
            do something in O(1)
```

<details>
  <summary>Solution: (Click to expand)</summary>
We have the series 1^2 + 2^2 + 3^2 + ... + sqrt(n)^2. Actually, we made it more difficult by substituting k with sqrt(n) at this stage. We should leave k as it is: 1^2 + 2^2 + 3^2 + ... + k^2 = O(k^3). Since k = n^0.5, replace k with n^0.5 and you get the answer: O((n^0.5)^3) = O(n^1.5). The trick is to calculate the time complexity first in terms of k and then substitute n for k to get the time complexity in terms of n. 


</details>
</br>

Problem-35 from Kar16 (Data Structures and Algorithms Made Easy: Data Structures and Algorithmic Puzzles, Fifth Edition by Narasimha Karumanchi):   </br>
What is the time complexity of this algorithm:
```python
for i = 1->n:
    for (j = 1; j < n; j += i):
        do something in O(1)
```

<details>
  <summary>Solution: (Click to expand)</summary>
   This is really just a repeat of the earlier Harmonic series problem. First figure out how many times the inner loop runs. When j is increasing by 1 each iteration, it runs n times. When increasing by 2, it runs n/2 times, and so on. So we get the series n + n/2 + n/3 + ... + n/n. Thus the time complexity is the same as COUNT2, which is O(n log n). 


</details>
</br>

Example 3.3 from Manber89 (Introduction to Algorithms by Udi Manber):     </br>
What is the runtime of this algorithm:
```python
def f(n):
    for i=1->n:
        for j=1->i:
            run 2^i steps
```

<details>
  <summary>Solution: (Click to expand)</summary>
    The series is 1 * 2^1 + 2 * 2^2 + 3 * 2^3 + ... + n * 2^n. We apply the technique:</br>
    G = 1 * 2^1 + 2 * 2^2 + 3 * 2^3 + ... + n * 2^n   </br>
    2G = 1 * 2^2 + 2 * 2^3 + 3 * 2^4 + ... + n * 2^(n+1)   </br>
    2G - G = -1 * 2^1 - 1 * 2^2 - 1 * 2^3 - ... - 1 * 2^n + n * 2^(n+1)   </br>
    Apply geometric sum and we get:    </br>
    G = -2^(n+1) + 2 + n * 2^(n+1)   </br>
    = (n-1) * 2^(n+1) + 2   </br>
    = O(n * 2^n)   

</details>
</br>

Example 3.4 from Manber89:    </br>
What is the runtime of this algorithm:   
```python
def f(n):
    for i=1->n:
        for j=1->i:
            run 2^(n-i) steps
```


<details>
  <summary>Solution: (Click to expand)</summary>
    The series is 1 * 2^(n-1) + 2 * 2^(n-2) + ... + n * 2^(0). Apply the same technique:</br>
    G = 1 * 2^(n-1) + 2 * 2^(n-2) + 3 * 2^(n-3) + ... + n * 2^(0)</br>
    2G = 1 * 2^(n) + 2 * 2^(n-1) + 3 * 2^(n-2) + ... + n * 2^(1)</br>
    2G - G = 1 * 2^(n) + 1 * 2^(n-1) + 1 * 2^(n-2) + ... + 1 * 2^(1) - n * 2^(0)</br>
    Apply geometric sum to get:   </br>
    G = 2^(n+1) - 2 - n * 2^0   </br>
    = 2^(n+1) - n - 2   </br>
    = O(2^n)

</details>
</br>


# Amortized runtime analysis

Amortized runtime analysis gives the average cost of an operation over a sequence of operations. There are several methods, this one's called aggregate analysis. There's also the accounting method and the potential method, you can find out more in CLRS3 chapter 17.

CLRS3 Example from Figure 17.1:   
Suppose you have a stack, each push and pop operation is O(1), and you have this function:
```
def multipop(stack, k):
    while not stack.empty and k > 0:
        stack.pop()
        k--
```
The time complexity of multipop is max(stack.size(), k), if there are n pushes then the max size of the stack is n, which means the time complexity of multipop is O(n) in the worst case. In a simplistic analysis, if there are n pushes and n multipops, since each multipop is worst case O(n), then n multipops gives O(n^2) worst case. However, using aggregate analysis, we see that each item can be popped at most once for each time it's pushed onto the stack. If n items are each pushed once (since each push only adds one item) onto the stack, and the stack is empty at the end, then each item must have been popped exactly once as well. So the number of pushes and pops is the same - n. 

In a sequence containing a number of multipop calls, the total cost of all the multipop calls is the same as the total number of pops, since in each iteration of the while loop, one call to pop is made, so the cost of a multipop operation is the same as the number of pops it makes. And we know that there can be at most n pops, which means the total cost of all the multipop calls, regardless of how many multipop calls there are in the sequence of operations, can at most be n. Which means that a sequence of n operations consisting of pushes, pops, and multipops, has a total cost of O(n), which yields an amortized cost of O(n) / n = O(1) amortized cost per operation. 

In general, a principle that is often used for amortized analysis is that the number of objects destroyed is at most the number created, so if you have two operations, one which creates one object, and the other which can destroy any number of objects, a sequence of n such operations takes O(n) time. 

Note however that if you then add a multipush operation then the analysis no longer holds, since you can then have n multipush operations each costing O(k) and n multipop operations each costing O(k), and the total cost would be O(nk). 

Another example from CTCI6:   
In a dynamic array (in C++ called a vector) that doubles itself every time it expands past its limit, you get amortized O(1) insertion cost. 

Every time the vector doubles itself, it has to copy all of its elements to a new memory region. So if you insert n elements, it copies itself when it has 1, 2, 4, 8, 16...elements. So the total cost is n (each normal insert is O(1)) plus 1 + 2 + 4 + 8 + 16 + ... + n (for the insertions when the vector is full). Which is O(n). So inserting n elements takes O(n) time which means the amortized cost of each insert is O(1). 

Funny thing: if the vector automatically halves its capacity, then a sequences of n alternating inserts and deletes at the threshold would be O(n^2) - since every insert causes the vector to double itself and every delete causes the vector to halve itself. That is why in practice, vector implementations usually don't automatically shrink - they just grow forever. 

[Example 2](http://www.cs.cmu.edu/afs/cs/academic/class/15750-s01/www/notes/lect0123):   
Here is an implementation of a queue of size n using 2 stacks A and B of size n:
```
def transfer(): //this reverses a stack
    while A is not empty:
        x = A.pop()
        B.push(x)
def enqueue(x):
    A.push(x)
def dequeue():
    if B is empty: transfer()
    return B.pop()
```
Correctness: We want to show that the dequeue operation always returns the oldest item in the container. There are 2 situations: B is empty or not. If B is empty, we just reverse A into B and pop off the top of B. If B is not empty, then we want to show that the item at the top of B is the oldest. Since A is either emptied or has items added to it, it always maintains the stack property. Since B either has items popped off, or is a reversed version of A, it also holds the reversed stack property - thus, oldest item at top.   
Efficiency: Whilst enqueue is always O(1), the worst case for a dequeue operation is O(n) because we have to transfer potentially n items from A to B. However, we can show that, given any sequence of n enqueue and dequeue operations, the amortized cost of each operation is O(1). We can do this easily via the aggregate method: each item that is enqueued that is eventually dequeued goes through exactly 2 pushes and 2 pops - 1 push into A, 1 pop off A, 1 push into B, 1 pop off B. Since n items are enqueued and dequeued, that's O(n) total cost, for an amortized cost of O(1) per operation.  

Example 1.33 from Als16:   
We have a doubly linked list L that initially consists of one node which contains the integer 0. We have as input an array A[1..n] of n positive integers that are to be processed in the following way. If the current integer x is odd, then append x to the list. If it is even, then first append x and then remove all odd elements before x in the list. 
```
def append(x):
    if x % 2 == 1:
        L.append(x)
    else:
        while L[-1] is odd:
            delete L[-1]
        L.append(x)
```
Given n append operations, what is the time complexity of an append operation?   
Well, we can consider that in the worst case, L may consist of n elements, all of which are odd. Adding an even element to L would cause all of these n elements to be deleted, so append(x) is O(n) in the worst case. So, we have n append operations, and the worst case for one append operation is O(n), which gives us an upper bound of O(n^2). Is that a tight bound?   
Yes we can. Notice that the while loop must delete one element in each iteration after the first. Therefore, the number of iterations of the while loop in total (minus n) is just the number of elements deleted from the linked list. Since each element can be deleted from L at most once (each is only appended once, so logically they can be at most deleted once), this puts an upper bound on the number of iterations of the while loops at n (with n appends). Thus, the amortized time complexity of append is O(1). 

## Exercises 3

CLRS3 page 454 example incrementing a binary counter:   
Given a k-bit binary counter c. Flipping one bit of the counter is O(1). What's the time complexity of incrementing the counter n times? This is the algorithm:
```
def increment():
    i = 0
    while c[i] == 1 and i < len(n):
        c[i] = 0
        i++
    if i < len(n):
        c[i] = 1 
```
Solution:   
Clearly, the worst case for a call to increment flips all the bits (if all the bits are 1) in c, for O(c). A straightforward multiplication gives the worst case for n increments O(nc). Can we get a tighter bound? We can. Notice that the first bit is flipped every increment, whilst the second bit is flipped every 2nd increment, and the 3rd bit is flipped every 4th increment. This is obviously because in order for a bit to flip, it needs the preceding bit to flip from 1 to 0. In other words, a bit at position n flips when the bit at position n-1 has flipped twice. Thus we get the pattern: n + n/2 + n/4 + ... n/2^c, factoring out n you get the geometric series 1 + 1/2 + 1/4 ... which is just 2. So the sum is 2n = O(n). The amortized cost of each increment is therefore O(1). 

Note that this analysis only works when you only increment the counter. If you decrement the counter as well then it is trivial to ensure that you always trigger the worst case O(k) repeatedly by incrementing 111..1 to 100..0 and then decrementing it, repeatedly. In that case the amortized cost of both the increment operation and the decrement operation are just the same as the worst case: O(k). 

# Lower bounds

In order to answer the question of whether an algorithm's asymptotic time complexity is "optimal", we have to first ask "what is the best possible time complexity for any algorithm for this problem?". Thus we want to prove a lower bound for the problem itself, so that we know that an algorithm which hits this lower bound is optimal - there are no algorithms that can run faster (asymptotically). 

An example is matrix multiplication. Any algorithm that multiplies 2 nxn matrices must at least look at every element in each matrix, thus a lower bound of O(n^2). It is not yet known if this lower bound is tight. 

There is a proof that no comparison-based sort can do better than O(n log n). However it only applies to the most general case whe you have no additional information about the problem. If you know for example that all the elements are within a certain range then you can sometimes achieve O(n). 

# Recursive algorithms 

There are many ways to solve recurrence relations, such as iteration, telescoping, and recursion tree. 

## Order of growth of geometric sums

The sum of a geometric series c + cr + cr^2 + ... + cr^(n-1) is given by the expression:

c * (1 - r^n) / (1 - r)

When r != 1. 

When r = 1, then r^1, r^2 ... all equals 1, so the series is just c + c + c + ... + c, so the sum of that is just nc. The order of growth is also just O(nc).

For r != 1 we will prove the order of growth algebraically. 

c * (1 - r^n) / (1 - r) = c * (r^n - 1) / (r - 1)

Now there are 2 cases:

1. r > 1

In this case, since r > 1, r - 1 is a positive constant so we can multiply by it. This is fine because we don't care about constant factors in asymptotic analysis. 
```
c * (r^n - 1) / (r - 1) * (r - 1)
= c * (r^n - 1) 
= cr ^ n - c
```
Since r^n gets bigger as n grows, cr^n dominates c so it's O(cr^n). 

2. r `<` 1

In this case, r `<` 1 so 1 - r is a positive constant so we can multiply by it. 
```
c * (1 - r^n) / (1 - r) * (1 - r)
= c * (1 - r^n)
= c - cr^n
```
Since r^n gets smaller as n grows, c dominates cr^n so it's just O(c). 

## The master theorem

The master theorem is derived from the recursion tree method and can only be used to solve recurrence relations which are of a specific form, mainly divide-and-conquer recurrences.

Thm. if T(n) = aT(n/b) + O(n^d) for a > 0, b > 1, d >= 0 then the time complexity is

1. O(n^d log n) if d = logb(a)
2. O(n^d) if d > logb(a) 
3. O(n^(logb(a))) if d `<` logb(a)

Note that the master theorem ONLY applies when the recurrence can be written that way, so you cannot use the master theorem to solve the Fibonacci recurrence for example. 

Proof. There are 2 parts to this proof, found in CLRS3 4.6 (page 97). The first part of the proof assumes that n is a power of b, and the 2nd part (the smoothness rule) extends the first part of the proof for all values of n. All of the important intuition for understanding the master theorem is found in the 1st part, the 2nd part is just for handling floors and ceilings. 

Draw the recursion tree for T(n) = aT(n/b) + f(n):

Top level: Only 1 node (problem). Work done: f(n) = n^d. 
2nd level: There are a nodes (problems), each term doing f(n/b) work, so overall a * f(n/b) = a * (n/b)^d work. 
ith level: Each node has a children, so on the ith level there are a^i nodes. On each level the amount of work is divided by b, so the amount of work done in each node on the ith level is f(n/b^i) = (n/b^i)^d, so the total amount of work done on the ith level is a^i * (n/b^i)^d. 

To get the last level l we solve for 1 = n/b^l, since on the last level the size of each problem is 1:
```
b^l = n
l = logb(n)
```
So the last level is the logb(n)th level. The number of nodes on this level is a^logb(n) and each node does O(1) work (since input is size 1) so total amount of work done on this level is just a^logb(n) = n^logb(a) by the log identity. 

Adding the amount of work on each level, we obtain the following sum for total time complexity:

a * (n/b)^d + a^2 * (n/b^2)^d + ... + a^logb(n) * (n/b^logb(n))^d

Can we apply what we know about the geometric series to this expression? In order to do that, we need to convert the terms to the form of cr^i - the terms of a geometric series - so that we can apply what we know about geometric series to our series as well:
```
a^i * (n / b^i)^d
= a^i * n^d / b^id
= n^d * (a / b^d)^i
```
So now we have our terms in the form cr^i:
```
c = n^d
r = a / b^d
```
We therefore have:
```
a = b^d => r = 1 => logb(a) = d
a > b^d => r > 1 => logb(a) > d
a < b^d => r < 1 => logb(a) < d
```
And the geometric sum is:

n^d * (a / b^d) + n^d * (a / b^d)^2 + ... + n^d * (a / b^d)^logb(n)

Note that there are logb(n) terms NOT n, so we also need to substitute n = logb(n) when we can apply the formula for the order of growth of geometric sums to our sum:
```
Case 1. r = 1. O(nc). Substitute and we get O(logb(n) * n^d) = O(n^d log n). 
Case 2. r > 1. O(cr^n). Substitute and obtain O(n^d * (a / b^d) ^ logb(n)). 
Case 3. r < 1. O(c). Substitute and obtain O(n^d). 
```
And thus we obtain the master theorem. 

### Examples of how to use the master theorem

#### Binary search

The recurrence relation for binary search is:

T(n) = T(n/2) + O(1)

Apply the formula:
```
aT(n/b) + n^d
a = 1 
b = 2
d = 0
```
Since d = logb(a) the time complexity is O(n^d log n) = O(1 log n) = O(log n)

#### Mergesort, maximum subarray, and FFT

The recurrence relation for mergesort, maximum subarray, and fast fourier transform is:

T(n) = 2T(n/2) + O(n)

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 2
d = 1
```
Since d = logb(a) the time complexity is O(n^d log n) = O(n log n)

#### Fast heap construction

The recurrence relation for fast heap construction is:

T(n) = 2T(n/2) + O(log n)

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 2
d = 0 Note that log n is greater than O(1) but smaller than O(n^d) for any d > 0. 
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^log2(2)) = O(n)

#### Stooge sort

The recurrence relation for stooge sort is:

T(n) = 3T(2n/3) + 1

Apply the formula:
```
aT(n/b) + n^d
a = 3
b = 3/2
d = 0
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^log1.5(3)) ~= O(n^2.7095)

#### Karatsuba multiplication

The recurrence relation for Karatsuba multiplication is:

T(n) = 3T(n/2) + O(n)

Apply the formula:
```
aT(n/b) + n^d
a = 3
b = 2
d = 1
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^log2(3)) ~= O(n^1.585)

#### Strassen's multiplication

The recurrence relation for Strassen's multiplication is:

T(n) = 7T(n/2) + O(n^2)

Apply the formula:
```
aT(n/b) + n^d
a = 7
b = 2
d = 2
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^log2(7)) ~= O(n^2.807)

#### Max-heapify

The recurrence relation for max-heapify is:

T(2n/3) + 1

Apply the formula:
```
aT(n/b) + n^d
a = 1
b = 3/2
d = 0
```
Since d = logb(a) the time complexity is O(n^d log n) = O(1 log n) = O(log n)

#### Examples from CLRS3

Example 1. 9T(n/3) + n

Apply the formula:
```
aT(n/b) + n^d
a = 9
b = 3
d = 1
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^2)

Example 3. 3T(n/4) + n log n

Apply the formula:
```
aT(n/b) + n^d
a = 3
b = 4
d = 1 Note that n log n is greater than O(n) but smaller than O(n^d) for any d > 1. 
```
Since d > logb(a) the time complexity is O(n^d). It's bigger than O(n) but smaller than (n^d) for any d > 1. Actually it's O(n log n) because in such cases it's just O(f(n)) - this is from the master theorem proved in CLRS but proving that version is less convenient than this version. 

Example 4. 2T(n/2) + n log n

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 2
d = 1 Note that n log n is greater than O(n) but smaller than O(n^d) for any d > 1. 
```
Note that the time complexity of n log n when expressed in terms of O(n^d), the d value cannot be greater than 1, but at the same time n log n is not O(n) either. So d is neither equal to 1 nor is it greater. So we actually can't solve this using the master theorem. 

Example 5. 2T(n/2) + O(n)

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 2
d = 1
```
Since d = logb(a) the time complexity is O(n^d log n) = O(n log n)

#### CLRS Exercise 4.5-1

a. 2T(n/4) + 1

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 4
d = 0
```
Since d `<` logb(a) the time complexity is O(n^(logb(a))) = O(n^0.5)

b. 2T(n/4) + n^0.5

Apply the formula:
```
aT(n/b) + n^0.5
a = 2
b = 4
d = 0.5
```
Since d = logb(a) the time complexity is O(n^d log n) = O(n^0.5 log n)

c. 2T(n/4) + n

Apply the formula:
```
aT(n/b) + n^d
a = 2
b = 4
d = 1
```
Since d > logb(a) the time complexity is O(n^d) = O(n)

#### More examples

Example 11.9 from Algorithm Design and Applications:

2T(n^0.5) + log n

Solve using substitution:
```
T(n) = T(2^k) = 2T(2^0.5k) + k
S(k) = T(2^k)
S(k) = 2S(0.5k) + k
S(k) is O(k log k)
T(n) is O(log n log log n)
```
Example from CLRS:

T(n) = 2T(n^0.25) + O(1)

Solve using substitution:
```
T(n) = T(2^k) = 2T(2^0.25k) + O(1)
S(k) = T(2^k)
S(k) = 2S(k/4) + O(1)
S(k) is O(k^log4(2)) = O(k^0.5)
T(n) is O((log n)^0.5)
```














## [Not done yet]

The following recurrence relations:
T(n) = T(n-1) + T(n-1)
T(n) = T(n-1) + T(n-2)  //Fibonacci
 

CTCI Example 15:  
What is the runtime of this algorithm:
```
def f(n):
    for i = 0 -> n: g(i)
def g(n):
    if n == 0 return 0
    return g(n-1) + g(n-1)
```
Solution: We know that each function call g(i) takes O(2^i) time, so we end up with a sequence of function calls each taking 2^0, 2^1, 2^2...2^n time. As shown earlier with the summation, this is actually just 2^(n+1)-1 which is still O(2^n). 


Example 13 from CTCI 6:  
What's the runtime of this algorithm:
```
def f(n):
    if (n <= 1) return 0
    else return f(n-1) + f(n-2)
```
Solution: This is the Fibonacci function. Draw the call tree to see that it is exponential. The asymptotic tight bound is actually not O(2^n) but something more like (1.618^n) - the base converges to the Golden ratio. Manber89 page 49 shows how you arrive at that by the guess-and-try-to-prove method. 

