# Merge sort

## Idea

_Merge sort_ is an efficient algorithm that
applies the concepts of [_divide and conquer_][d&c] to sort the list.

The key idea of [_divide and conquer_][d&c] is to recursively break down the problems
into two or more sub-problems and they are same or related to the original problem,
until these divided sub-problems are simple enough to solve directly.
Then, the solutions of the original problem can be combined and derived
by the solutions of all the sub-problems.

### Divide and conquer

The calculation of [_Fibonacci number_][fib], \\( F(n) = F(n-1) + F(n-2) \\),
is one example.
To calculate \\( F(n) \\), it needs to find \\( F(n-1) \\) and \\( F(n-2) \\).
Similarly, to calculate \\( F(n-1) \\), it needs to \\( F(n-2) \\) and \\( F(n-3) \\).
The sub-problems for calculating \\( F(n-1) \\) and \\( F(n-2) \\) have same form
as the one for \\( F(n) \\).

Recursively, we will need to get \\( F(n-1) \\), \\( F(n-2) \\), ..., \\( F(2) \\), \\( F(1) \\)
and \\( F(1) \\) and \\( F(2) \\) are easy enough to solve directly. They are both \\( 1 \\).
Thus, \\( F(3) = F(2) + F(1) = 2 \\), \\( F(4) = F(3) + F(2) = 3 \\), ... and then \\( F(n) \\)
can be computed.

### Dividing the sorting-problem

Let's apply this concept to the sorting problem.
If we want to sort the list \\( L = [6, 3, 7, 1, 9, 2, 5] \\),
the sub-lists \\( [6, 3, 7, 1], [9, 2, 5] \\) must also be sorted,
so we can narrow down our problem scope for handling the sub-lists.
Next, \\( [6, 3, 7, 1] \\) can be divided to \\( [6, 3], [7, 1] \\)
and \\( [6, 3] \\) also can be split into \\( [6] \\) and \\( [3] \\).
Finally, \\( [6], [3] \\) are not dividable
so we stop breaking down the list.

<pre>
       [6, 3, 7, 1, 9, 2, 5]
         /               \
   [6, 3, 7, 1]       [9, 2, 5]
    /        \          /     \
 [6, 3]    [7, 1]    [9, 2]  [5]
 /    \    /    \    /    \
[6]  [3]  [7]  [1]  [9]  [2]
</pre>

In the same way, the whole list can be divided into
\\( [6], [3], [7], [1], [9], [2], [5] \\).

### Conquering the sub-problems

After there is only one element left,
the subproblem is solved by nature since it's already sorted.

However, the problem becomes
> how do we combine these sorted chunks into a sorted list

We need a method that can merge two sorted lists,
\\( L_1[1...X] \\) and \\( L_2[1...Y] \\), where \\( X, Y \geq 1 \\),
into a bigger sorted list \\( L[1...(X+Y)] \\).

### Combining all the results of sub-sorting-problem

Suppose we have two sorted lists \\( A = [3, 6, 10, 23] \\) and \\( B = [2, 7, 50, 55] \\).
We provide two ways to merge them into a sorted list.

#### Picking the smallest elements one by one

The simplest method is to pick the smallest elements iteratively
by searching both lists from the minimal to maximal.

We only need to compare the left most elements of both lists
and pick the smaller one since \\( A \\) and \\( B \\) are already sorted.

The following example demonstrate the process of this idea:

<pre>
() <- search index

A = [(3), 6, 10, 23]
B = [(2), 7, 50, 55]
L = []                            // <- 2
</pre>
In the first round, \\( 2 \\) is picked since \\( 2 < 3 \\)
and going to be put into another list \\( L \\).

<pre>
                                  // You can think the left most element
                                  // is shifted one by one
A = [(3), 6, 10, 23]              // [3, 6, 10, 23]
B = [2, (7), 50, 55]              // [7, 50, 55]
L = [2]                           // <- 3
</pre>

After \\( 2 \\) is picked, we move the index of \\( B \\) from \\( 2 \\) to \\( 7 \\).
Next, \\( 3 \\) is picked since \\( 3 < 7 \\) and going to be put into \\( L \\).

