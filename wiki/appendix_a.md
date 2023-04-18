!!Polyglot
#page(0)

# Appendix A: Scheme

#Quote( Programming languages should be designed not by piling feature on top of feature, but by removing the weaknesses and restrictions that make additional features appear necessary. Scheme demonstrates that a very small number of rules for forming expressions, with no restrictions on how they are composed, suffice to form a practical and efficient programming language that is flexible enough to support most of the major programming paradigms in use today.)
#Caption IEEE Standard for the Scheme Programming Language [10](references!bib_10), p. 3)

Here we give an elementary introduction to Scheme.#Footnote(1) For a more precise explanation of the language see the IEEE standard [10](references!bib_10); for a longer introduction see the textbook [1](references!bib_1).

Scheme is a simple programming language based on expressions. An expression names a value. For example, the numeral #Code(3.14) names an approximation to a familiar number. There are primitive expressions, such as a numeral, that we directly recognize, and there are compound expressions of several kinds.

### Procedure Calls

A /procedure call/ is a kind of compound expression. A procedure call is a sequence of expressions delimited by parentheses. The first subexpression in a procedure call is taken to name a procedure, and the rest of the subexpressions are taken to name the arguments to that procedure. The value produced by the procedure when applied to the given arguments is the value named by the procedure call. For example,

```Scheme
(+ 1 2.14)
;; 3.14
  ```

```Scheme
(+ 1 (* 2 1.07))
;; 3.14
  ```

are both compound expressions that name the same number as the numeral #Code(3.14).#Footnote(2) In these cases the symbols #Code(+) and #Code(#*) name procedures that add and multiply, respectively. If we replace any subexpression of any expression with an expression that names the same thing as the original subexpression, the thing named by the overall expression remains unchanged. In general, a procedure call is written
$$\begin{equation}
(\quad \textit{operator} \quad \textit{operand-1} \quad \ldots \quad \textit{operand-n} \quad )
\end{equation}$$
where /operator/ names a procedure and /operand-i/ names the /i/th argument.#Footnote(3)

### Lambda Expressions

Just as we use numerals to name numbers, we use $\lambda$-expressions to name procedures.#Footnote(4) For example, the procedure that squares its input can be written:

```Scheme
(lambda (x) (* x x))
```

This expression can be read: "The procedure of one argument, $x$, that multiplies $x$ by $x$." Of course, we can use this expression in any context where a procedure is needed. For example,

```Scheme
((lambda (x) (* x x)) 4)
;; 16
```

The general form of a $\lambda$-expression is
$$\begin{equation}
\texttt{(lambda} \quad \textit{formal-parameters} \quad \textit{body} \texttt{)}
\end{equation}$$
where /formal-parameters/ is a list of symbols that will be the names of the arguments to the procedure and /body/ is an expression that may refer to the formal parameters. The value of a procedure call is the value of the body of the procedure with the arguments substituted for the formal parameters.

### Definitions

We can use the define construct to give a name to any object. For example, if we make the definitions#Footnote(5)

```Scheme
(define pi 3.141592653589793)

(define square (lambda (x) (* x x)))
```

we can then use the symbols #Code(pi) and =square wherever the numeral or the $\lambda$-expression could appear. For example, the area of the surface of a sphere of radius 5 meters is

```Scheme
(* 4 pi (square 5))
;; 314.1592653589793
```

Procedure definitions may be expressed more conveniently using "syntactic sugar." The squaring procedure may be defined

```Scheme
(define (square x) (* x x))
```

which we may read: "To square /x/ multiply /x/ by /x/."

In Scheme, procedures may be passed as arguments and returned as values. For example, it is possible to make a procedure that implements the mathematical notion of the composition of two functions:#Footnote(6)

```Scheme
(define compose
(lambda (f g)
 (lambda (x)
   (f (g x)))))

((compose square sin) 2)
;; .826821810431806
```

```Scheme
(square (sin 2))
;; .826821810431806
```

Using the syntactic sugar shown above, we can write the definition more conveniently. The following are both equivalent to the definition above:

```Scheme
(define (compose f g)
(lambda (x)
 (f (g x))))

(define ((compose f g) x)
(f (g x)))
```

### Conditionals

Conditional expressions may be used to choose among several expressions to produce a value. For example, a procedure that implements the absolute value function may be written:

