---
title: "Emacs Lisp"
---

- open elisp shell by `M-x ielm`

A lisp

```commonlisp
(setq test "kittens")

(put 'test 'noun '(a buzzing little bug))
(put 'test 'verb 'transitive)

(defun test ()
  (message "puppies"))

(symbol-value 'test)    ; => "kittens"
(symbol-function 'test) ; => (lambda nil (message "puppies"))
(symbol-plist 'test)  ; => (function-history (nil (lambda nil (message "puppies"))) noun (a buzzing little bug) verb transitive)
```

  -------- ----- -------------------
  lambda   nil   (message puppies)
  -------- ----- -------------------
