- Functions, Lambda, Let, Let*, Progn
- Conditionals, Decision Structures
- Loops
- Arrays
- Lists, Misc.

### Lambda
Create a "one-line" anonymous procedure that receives all its inputs at the end of the form.

```Lisp
CL-USER> (defvar x (lambda (x y z) (+ x y (* z z))))
CL-USER> (funcall x 1 2 3)
12
```

Let and Let* variables only exist within the form. Very common to use at the beginning of a function!

`progn` just lets us "squeeze" a bunch of forms into the "space" of one.

`dotimes` acts like a "for loop" while `do` acts like a "while loop"

