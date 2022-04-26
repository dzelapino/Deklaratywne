# Deklaratywne

notuje stuff z wykladu

```rkt
input w konsoli: >
output w konsoli: ->
```

## wyklad 1

```s
(define (nwd x y)
  (if (= x y)
      x
      (if (> x y)
          (nwd (- x y) y)
      (nwd x (- y x)))))

#|(nwd 30 20)|#

(define (nww x y)
  (if (/ (* x y) (nwd x y))
      (/ (* x y) (nwd x y))
      1))

(nww 12 32)

(define (new.> x y)
  (> x y))

(define (new.< x y)
  (< x y))

(define (new.= x y)
  (not (or (> x y) (> y x))))

(define (new.<> x y)
  (or (> x y) (> y x)))

(define (new.<= x y)
  (not (> x y)))

(define (new.>= x y)
  (not (< x y)))

(define (same-values? p1 p2 x y)
  (if (equal? (p1 x y) (p2 x y))
  #t
  #f)
)

(same-values? new.<= < 6 4)
```

## wyklad 2

### silnia

```s
(define (factorialRecursive x)
  (if (= x 0)
  1
  (* x (factorialRecursive(- x 1)))))

(factorialRecursive 3)

(define (factorialAcc x)
  (define (factorialHelper x acc)
    (if (> x 1)
    (factorialHelper (- x 1) (* acc x))
    acc))
    (factorialHelper x 1)
)

(factorialAcc 3)
```

### ciag bonifacego

```s
(define (bonifacy x)
(if (not (> x 1))
x
(+ (bonifacy (- x 1)) (bonifacy (- x 2)))))

(bonifacy 10)

(define (bonifacyAcc x)
  (define (bonifacyHelper x c1 c2)
    (cond ((= x 0) c1)
          ((= x 1) c2)
          (else (bonifacyHelper (- x 1) c2 (+ c1 c2))))
  )
  (bonifacyHelper x 0 1)
)

(bonifacyAcc 10)
```

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

```rkt
>  (f 5)
-> (lambda(x)(+ (*x x) 5))
```

^^tu bedzie blad^^  

```rkt
>   ((f 5) 2)
->  (lambda (x) (+ (*x x) 5) 2)
->  (+ (* 2 2) 5)
->  9
```

^^tu mamy funkcje i argument do owej funkcji^^

pochodna
Df(x) = f'(x)
D f(x) = (f(x + dx) - f(x)) /dx

(define (derive f dx)
  (lambda (x)
    (/ (- f (+ x dx) (f x)) dx)
  )
)

(define (cube x)
  (* x x x))

```rkt
>  ((derive cube 0.001) 5)
-> 75.015
```

### listy

cons, car, cdr

```rkt
> (cons 1 2)
-> (1 . 2)

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
```

(define (length e)
  (if (null? e)
    0
    (+ 1 (length (cdr e)))
  )
)

```rkt
> (length (1 2 3))
-> 3
```

z = (list 1 (cons 2 3) 7) pare ( 2 3 ) bierze jako 1 element

```rkt
> (length z)
-> 3

> (length (list 1 2 3))
-> 3

tu bedzie myslal ze to jest funkcja
> (length (1 2 3))
> (length (a b c))

 dlatego trzeba napisac pypcia przed nawiasem i wtedy wie ze to lista

> (length '(1 2 3))
> (length '(a b c))

pypcia mozna napisac slownie badz symbolem

> (quote (a b c))
-> (a b c)
```

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

```rkt
> (member? 'a '(a b c a))
-> #t

> (member? 'a '(b (a b a) c))
-> #f
```

## wyklad 4

### drzewa binarne

Puste drzewo:

```rkt
'()
```

Drzewo:

```rkt
        (x)
      /     \
    (l)     (t)
```

'(x l r)

```rkt
        (2)
      /     \
    (3)     (1)
   /   \    / \
 ()   (4) ()   ()
      / \
     ()  ()
```

'(2 (3 () (4 () () ) ) (1 () () ) )

```rkt
  (define (height t)

      (if (eq? t '())
        0
        (+ 1 (max (height (car (cdr t)))
                        (height (car (cdr (cdr t))))
        ))
      )

  )
```

skrocone formy zapisu:

(car (cdr (cdr t))) -> (caddr t)

(car (cdr t)) -> (cadr t)

```rkt
(define (left t)

  (cadr t)

)

(define (right t)

  (caddr t)

)

(define (is-null? t)

  (eq? t '())
)

```

