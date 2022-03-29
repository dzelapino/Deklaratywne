# Deklaratywne

notuje stuff z wykladu

```rkt
input w konsoli: >
output w konsoli: ->
```

## wyklad 1

to jest taki jezyk

## wyklad 2

rekurencja robi brr

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