<pre>
A = [3, (6), 10, 23]              // [6, 10, 23]
B = [2, (7), 50, 55]              // [7, 50, 55]
L = [2, 3]                        // <- 6
</pre>

After \\( 3 \\) is picked, we move the index of \\( A \\) from \\( 3 \\) to \\( 6 \\).
Next, \\( 6 \\) is picked since \\( 6 < 7 \\) and going to be put into \\( L \\).

<pre>
A = [3, 6, (10), 23]              // [10, 23]
B = [2, (7), 50, 55]              // [7, 50, 55]
L = [2, 3, 6]                     // <- 7
</pre>

After \\( 6 \\) is picked, we move the index of \\( A \\) from \\( 6 \\) to \\( 10 \\).
Next, \\( 7 \\) is picked since \\( 7 < 10 \\) and going to be put into \\( L \\).

<pre>
A = [3, 6, (10), 23]              // [10, 23]
B = [2, 7, (50), 55]              // [50, 55]
L = [2, 3, 6, 7]                  // <- 10
</pre>

After \\( 7 \\) is picked, we move the index of \\( B \\) from \\( 7 \\) to \\( 50 \\).
Next, \\( 10 \\) is picked since \\( 10 < 50 \\) and going to be put into \\( L \\).

<pre>
A = [3, 6, 10, (23)]              // [23]
B = [2, 7, (50), 55]              // [50, 55]
L = [2, 3, 6, 7, 10]
</pre>

After \\( 10 \\) is picked, we move the index of \\( A \\) from \\( 10 \\) to \\( 23 \\).
Next, \\( 23 \\) is picked since \\( 23 < 50 \\) and going to be put into \\( L \\).

<pre>
A = [3, 6, 10, 23]                // []
B = [2, 7, (50), 55]              // [50, 55]
L = [2, 3, 6, 7, 10, 23]
</pre>

After \\( 23 \\) is picked, there is no need to compare again
since the \\( 23 \\) is the last element in \\( A \\).

<pre>
A = [3, 6, 10, 23]                // []
B = [2, 7, 50, 55]                // []
L = [2, 3, 6, 7, 10, 23, 50, 55]
</pre>

Next, we can append all the rest elements
from \\( 50 \\) to the end of \\( B \\) into the \\( L \\).
Finally, we get a sort list \\( L \\).

#### Swapping the elements one by one

Another idea to merge the two sorted lists
\\( A = [3, 6, 10, 23] \\) and \\( B = [2, 7, 50, 55] \\),
is to couple them together into a list \\( L = A \cup B \\)

<pre>
() <- element who will be moved
L = [3, 6, 10, 23, | (2), 7, 50, 55]

// The '|' doesn't exist! It's only a notation for better explanation.
</pre>

and then move the minimal element of the later list(\\( B \\))
to the right position of the former list(\\( A \\)).

The way for finding right the position is to compare the elements one by one
from the end of the former list(\\( A \\)) to its head.

<pre>
() <- element who will be moved
L = [3, 6, 10, (2), 23, | 7, 50, 55]
</pre>

In our example, the \\( 2 \\) is swapped with \\( 23 \\) since \\( 2 < 23 \\).
Then we keep comparing \\( 2 \\) with \\( 10 \\).

<pre>
() <- element who will be moved
L = [3, 6, (2), 10, 23, | 7, 50, 55]
</pre>

Similarly, the \\( 2, 10 \\) are swapped since \\( 2 < 10 \\).

<pre>
() <- element who will be moved
L = [3, (2), 6,  10, 23, | 7, 50, 55]
</pre>

Next, the \\( 2, 6 \\) are swapped since \\( 2 < 6 \\).

<pre>
() <- element who will be moved
L = [(2), 3, 6,  10, 23, | 7, 50, 55]
</pre>

Next, the \\( 2, 3 \\) are swapped since \\( 2 < 3 \\).
After this round, there is nothing to compare,
so the \\( 2 \\) is moved to its right position.
Now \\( A = [2, 3, 6, 10, 23] \\) and \\( B = [7, 50, 55] \\)