```rkt
(define (height t)

  (if (is-null? t)
    0
    (+ 1 (max (height (left t))
                    (height (right t))
    ))
  )

)
```

```rkt
(define (element t) (car t))
```

Kolejnosc prefikoswa: od lewych do prawych

```rkt
        (2)
      /     \
    (3)     (1)
   /   \    / \
 ()   (4) ()   ()
      / \
     ()  ()
```

`(2 3 4 1)

```rkt
(define (preorder t)

  (if (is-null? t)
    '()
  (con (element t)
   (append (preorder (left t))
    (preorder(right t))))

  )

)
```

```rkt
        (2)
      /     \
    (3)     (1)
   /   \    / \
 ()   (4) ()   ()
      / \
     ()  ()

-wymnazamy przez 5-

        (10)
      /     \
    (15)     (5)
   /   \    / \
 ()   (20) ()   ()
      / \
     ()  ()

(define (times n t)

  (if (is-null? t )
    '()
   (make-tree (* n (element t))
    (times n (left t))
    (times n (right t)))

  )
)

(define (make-tree root left right)

  (list root left right)

)
```

### symboliczne pochodne

```txt
dc/dx = 0, c stała lub c = y != x

dx/dx = 1

d(u + v)/dx = du/dx + dv/dx

d(u * v)/dx = v * du/dx  + u * dv/dv
```

```rkt
(define (derive exp var)

  (cond ((constant? exp) 0)
      ((variable? exp)
        (if (same-variable? exp var) 1 0))
      ((sum? exp)
        (make-sum  (derive (addend exp) var)
                    (derive (augment exp) var)))
      ((pred? exp)
        (make-sum (make-product ....)
                        (make-product ....)))
    )
  )
```

#### reprezentacja wyrazen

```txt
a * x + b

a * x * y + b * x
```

```rkt
'(+ (* a x) b)

(define (constant? exp) (number? exp))

(define (variable? exp) (symbol? exp))

(define (same-variables? x y)
  (and (eq? x y)
  (variable? x) (variable? y))
)

