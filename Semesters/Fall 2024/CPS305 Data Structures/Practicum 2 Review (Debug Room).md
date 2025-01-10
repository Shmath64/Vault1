`(funcall comp element-1 element-2)`
Note the lack of brackets inside funcall

## Higher-Order Function
```Lisp
(defun getList (mylist validator)
   (let ((result '()))
	 (dolist (curr myList)
	   (if (funcall validator curr)
		  (setf result (cons curr result)))) ; Can be done with push
	   (reverse result)))

> (getlist '(0 1 2 3 4 5) 'oddp) 
; The quote before `oddp` is needed so that it its a reference to the function
(1 3 5)
> (getlist '("R" "a" "s" "D" "R") #'(lambda (c) (string= c "R")))
;This NEEDS the `#` because `#'` is for function objects. ()
("R" "R")
```

Note that using `#'` is the best way to pass functions!
## Recursive Functions
```Lisp
(defun insert-node (alist k value &optional (acc ()) (count 1))
   (cond ((= k count) (append (reverse acc) (list value) alist)) ;Append
	 ((null alist) ()) ;If alist nil, nil
	 ((> k (+ (length alist) (length acc))) alist) ; If k greater than alist
	 (t (insert-node (cdr alist) k value (cons (car alist) acc) (1+ count)))
	)
)

> (insert-node '(1 2 3 4 5 6 7) 3 'a)
'(1 2 A 3 4 5 6 7)

(defun getListR (mylist validator &optional (acc '()))
   ;We know we need to:
   ; Funcall validator on the first
   ; Identify base case
   ; Recursive call moving towards the base case
   (cond ((null mylist) (reverse acc)) ;base case
		 (t (if (funcall validator (car mylist)) (push (car mylist) acc))
			(getlistR (cdr mylist) validator acc)))
   )
> (getlistr '(0 1 2 3 4 5 6 7 8) #'oddp)
(1 3 5 7)
```

Factorial:
```Lisp
;Normal recursive
(defun fact (x)
	(if (< x 3)
		x
		(* x (fact (1- x)))))

;Tail recursive:
(defun trFact (x &optional (acc 1))
	(if (< x 2)
		acc
		(trFact (- x 1) (* acc x))))
```

`typep` is a useful symbol for checking the type of things:
```Lisp
(typep 12 'integer)
> T
```

Some interesting `typep` behaviour:
```Lisp
CL-USER> (defparameter x '(1 2 3))
X
CL-USER> (typep (car x) 'integer)
T
CL-USER> (typep (car x) 'bit)
T
CL-USER> (typep (car x) 'number)
T
```

Use `nth` to get the element `nth` index of a list

Remember that for a do-loop, all the variables and they're iteration functions must be in brackets, and the test condition must also be in brackets:
```Lisp
(defun print-squares-of-3s (mylist start stop)
	(do ))
```

## EMACS Commands:
`C+Space` to highlight
`Alt + W` to 'copy'
`C + Y` to 'paste/yank'