```Scheme
(define (abs x)
(cond ((< x 0) (- x))
     ((= x 0) x)
     ((> x 0) x)))
```

The conditional #Code(cond) takes a number of clauses. Each clause has a predicate expression, which may be either true or false, and a consequent expression. The value of the #Code(cond) expression is the value of the consequent expression of the first clause for which the corresponding predicate expression is true. The general form of a conditional expression is

$$\begin{align*}
\texttt{(cond }&\texttt{( } \textit{predicate-1}\quad \textit{consequent-1} \texttt{)} \\
&\ldots \\
&\texttt{( } \textit{predicate-n}\quad \textit{consequent-n} \texttt{))}
\end{align*}$$

For convenience there is a special predicate expression #Code(else) that can be used as the predicate in the last clause of a #Code(cond). The #Code(if) construct provides another way to make a conditional when there is only a binary choice to be made. For example, because we have to do something special only when the argument is negative, we could have defined #Code(abs) as:

```Scheme
(define (abs x)
(if (< x 0)
   (- x)
   x))
```

The general form of an #Code(if) expression is
$$\begin{equation}
\texttt{(if} \quad \textit{predicate} \quad \textit{consequent} \quad \textit{alternative} \texttt{)}
\end{equation}$$
If the /predicate/ is true the value of the #Code(if) expression is the value of the /consequent/, otherwise it is the value of the /alternative/.

### Recursive Procedures

Given conditionals and definitions, we can write recursive procedures. For example, to compute the $n$th factorial number we may write:

```Scheme
(define (factorial n)
(if (= n 0)
   1
   (* n (factorial (- n 1)))))

(factorial 6)
;; 720
```

```Scheme
(factorial 40)
;; 815915283247897734345611269596115894272000000000
```

### Local Names

The #Code(let) expression is used to give names to objects in a local context. For example,

```Scheme
(define (f radius)
(let ((area (* 4 pi (square radius)))
     (volume (* 4/3 pi (cube radius))))
 (/ volume area)))

(f 3)
;; 1
```

The general form of a #Code(let) expression is

$$\begin{align*}
\texttt{(let (}&\texttt{( } \textit{variable-1}\quad \textit{expression-1} \texttt{)} \\
&\ldots \\
&\texttt{( } \textit{variable-n}\quad \textit{expression-n} \texttt{))} \\
\qquad \textit{body} \texttt{)}
\end{align*}$$

The value of the #Code(let) expression is the value of the /body/ expression in the context where the variables /variable-i/ have the values of the expressions /expression-i/. The expressions /expression-i/ may not refer to any of the variables.

A slight variant of the #Code(let) expression provides a convenient way to express looping constructs. We can write a procedure that implements an alternative algorithm for computing factorials as follows:

```Scheme
(define (factorial n)
(let factlp ((count 1) (answer 1))
 (if (> count n)
     answer
     (factlp (+ count 1) (* count answer)))))

(factorial 6)
;; 720
```

Here, the symbol #Code(factlp) following the #Code(let) is locally defined to be a procedure that has the variables #Code(count) and #Code(answer) as its formal parameters. It is called the first time with the expressions 1 and 1, initializing the loop. Whenever the procedure named #Code(factlp) is called later, these variables get new values that are the values of the operand expressions #Code((+ count 1#)) and #Code((#* count answer#)).

### Compound Data --- Lists and Vectors

Data can be glued together to form compound data structures. A list is a data structure in which the elements are linked sequentially. A Scheme vector is a data structure in which the elements are packed in a linear array. New elements can be added to lists, but to access the $n$th element of a list takes computing time proportional to $n$. By contrast a Scheme vector is of fixed length, and its elements can be accessed in constant time. All data structures in this book are implemented as combinations of lists and Scheme vectors. Compound data objects are constructed from components by procedures called constructors and the components are accessed by selectors.

The procedure #Code(list) is the constructor for lists. The selector #Code(list-ref) gets an element of the list. All selectors in Scheme are zero-based. For example, 

```Scheme
(define a-list (list 6 946 8 356 12 620))

a-list
;; (6 946 8 356 12 620)
```

```Scheme
(list-ref a-list 3)
;; 356
```

```Scheme
(list-ref a-list 0)
;; 6
```

Lists are built from pairs. A pair is made using the constructor #Code(cons). The selectors for the two components of the pair are #Code(car) and #Code(cdr) (pronounced "could-er").#Footnote(7) A list is a chain of pairs, such that the #Code(car) of each pair is the list element and the #Code(cdr) of each pair is the next pair, except for the last #Code(cdr), which is a distinguishable value called the empty list and is written #Code((#)). Thus,