(define (sum? exp)
  (if (list? exp)
      (eq? (car exp) '+)
      (error "......."))
)

(define (addend exp)
  (cadr exp)
)

(define (augment exp)
  (caddr exp)
)

(define (make-sum x y)
  (list '+ x y)
)
```

```rkt
>   (derive '(+ (* a x) b) 'x)
->  ( + ( + ( * a 1) (* x 0)) 0 )

>   (derive '(x * y) 'x)
->  (+ (* x 0) (* 1 y))
```

```txt
d(x * y')/dx = y * dx/dx + x * dy/dx

(+ (* y 1) .... )
```

## wyklad 5

### list comprehension

sumowanie wszystkich elementow w liscie
(define (sum l)

  (if (null? l)
    0
    (+ (car l) (sum  (cdr l)))
  )

)

mnozenie wszystkich elementow w liscie

(define (prod l)

  (if (null? l)
    1
    (* (car l) (prod  (cdr l)))
  )

)

(define (fold f e l)

  (if (null? l)
    e
    (f (car l) (fold f e (cdr l)))
  )

)

e mowi nam co zadzieje sie w wypadku gdy lista bedzie pusta
f jest argumentem funkcyjnym

```rkt
> (fold + 0 '(1 2 3 4))
-> 10

> (fold * 1 '(1 2 3 4))
-> 24

> (fold * 3 '(1 2 3 4))
-> 72
```

(define (sum l) (fold + 0 l))
(define (frod l) (fold * 1 l))

Append uzywajac fold
(define (fold cons l1 l2)

(if (null? l2)
    l1
    (cons (car l2) (fold cons l1 (cdr l2)))
  )

)

Szalony sposob na uzycie folda z lambda

```rkt
(define (fold f e)

  (lambda (l)
  
    (if (null? l)
    
      e
      (f (car l)((fold f e)(cdr l)))
    
    )

  )

)
```

```rkt
> ((fold + 0) '(1 2 3 4))
-> 10
> ((fold * 1) '(1 2 3 4))
-> 24
```

### rozdział o tym czym są dane?

#### liczby zespolone

ogolny schemat sumowania i mnozenia liczb zespolonych

```rkt
(define (+c z1 z2)

    (make-rectangular
    
      (+ (real-part z1) (real-part z2))
      (+ (imag-part z1) (imag-part z2))
    
    )

)

(define (-c z1 z2)

    (make-rectangular
    
      (- (real-part z1) (real-part z2))
      (- (imag-part z1) (imag-part z2))
    
    )

)

(define (*c z1 z2)

  (make-polar
  
    (* (magnitude z1) (magnitude z2))
    (+ (angle z1) (angle z2))
  
  )

)

(define (/c z1 z2)

  (make-polar
  
    (/ (magnitude z1) (magnitude z2))
    (- (angle z1) (angle z2))
  
  )

)
```

Manifest types <=> Informacja o reprezentacji
(rectangular. (1. 5))
(polar. (7. 3))

(define (attach-type type contents) (cons type contents))

(define (type datum) (car datum))

(define (contents datum) (cdr datum))

(define (rectangular?) (eq? (type z) 'rectangular))

(define (polar?) (eq? (type z) 'polar))

(define (make-rectangular x y)
  (attach-type 'rectangular (cons x y))
)

(define (make-polar x y)
  (attach-type 'polar (cons x y))
)

(define (real-part z)
  (cond (
    (rectangular? z)
      (real-part-rectangular (contents z))
  )
    ((polar? z)
      (real-part-polar (contents z))
    )
  )
)

(define (imag-part z)
  (cond (
    (rectangular? z)
      (imag-part-rectangular (contents z))
  )
    ((polar? z)
      (imag-part-polar (contents z))
    )
  )
)

(define (magnitude z)
  (cond ((rectangular? z)
    (magnitude-rectangular (contents z)))
  ((polar? z)
    (magnitude-polar (contents z)))
  )
)

(define (angle z)
    (cond ((rectangular? z)
  (angle-rectangular (contents z)))
    ((polar? z)
  (angle-polar (contents z))))
)

(define (real-part-rectangular z) (car z))

(define (imag-part-rectangular z) (cdr z))

(define (magnitude-rectangular z)
  (sqrt (+ (square (car z)) (square (cdr z)))))

```rkt
(define (make-rectangular x y)
  (define (dispatch m)
    (cond ((eq? m 'real-part) x)
      ((eq ? q 'imag-part) y)
      ((eq? m 'magnitude) (sqrt (+ (square x) (square y))))
      ((eq? m 'angle) (atan y x))
      (else (error "Unknown message in make-rectangular: " m)))))

(define (make-polar x y)
  (define (dispatch m)
    (cond ((eq? m 'real-part) (* x (cos y)))
      ((eq ? q 'imag-part) (* x (sin y)))
      ((eq? m 'magnitude) x)
      ((eq? m 'angle) y)
      (else (error "Unknown message in make-rectangular: " m)))))

(define (real-part obj) (obj 'real-part))
(define (imag-part obj) (obj 'imag-part))
(define (magnitude obj) (obj 'magnitude))
(define (angle obj) (obj 'angle))
```

#### Pary

```txt
[ ][ ]
 |  |
 v  v
 x y
```

Definicja par
(car (cons x y)) = x
(cdr (cons x y)) = y
(cons (car z) (cdr z)) = z

(define (new-cons x y)
  (define (dispatch m)
    (cond ((= m 0) x)
      ((= m 1) y)
      (else (error "..."))))
    dispatch)

(define (new-car z) (z 0))
(define (new-cdr z) (z 1))

## wyklad 6

### kolejki

```txt
<----- [ 2 | 3 | .... ] <----

pop(q) = [3 | ...]
push(7) = [2 | 3 | .... | 7 ]
top(q) = 2

pop(q)  = [3 | ...]
pop(q)  = [3 | ...]

nie mamy wymodelowanej histori wiec usunieta zostala znowu 2

```

w scheme

```s

(set! x w)

```

Przykład: konto

```s

> (withdraw 30)
-> 70
> (withdraw 30)
-> 40

(define balance 100)

(define (withdraw ammount)
  (if (=> balance ammount)
    (begin (set! balance (- balance ammount))
       balance)
    (error "za malo $$$")))


(define (make-withdraw balance)
  (define (dispatch ammount)
    (if (>= ....)
      .
      .
      .
    ))
  dispatch)
```

### rozszerzenie modelu ewaluacji (model srodowisk)

zeby ewaluowac funkcje f,  stworzymy nowe srodowisko, w ktorym wartosci formalne parametrow sa aktualnymi parametrami

Nastepca srodowiska
jest to srodowisko w ktorym nasza funkcja f zostala zdefiniowana, to co zostaje w nowym srodowisku ewaluujemy cialo funkcji f

```s
> (square (square 3))
-> (square 9)
-> 81
```

rysunki z tego sa na jego stronie: <https://inf.ug.edu.pl/~schwarzw/ProgDekl-Rozd5.pdf>

