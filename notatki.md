# Deklaratywne

## wyklad 1

nic

## wyklad 2

rekurencja

## wyklad 3

(sum term next a b)         term next musi byc funkjca
(sum (lambda(x) x)
    (lambda (x) (+ x 2))
    2
    10)

Funkcje jako wartosci innych funkcji

fa(x) := x^2 + a        (f a)(x) = x^2 + a

(define (f a)
    (lambda (x) (+ (* x x) a))
)

> (f 5)
->    (lambda(x)(+ (*x x) 5))

<!--^^tu bedzie blad^^-->  

> ((f 5) 2)
->    (lambda (x) (+ (*x x) 5) 2)
->    (+ (* 2 2) 5)
->    9

<!-- ^^tu mamy funkcje i argument do owej funkcji -->

<!-- pochodna -->
Df(x) = f'(x)
D f(x) = (f(x + dx) - f(x)) /dx

(define (derive f dx)
  (lambda (x)
    (/ (- f (+ x dx) (f x)) dx)
  )
)

(define (cube x)
  (* x x x))

> ((derive cube 0.001) 5)
-> 75.015

### listy

cons, car, cdr

> (cons 1 2)
->   (1 . 2)
> (car (1 . 2))
-> 1
> (cdr (1 . 2))
-> 2
> (cons 1 (cons 2 (cons 3 '())))
-> (1 2 3)              (1 . (2 . (3 . '())))
> (list 1 2 3)
-> (1 2 3)
> (car '(1 2 3))
-> 1
> (cdr '(1 2 3))
-> (2 3)
> (list 1 (cons 2 3) 7)
-> (1 (2 . 3) 7)

(define (length e)
  (if (null? e)
    0
    (+ 1 (length (cdr e)))
  )
)

> (length (1 2 3))
-> 3
<!-- z to ta tablica z gory  (list 1 (cons 2 3) 7) o ta i te pare bierze jako 1-->
> (length z)
-> 3
> (length (list 1 2 3))
-> 3
<!-- tu bedzie myslal ze to jest funkcja -->
> (length (1 2 3))
> (length (a b c))
<!--  dlatego trzeba napisac pypcia przed nawiasem i wtedy wie ze to lista -->
> (length '(1 2 3))
> (length '(a b c))
<!-- pypcia mozna napisac slownie badz symbolem -->
> (quote (a b c))
-> (a b c)

(define (append e1 e2)
  (if (null? e1)
    e2
    (cons (car e1) (append (cdr e1) e2))
  )
)

(define (sil n)
  (if (zero? n)
    1
    (* n (sil (- n 1)))
  )
)

(define (member? x l)
  (cond ((null? l)
    #f)
    ((eq? (car l) x ) #t)
    (else (member? x (cdr l)))
  )
)

(define (mem? x l)
  (if (null? l)
    #f
    (or (eq? x (car l))
          (mem? x (cdr l))
    )
  )
)

> (member? 'a '(a b c a))
-> #t
> (member? 'a '(b (a b a) c))
-> #f