```Scheme
(car a-list)
;; 6
```

```Scheme
(cdr a-list)
;; (946 8 356 12 620)
```

```Scheme
(car (cdr a-list))
;; 946
```

```Scheme
(define another-list
(cons 32 (cdr a-list)))

another-list
;; (32 946 8 356 12 620)
```

```Scheme
(car (cdr another-list))
;; 946
```

Both #Code(a-list) and #Code(another-list) share the same tail (their #Code(cdr)).

There is a predicate #Code(pair?) that is true of pairs and false on all other types of data.

Vectors are simpler than lists. There is a constructor #Code(vector) that can be used to make vectors and a selector =vector-ref for accessing the elements of a vector:

```Scheme
(define a-vector
(vector 37 63 49 21 88 56))

a-vector
;; #(37 63 49 21 88 56)
```

```Scheme
(vector-ref a-vector 3)
;; 21
```

```Scheme
(vector-ref a-vector 0)
;; 37
```

Notice that a vector is distinguished from a list on printout by the character $\#$ appearing before the initial parenthesis.

There is a predicate #Code(vector?) that is true of vectors and false for all other types of data.

The elements of lists and vectors may be any kind of data, including numbers, procedures, lists, and vectors. Numerous other procedures for manipulating list-structured data and vector-structured data can be found in the Scheme
online documentation.

### Symbols

Symbols are a very important kind of primitive data type that we use to make programs and algebraic expressions. You probably have noticed that Scheme programs look just like lists. In fact, they are lists. Some of the elements of the lists that make up programs are symbols, such as #Code(+) and #Code(vector).#Footnote(8) If we are to make programs that can manipulate programs, we need to be able to write an expression that names such a symbol. This is accomplished by the mechanism of /quotation/. The name of the symbol #Code(+) is the expression #Code('+), and in general the name of an expression is the expression preceded by a single quote character. Thus the name of the expression #Code((+ 3 a#)) is #Code('(+ 3 a#)).

We can test if two symbols are identical by using the predicate #Code(eq?). For example, we can write a program to determine if an expression is a sum:

```Scheme
(define (sum? expression)
(and (pair? expression)
    (eq? (car expression) '+)))
(sum? '(+ 3 a))
;; #t
```

```Scheme
(sum? '(* 3 a))
;; #f
```

Here #Code(#t) and #Code(#f) are the printed representations of the boolean values true and false.

Consider what would happen if we were to leave out the quote in the expression #Code((sum? '(+ 3 a#)#)). If the variable #Code(a) had the value 4 we would be asking if 7 is a sum. But what we wanted to know was whether the expression #Code((+ 3 a#)) is a sum. That is why we need the quote.

----
### Footnotes

#FootnoteRef(1) Many of the statements here are valid only assuming that no assignments are used.
#FootnoteRef(2) In examples we show the value that would be printed by the Scheme system using slanted characters following the input expression.
#FootnoteRef(3) In Scheme every parenthesis is essential: you cannot add extra parentheses or remove any.
#FootnoteRef(4) The logician Alonzo Church [5](references!bib_5) invented $\lambda$-notation to allow the specification of an anonymous function of a named parameter:
$\boldsymbol{\lambda}x[\text{expression in } x]$. This is read, "That function of one argument that is obtained by substituting the argument for x in the indicated expression."
#FootnoteRef(5) The definition of #Code(square) given here is not the definition of =square in the Scmutils system. In Scmutils, #Code(square) is extended for tuples to mean the sum of the squares of the components of the tuple. However, for arguments that are not tuples the Scmutils square does multiply the argument by itself.
#FootnoteRef(6) The examples are indented to help with readability. Scheme does not care about extra white space, so we may add as much as we please to make things easier to read.
#FootnoteRef(7) These names are accidents of history. They stand for "Contents of the Address part of Register" and "Contents of the Decrement part of Register" of the IBM 704 computer, which was used for the first implementation of Lisp in the late 1950s. Scheme is a dialect of Lisp.
#FootnoteRef(8)  Symbols may have any number of characters. A symbol may not contain whitespace or a delimiter character, such as parentheses, brackets, quotation marks, comma, or $\#$.
#FootnoteEnd