In the same way, we can do this process again with \\( 7 \\).
It's the minimal element of the later list \\( B \\) now.

<pre>
() <- element who will be moved
L = [2, 3, 6, 10, 23, | (7), 50, 55]
L = [2, 3, 6, 10, (7), 23, | 50, 55]
L = [2, 3, 6, (7), 10, 23, | 50, 55]

L = [2, 3, 6, 7, 10, 23, | 50, 55]
</pre>

After this round, \\( A = [2, 3, 6, 7, 10, 23] \\) and \\( B = [50, 55] \\).

Then do it again with with \\( 50 \\).

<pre>
() <- element who will be moved
L = [2, 3, 6,  7, 10, 23, | (50), 55]

L = [2, 3, 6,  7, 10, 23, 50, | 55]
</pre>

However, \\( 50 \\) doesn't move because \\( 23 < 50 \\).
We just need to append \\( 50 \\) to the end of the former list \\( A \\).
After this round, \\( A = [2, 3, 6, 7, 10, 23, 50] \\) and \\( B = [55] \\).

<pre>
() <- element who will be moved
L = [2, 3, 6,  7, 10, 23, 50, | (55)]

L = [2, 3, 6,  7, 10, 23, 50, 55]
</pre>

It's same to \\( 55 \\).
It doesn't need to be moved since \\( 50 < 55 \\),
so just append it to the \\( A \\).
Finally, \\( A = [2, 3, 6, 7, 10, 23, 50, 55] \\) and \\( B = [] \\) is empty now.
Now we have a sorted list \\( L = A \cup B = A \\)!

#### Which merge method is better

The first method use __extra space__ to store the sorted results,
rather than the second in-place solution.
On the other hand, the second method needs __more swapping executions__
and its a linear operation.
For better performance, we take the first method as our approach here.

Actually, there is a way to save the extra space
and it works as fast as the first method above.
However, it's complicated.
I will write another post for illustrating it.
Please refer _In-place merge sort_ in [Elementary Algorithms][eleAlg]
to read it.

## Algorithm

\\[
\begin{aligned}
& MergeSort(L): \newline
& \space \space \space \space mergeSort(L, 1, \vert L \vert)
\end{aligned}
\\]

\\[
\begin{aligned}
& mergeSort(L, l, r): \newline
& \space \space \space \space \text{if } l < r: \newline
& \space \space \space \space \space \space \space \space m \leftarrow \lfloor \frac{l+r}{2} \rfloor \newline
& \space \space \space \space \space \space \space \space mergeSort(L, l, m) \newline
& \space \space \space \space \space \space \space \space mergeSort(L, m+1, r) \newline
& \space \space \space \space \space \space \space \space merge(L, l, m, r)
\end{aligned}
\\]


\\[
\begin{aligned}
& merge(L, l, m, r): \newline
& \space \space \space \space L^\prime \leftarrow [] \newline
& \space \space \space \space i \leftarrow l, j \leftarrow m+1, k \leftarrow l \newline
& \space \space \space \space \text{while } i \leq m \text{ and } j \leq r: \newline
& \space \space \space \space \space \space \space \space \text{if } L[i] < L[j]: \newline
& \space \space \space \space \space \space \space \space \space \space \space \space L^\prime[k] \leftarrow L[i] \newline
& \space \space \space \space \space \space \space \space \space \space \space \space i \leftarrow i + 1 \newline
& \space \space \space \space \space \space \space \space \text{else}: \newline
& \space \space \space \space \space \space \space \space \space \space \space \space L^\prime[k] \leftarrow L[j] \newline
& \space \space \space \space \space \space \space \space \space \space \space \space j \leftarrow j + 1 \newline
& \space \space \space \space \space \space \space \space k \leftarrow k + 1 \newline
& \space \space \space \space \text{while } i \leq m: \newline
& \space \space \space \space \space \space \space \space L^\prime[k] \leftarrow L[i] \newline
& \space \space \space \space \space \space \space \space i \leftarrow i + 1 \newline
& \space \space \space \space \space \space \space \space k \leftarrow k + 1 \newline
& \space \space \space \space \text{while } j \leq r: \newline
& \space \space \space \space \space \space \space \space L^\prime[k] \leftarrow L[j] \newline
& \space \space \space \space \space \space \space \space j \leftarrow j + 1 \newline
& \space \space \space \space \space \space \space \space k \leftarrow k + 1 \newline
& \space \space \space \space \text{for } i \leftarrow l \text{ to } r: \newline
& \space \space \space \space \space \space \space \space L[i] \leftarrow L^\prime[i]
\end{aligned}
\\]

