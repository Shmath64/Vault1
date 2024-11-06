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
