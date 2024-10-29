Big O notation:
1. 1 ; Constant
2. log(n) ; logarithmic
3. n ; linear
4. nlog(n) ; log-linear
5. $n^x$ ; Polynomial
6. $2^n$ ; exponential
7. n! ; factorial

Note that fetching data at an index is constant time

When "combining" statements, the most complex term will dominate (remember that "terms" are NOT separated by multiplication).
$n^xlog(n)$ can happen and THIS is what is dominating.

```Lisp
(dotimes (x n)
	(setf a 1)
	(setf b 2))
```
$\sum^{n-1}_{x=0}(1 + 1) = \sum^{n-1}_{x=0}(1) = O(n)$
Nested loops leads us to $O(n^2)$


#### Do Loop
```Lisp
(DO (var-definitions*)
	(end-test result-form)
	body-form*)
```
In which `var-definition` is a list: `(var init [step-form])`
\*Note that `*` means *0 or more* while `[...]` means form is *optional*

```Lisp
(do ((x 1 (+ x 1)))
	((>x n))
	(forms...)
	(forms...))
```
This is still just $O(n)$


Important Log rule:
$log_xx^a = a$
May sometimes need to "write out the math" to find the big O notation.

For,
```Lisp 
(do ((x 1 (* x 5)))
	((>= x n))
	(setf a 1))
```
Let P be our iteration number (starting at 0), $x=5^p$
$5^p >= n$
$log_5 5^p >= log_5n$
$p >= log_5n$ 
Therefore the number of iterations is going to be less than $log_5 n$ 

#### Dotimes
`Dotimes` is simpler than the `do`. It creates an iterator that starts at zero and it increments 1 each time.
```Lisp
(dotimes (i 7)
	(forms ...)
	(forms ...))
	
(dotimes (i 7 "Thing To Return")
	(forms ...))
```

![[CPS305DebugRoom-1.png]]
Note that $log_9n$ is *less* complex than $log_5n$


## Searching Algorithms
#### Linear search = Sequential search
- Just looking through each element one by one $O(n)$
#### Binary Search
- Only works on a sorted array
- Looking at the middle element, if not what you're looking for, eliminate half of the array
- Technically $O(log_2(n))$ but on the test it'll likely be $O(log(n))$


## Sorting Algorithms
The objective is to place elements in order given some comparison predicate.

 
**Stable Sorting** is defined by keeping elements in the same order if the comparison predicate deems them "equal"
**In-Place Sorting** modifies the existing array itself
**Online Sorting** doesn't require you to see the entire array to work