### Proof

#### Correctness of _Merge_

<pre>
  List L

        <------   sorted   ------> <------   sorted  ------->
        <- merged -> <---  A  ---> <- merged -> <---  B  --->
  -----+---+--------+---+-----+---+-----+------+---+-----+---+-----
   ... | l | ...... | i | ... | m | m+1 | .... | j | ... | r | ...
  -----+---+--------+---+-----+---+-----+------+---+-----+---+-----
                      ^                          ^
                head of sublist A          head of sublist B

  List L'

   <--   merged  --> <---   empty   --->
  +---+-------+-----+---+-------+-------+
  | 1 |  ...  | k-1 | k | ..... | r-l+1 |
  +---+-------+-----+---+-------+-------+
                      ^
                head of empty area of list L'

  L[l...m]    : the sorted sublists for merging with L[m+1...r]
  L[m+1...r]  : the sorted sublists for merging with L[l...m]
  A, B        : the sublists containing elements that have NOT been merged yet
  L'[1...k-1] : the merged list from L[l...i-1] and L[m+1...j-1]

  i: The index of the first element in L[l...m] that has NOT been merged yet
  j: The index of the first element in L[m+1...r] that has NOT been merged yet
  k: The index of next merged element copied from L[i] or L[j]
</pre>

__Loop Invariant__:
At the beginning of the while-loop, the following conditions hold:

1. Sublists \\( L[i...m] \\) and \\( L[j...r] \\) are sorted
2. \\( L^\prime \\) holds the elements from sublists \\( L[l...i-1] \\) and \\( L[m+1...j-1] \\)
3. All elements in \\( L^\prime[1...k-1] \\) is less or equal than
   sublists \\( L[i...m] \\) and \\( L[j...r] \\)
4. \\( L^\prime \\) are sorted.
   Formally, \\( \forall i \in [l + 1, r], L^\prime[i - 1] \leq L^\prime[i] \\)

Then we use loop-invariants to prove:

- Initialization: At the very beginning when \\( k = 1, i = l, j = m+1 \\)
  - the input \\( L[l...m], L[m+1...r] \\) are sorted so _1_ holds
  - the list \\( L^\prime \\) is empty so _2, 3, 4_ hold
- Maintenance: Consider the iteration \\( k = x \\)
  - _1_ is preserved since there is no change in \\( L \\)
  - _2_ is preserved because
    - If \\( j > r \lor (i \leq m \land L[i] < L[j]) \\), \\( L^\prime[k] \leftarrow L[i] \\)
      - then \\( k \leftarrow k+1, i \leftarrow i+1 \\)
      - _3_ is preserved because \\( L[k-1] = L[i-1] < L[j] \leq L[j+1] \leq ... \leq L[r] \\)
        and \\( L[k-1] = L[i-1] \leq L[i] \leq ... \leq L[m] \\)
    - Otherwise, \\( L^\prime[k] \leftarrow L[j] \\)
      - then \\( k \leftarrow k+1, j \leftarrow j+1 \\)
      - _3_ is preserved because \\( L[k-1] = L[j-1] < L[j] \leq L[j+1] \leq ... \leq L[r] \\)
        and \\( L[k-1] = L[j-1] \leq L[i] \leq L[i+1] \leq ... \leq L[m] \\)
  - The previous appended element must be smaller than
    the current selected minimal element or _1_ is false
  - By _3_, the next selected minimal element will be larger than current one
  - So _4_ is also preserved
