# Naive in-place merge

## Correctness

\\[
\begin{aligned}
& naiveInplaceMerge(L, l, m, r): \newline
& \space \space \space \space \text{for } i \leftarrow m+1 \text{ to } r: \newline
& \space \space \space \space \space \space \space \space \text{for } j \leftarrow i \text{ down to } l+1: \newline
& \space \space \space \space \space \space \space \space \space \space \space \space \text{if } L[j-1] \leq L[j]: \newline
& \space \space \space \space \space \space \space \space \space \space \space \space \space \space \space \space \text{break} \newline
& \space \space \space \space \space \space \space \space \space \space \space \space \text{swap } L[j-1] \text{ and } L[j]
\end{aligned}
\\]

### Lemma 1

> Given a sorted list \\( A = [a_1, a_2, ..., a_N] \\) with \\( N \\) elements,
> where \\( a_1 \leq a_2 \leq ... \leq a_N \\),
> and one value \\( x \\),
> the list \\( L = A \cup [x] = [a_1, a_2, ..., a_N, x] \\)
> (\\( x \\) is appended to the end of list \\( A \\)),
> can be sorted by the _naive in-place merge_ method with \\( l = 1, m = N, r = N + 1 \\).

- Base step: When \\( N = 0 \\), list \\( L = [x] \\) is trivially true
- Induction Hypothesis: Suppose this assumption holds when \\( N = k \\)
- Induction Step: When \\( N = k + 1 \\)
  - If \\( x \geq L[k] \\), then the \\( L = a_1 \leq a_2 \leq ... \leq a_k \leq x \\) is naturally sorted
  - Otherwise, \\( x < L[k] \\) and the \\( x \\) and \\( L[k] \\) are swapped.
    - Now \\( L[1...k] = [a_1, a_2, ... , a_{k-1}, x] \\)
    - By the hypothesis, the _naive in-place merge_ works when \\( N = k \\), so we can a sorted \\( L[1...k] \\)
    - Thus, the list \\( L \\) now is sorted since \\( L[1...k] \\) is sorted
      and all its elements are smaller than the current \\( (k+1) \\)th element \\( L[k] \\)

### Lemma 2

> Given a sorted list \\( A = [a_1, a_2, ..., a_N] \\) with \\( N \\) elements
> where \\( a_1 \leq a_2 \leq ... \leq a_N \\),
> and \\( B = [b_1, b_2, ..., b_M] \\) with \\( M \\) elements
> where \\( b_1 \leq b_2 \leq ... \leq b_M \\),
> the list \\( L = A \cup B = [a_1, a_2, ..., a_N, b_1, b_2, ..., b_M] \\)
> can be sorted by the above _naive in-place merge_ method with \\( l = 1, m = N, r = M+N \\).

- Base step: When \\( M = 1 \\), the condition is same as _Lemma 1_, so it's true
- Induction Hypothesis: Suppose this assumption holds when \\( M = k \\)
- Induction Step: When \\( M = k + 1 \\)
  - When \\( i = N + 1 \\)
    - the element \\( L[N+1] \\) will be merged with \\( L[1...N] = A \\)
    - then the list \\( L[1...N+1] \\) is sorted by _Lemma 1_
  - When \\( i = N + 2 \\)
    - the list is composed by sorted sublists \\( A^\prime = L[1...N+1] \\)
      and \\( B^\prime = L[N+2...N+k+1] \\) with \\( k \\) elements
    - By the hypothesis, the _naive in-place merge_ works when \\( \vert B^\prime \vert = k \\)
    - Thus, the list \\( L = A^\prime \cup B^\prime = A \cup B \\) is sorted

### Complexity

\\( \mathcal{O}(N^2) \\)