### Selection Sort
Look through the array to find the smallest element, swap the first element with the smallest element, this is now locked in place. Move forward and keep going.
Operations:
$\sum^{n-1}_{i=1}i=\frac{n(n-1)}{2} = \frac{1}{2}n^2 - \frac{1}{2}n$
Therefore, $O(n^2)$
Best and Worse case are $n^2$
```Lisp
;My selection Sort
(defun selectionSort (arr)
  (when (< (length arr) 2) (return-from selectionSort arr))
  (dotimes (i (1- (length arr))) ;i For each element (except the last)
    (let ((best (aref arr i)) (bestidx i))
      (do ((ii i (1+ ii))) ((= ii (length arr))) ;ii For elements i to end
        (when (< (aref arr ii) best)
          (setf best (aref arr ii))
          (setf bestidx ii))
        )
      (rotatef (aref arr i) (aref arr bestidx)))
    )
  arr)
```
### Insertion Sort
- *Online* (don't need to see the whole array)
If the next element is less than your current element, repeatedly swap it to the left until it is in the correct position.
If the next element is greater than your current element, continue moving to the right to check the next element
$\sum^{n}_{i=1}i = \frac{1}{2}n^2 + \frac{1}{2}n$
Therefore, $O(n^2)$ 
Best case: $O(n)$, worst case $O(n^2)$. Can be faster even though the running time is technically worse.


# Midterm Study (Outside of Debug Room)

### Merge Sort
note that `cdr` = `rest` and `car` = `first`

```Lisp
(defun merge-sort (list comp)
  (if (null (cdr list)) list ; If only one element long, return
      (let ((half (floor (length list) 2))) ;Find half of the list length
      ;Subseq gets first and second half
      ; Each are passed into merge-sort
        (merge-lists (merge-sort (subseq list 0 half) comp) 
                       (merge-sort (subseq list half) comp)
                       comp))))
        ;merge lists combines each of them

(defun merge-lists (l1 l2 comp)
  "Merges the sorted lists l1 and l2 and returns the sorted resulting list"
  (let ((res ()))
    (do ()             ; Notice: no var initializations in the do-loop
        ((and (null l1) (null l2)))     ; Notice: no result-form
      (let ((i1 (first l1))
            (i2 (first l2)))
        (cond ((null i1) (dolist (i l2) (push i res))
               (return))
              ((null i2) (dolist (i l1) (push i res))
               (return))
              ((funcall comp i1 i2) (push i1 res)
               (setf l1 (rest l1)))
              (t (push i2 res)
                 (setf l2 (rest l2))))))
    (reverse res)))

```

`(quote v) = 'v`
`(and ...)` evaluates forms until one returns NIL. `(and 1 2 3) ;=> 3`
`(or ...)` evaluates until one doesn't return NIL. `(or 10 2 3) ;=> 10`
`()` = `'()` = `'NIL` = `NIL`

#### Lambda
`(setf x (lambda (x) (* x x)))`
`(funcall x 12)`
`=>144`


## Data Structures

### Lists
Three list constructors (ways to make lists)
- Quote
	- `'(1 2 3)`
- Make-list
	- `(make-list 3)` -> `(nil nil nil)`
- List
	- `(list 1 2 3)` -> `(1 2 3)`

Use `dolist` to iterate over elements of a list!
Use `elt` to access specific elements of a list!
### Deque
(Double ended queue) (Used in "job-stealing algorithms")
Note to be confused with "Dequeue"
(PUSH-FRONT, PUSH-BACK, POP-FRONT, POP-BACK)
Can be implemented with 2 stacks; one "front" and one "back".
If I were to "POP FRONT" but "front" is empty, push all from "back" to front.

### Sets
- ELEMENT-OF-SET?
- INSERT-SET / REMOVE-SET
- SUBSET-SET
- UNION, INTERSECTION, DIFFERENCE, etc.
ELEMENT-OF-SET? can be implemented recursively:
```Lisp
(defun element-of-set? (x set)
	(when set 
		(if (equal x (car set)) set
			(element-of-set? x (cdr set)))))
```

### Abstract Data Type (Structures)
Can be made using arrays or linked lists (but likely just linked lists on the midterm)
Includes:
- Stacks
- Queues
- Deques
- Sets




### 3 laws of Recursion
1. Must have a base case
2. Must change its state toward the base case
3. Must call itself

**Tail recursion** typically involves an optional "accumulator" parameter. 

### Miscellaneous Lisp Things
**ash** is a form for bit-shifts (doubling or halving integers)
`(ash 5 -1) = 2`

REPL: Read-Eval-Print-Loop

Use `EQ` to compare symbols but `EQUAL` to compare everything else.

Note that `aref` can also be used on strings, and they'll return a literal character e.g. `#\a` 

`(subseq sequence start &optional end)` Useful for slicing lists!
`rtl:slice` = `subseq` 

`defstruct` automatically creates a "constructor", e.g:
```Lisp
(defstruct movie
	title director year type)

(defvar amovie (make-movie :title "Blade Runner" :director "Scott"))
(setf (movie-year amovie) 1982)
```

`(make-array 3)` -> `#(0 0 0)`
`(make-array 3 :initial-element 1/2)` -> `#(1/2 1/2 1/2)`
`:initial-contents` can be used and passed a list OR an array

`typep` is used to check if `type-of` matches something
`(type-of amovie)` -> `movie`
`(typep amovie 'movie` -> `t`

Surrounding a variable name with asterisks indicates its probably a *global variable*

things following a colon are "**keyword-arguments**"

## Other DSA things

#### The Von Neumann's Bottleneck
CPU has only can access data stored in registers, but bulk of data is stored in memory
The **von Neumann's bottleneck** limits flow of information between memory and CPU.

**Two ways to store data** in memory:
- **Contiguous structure** (block after block) 
	- Arrays (defined with `#`) and **structs (aka tuples)** (defstruct)
	- Constant time to access an element
- Linked structure (contains pointers to other places in memory)
	- lists, trees, graphs
	- Less efficient data access