- Termination
  - By __2__, \\( L^\prime \\) consists of the elements in \\( L[l...r] \\)
  - By __4__, \\( L[l...r] = L^\prime[l...r] \\) are sorted

#### Correctness of _Merge Sort_

> Given a list \\( L \\) with \\( N \\) elements,
> the \\( L \\) can be sorted
> by applying the above the _MergeSort_ with \\( l = 1, r = N \\).

- Base step: When \\( N = 1 \\), it's trivial.
- Induction Hypothesis:
  Suppose this assumption holds when list has \\( N = 1, 2, ..., k \\) elements
- Induction Step: When \\( N = k + 1 \\)
  - the list \\( L \\) is divide to \\( L[1...m] \\)(\\( m \\) elements)
    and \\( L[m+1...N] \\)(\\( N - m \\) elements)
  - so \\( m = \lfloor \frac{1+(k+1)}{2} \rfloor = \lfloor \frac{k}{2} \rfloor + 1 \leq k \\)
  - \\( 1 \leq m \to 0 \leq m-1  \\)
    - \\(  \to k \leq k-1+m  \\)
    - \\(  \to k+1 \leq k+m  \\)
    - \\(  \to (k+1)-m \leq k  \\)
    - \\(  \to N-m \leq k \\)
  - By our hypothesis, \\( L[1...m] \\) and \\( L[m+1...N] \\) can be sorted
  - By the proved correctness of _merge_ above,
    the merged \\( L[1...m] \\) and \\( L[m+1...N] \\) is also sorted,
    so the proof is done

## Complexity

<pre>
  ^    +------------------------------------------------------+   Merge
  |    |                           N                          |   Complexity
  |    +------------------------------------------------------+
  |                 |                              |
  |                 v                              v
  |    +------------------------+    +------------------------+
  |    |           N/2          |    |           N/2          |   2 * O(N/2)
  |    +------------------------+    +------------------------+
  |         |              |              |              |
            v              v              v              v
  K    +---------+    +---------+    +---------+    +---------+
       |   N/4   |    |   N/4   |    |   N/4   |    |   N/4   |   4 * O(N/4)
  |    +---------+    +---------+    +---------+    +---------+
  |      |     |        |     |        |     |        |     |
  |      v     v        v     v        v     v        v     v
  |
  |                        .  .  .  .  .  .                       2^i * O(N/(2^i))
  |
  |    +---+  +---+  +---+                                +---+
  |    | 1 |  | 1 |  | 1 |  .  .  .  .  .  .  .  .  .  .  | 1 |   N * O(1)
  v    +---+  +---+  +---+                                +---+

  N: the number of list elements.
  K: K layers from N to 1.
     N/2^k = 1 => N = 2^K => K = log_2(N)
</pre>

The above figure is the __recursion tree__ of _merge sort_.
The list containing \\( N \\) elements is recursively divided to sort
until there is only one elements.
Suppose that there is \\( K \\) times of division, therefore,

\\[
\begin{aligned}
\frac{ N }{ 2^K }   &= 1 \newline
\to                 N &= 2^K \newline
\to                 K &= \log_{ 2 }N
\end{aligned}
\\]

On the other hand, the time complexity
depends on the performance of _merge_ \\( T_{merge}(N) \\).
The used _merge_ here is the basic version.
It iteratively picks the minimal elements from both sublists
then copied to another list \\( L^\prime \\).
After all the elements in one sublist are all selected,
we move the rest elements in the other sublist to list \\( L^\prime \\).
Finally, we assigned \\( L[i] \leftarrow L^\prime[i] \\), \\( \forall i \in [1, N] \\).
Thus, \\( T_{merge}(N) \\) can be defined as

\\[
T_{merge}(N) = c \cdot N
\\]

where \\( c \\) is a constant reflecting the basic operations
like comparisons or assignments for merging routine.

### By the recursion tree

From the above figure, the total time for the _merge sort_ is

\\[
\begin{aligned}
Time &= 2 \cdot c \cdot \frac{N}{2} +4 \cdot c \cdot \frac{N}{4} + 8 \cdot c \cdot \frac{N}{8} + ... + N \cdot c \cdot 1 \newline
&= K \cdot ( c \cdot N)
\end{aligned}
\\]
, where \\( K \\) is the number of terms in the above equation.

The term is \\( 2^K \cdot c \cdot \frac{N}{2^K} \\) 
and it is from \\( 2 \cdot c \cdot \frac{N}{2} \\) to \\( N \cdot c \cdot 1 \\),
so we have \\( K = \log_{ 2 }N \\) terms.
As a result,

\\[
Time = c \cdot N \cdot \log_{ 2 }N
\\]

Thus, the time complexity is \\( \mathcal{O}(N \log N) \\).

### By telescoping

Formally, since the _merge sort_ repeatedly breaks down the \\( N \\)-elements list
into two \\( \frac{N}{2} \\)-elements sublists,
the amount of time that _merge sort_, \\( T_{sort}(N) \\),
can be written as follows:

\\[
\begin{aligned}
T_{sort}(N)
&= T_{sort}(\frac{N}{2}) + T_{sort}(\frac{N}{2}) + T_{merge}(N) \newline
&= 2 \cdot T_{sort}(\frac{N}{2}) + T_{merge}(N) \newline
&= 2 \cdot T_{sort}(\frac{N}{2}) + c \cdot N
\end{aligned}
\\]

\\[
\begin{aligned}
T_{sort}(N)
&= 2 \cdot T_{sort}(\frac{N}{2}) + c \cdot N \newline
&= 2 \cdot (2 \cdot T_{sort}(\frac{N}{4}) + c \cdot \frac{N}{2}) + c \cdot N \newline
&= 2^2 \cdot T_{sort}(\frac{N}{4}) + 2 \cdot c \cdot N \newline
&= 2^2 \cdot (2 \cdot T_{sort}(\frac{N}{8}) + c \cdot \frac{N}{4}) + 2 \cdot c \cdot N \newline
&= 2^3 \cdot T_{sort}(\frac{N}{4}) + 3 \cdot c \cdot N \newline
&= ... \newline
&= 2^K \cdot T_{sort}(\frac{N}{2^K}) + K \cdot c \cdot N \newline
&= N \cdot T_{sort}(1) + K \cdot c \cdot N \newline
&= N \cdot 1 + K \cdot c \cdot N \newline
&= N + c \cdot N \cdot \log_{ 2 }N
\end{aligned}
\\]

Thus, the time complexity is \\( \mathcal{O}(N \log N) \\).

## Implementation

<script src="https://gist.github.com/ChunMinChang/dee9f3bd2ceab69726373ae006016edb.js?file=merge_sort.cpp"></script>

## Appendix

- [Naive in-pace merge][nimerge]
<!-- - [In-pace merge sort with working area][wimsort] -->

## Reference

- [CS104][cs104]
- [COS226][cos226]
- [COMP250][comp250]
- [DSA1112][dsa1112]
- [CSC282][csc282]

[cs104]: http://www-bcf.usc.edu/~dkempe/CS104/11-07.pdf "2013 CS104: Recursive Sorting Algorithms and their Analysis"
[cos226]: http://www.cs.princeton.edu/courses/archive/spr07/cos226/lectures/04MergeQuick.pdf "Mergesort and Quicksort"
[comp250]: http://www.cs.mcgill.ca/~dprecup/courses/IntroCS/Lectures/comp250-lecture16.pdf "Lecture 16: MergeSort proof of correctness, and running time"
[dsa1112]: http://www.inf.unibz.it/~nutt/DSA1112/DSALabs/sols2.pdf "Data Structures and Algorithms"
[csc282]: https://www.cs.rochester.edu/~gildea/csc282/slides/C02-start.pdf "Getting Started"

[d&c]: https://en.wikipedia.org/wiki/Divide_and_conquer_algorithm "Divide and conquer algorithm"
[fib]: https://en.wikipedia.org/wiki/Fibonacci_number "Fibonacci number"

[eleAlg]: https://github.com/liuxinyu95/AlgoXY/releases/download/v0.618033/elementary-algorithms.pdf "Elementary Algorithms"

[nimerge]: naive_inplace_merge.md
