<div id="top"></div>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h1 align="center">Racket Symbolic Differentation</h1>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About The Project</a></li>
    <li><a href="#built-with">Built With</a></li>
    <li><a href="#getting-started">Getting Started</a></li>
    <li><a href="#functionality-and-examples">Functionality and Examples</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

An implementation of a Symbolic Differentatior in Racket.
Consists of the following: 
- Differentation expressions 
- Simplifying expressions
- Mathmatical operations 
- Output presentation 
- Error handling

<p align="right">(<a href="#top">back to top</a>)</p>



## Built With

* [Racket](https://racket-lang.org/)

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- GETTING STARTED -->
## Getting Started

You can test the project by running the code and using the functions
Please keep in mind that if you use Internal functions or predicates you need to make sure that your input is valid beforehand


<p align="right">(<a href="#top">back to top</a>)</p>


<!-- Functionality and Examples -->
## Functionality and Examples

#### Parameters and Input criteria


For all functions and predicates we expect valid input for each parameter:
- expr: an expression paramter that should be in valid racket notation
- respect/respects: a list or symbol that consist of the variable/s we should derive for
- operator: a symbol that is a supported operator
- str: a string
- var: a variable or a number
- expected-expr/expected-form/actual-expr: parameters related to error handling that are handled internally

Since the user might write invalid input, we split the functions and predicates into two catagories:
1. User functions and predicates: Where the program validates all inputs before running any operation and raises an error if the input is invalid
2. Internal functions an predicates: Where there's no error handling due to the fact that it is impossible to have invalid input other than if the user tried to use them directly.

#### User Functions and Predicates

User Functions and Predicats:
- Predicates:
    The following predicates return a boolean value indicating if the expression is a specifc operation expression or not
    - ``(variable? expr)``  : returns boolean value indicating if the expression is a variable or not
    - ``(sum? expr)``       : for sum expressions (supports polyvariadic expressions)
    - ``(product? expr)``   : for product expressions (supports polyvariadic expressions)
    - ``(sub? expr)``       : for subtraction expressions
    - ``(div? expr)``       : for division expressions 
    - ``(exp? expr)``       : for power expressions
    - ``(sin? expr)``       : for sin expressions
    - ``(cos? expr)``       : for cos expressions
    - ``(tan? expr)``       : for tan expressions
    - ``(log? expr)``       : for log expressions
- Functions:
    - ``(summand-1 expr)``             : returns the first addend of the sum expression (supports polyvariadic expressions)
    - ``(summand-2 expr)``             : returns the second addend of the sum expression (supports polyvariadic expressions)
    - ``(multiplier-1 expr)``          : returns the first factor of the product expression (supports polyvariadic expressions)
    - ``(multiplier-2 expr)``          : returns the second factor of the product expression (supports polyvariadic expressions)
    - ``(derivative expr respect)``    : returns the symbolic derivative of a given expression in respect to a given variable (note: the derivate won't be in the simplest form)
    - ``(simplify expr)``              : returns the simplified version of a given expression (note: only simplifies trivial operations like:
                                     [operations for constants, removal of 0 terms...etc], and does not simplify the expression down to polynomial of its variables)
    - ``(to-infix expr)``              : returns the infix form of an expression
    - ``(variables-of expr)``          : returns a list of the variables present in an expression
    - ``(gradient expr respects)``     : returns a gradient of a multivariable expression given explicity the list of variables

Internal Functions and Predicates: (shouldn't be used by the user directly due to the lack of error-handling in them)
- Predicates:
    - ``(is-char? str)            ``                              ; checks if string starts with an alphabetic letter
    - ``(operation? operator expr)``                              ; given an operator and an expression, checks if the expression uses that operator
    - ``(atomic? expr)            ``                              ; checks if an expression is not a lits
    - ``(valid-expr? expr)        ``                              ; checks if an expression is syntactically valid 
    - ``(atomic-number? expr)     ``                              ; checks if the expression is atomic and is a number
- Functions: 
    - ``(format-error expected-expr expected-form actual-expr)``  ; formats an error message given some variable message parts
    - ``(expression-error expr operator)                      ``  ; calls format-error with the variable message parts based on the operation
    - ``(derivative-of-atomic var respect)                    ``  ; derives a variable or a number
    - ``(derivative-sin expr respect)                         ``  ; derives a sin expression
    - ``(derivative-cos expr respect)                         ``  ; derives a cos expression
    - ``(derivative-tan expr respect)                         ``  ; derives a tan expression
    - ``(simplify-sum expr)                                   ``  ; simplifies a sum expression
    - ``(simplify-product expr)                               ``  ; simplifies a product expression
    - ``(simplify-sub expr)                                   ``  ; simplifies a subtraction expression
    - ``(simplify-div expr)                                   ``  ; simplifies a division expression
    - ``(simplify-exp expr)                                   ``  ; simplifies a power expression
    - ``(simplify-sin expr)                                   ``  ; simplifies a sin expression
    - ``(simplify-cos expr)                                   ``  ; simplifies a cos expression
    - ``(simplify-tan expr)                                   ``  ; simplifies a tan expression
    - ``(simplify-log expr)                                   ``  ; simplifies a log expression
    - ``(simplify-at-root expr)                               ``  ; checks the expression type and delegates the simplification to its designated function

### Examples

``(derivative expr respect)`` : derivative function allows you to derive an expression in respect to a variable


```
Input: 
(derivative '(* (+ x y) (+ x (+ x x))) 'x)
Output: 
'(+ (* (+ 1 0) (+ x (+ x x))) (* (+ 1 (+ 1 1)) (+ x y)))
```
<hr>

``(simplify expr)`` : simplify function allows you to simplify an expression

```
Input: 
(simplify '(+ (* (+ 1 0) (+ x (+ x x))) (* (+ x y) (+ 1 (+ 1 1)))))
Output: 
'(+ (+ x (+ x x)) (* (+ x y) 3))
```
<hr>

``(to-infix expr)`` : to-infix function allows you to get an expression in infix form

```
Input: 
(to-infix '(+ (+ x (+ x x)) (* (+ x y) 3)))
Output: 
'((x + (x + x)) + ((x + y) * 3))
```
<hr>

``(variables-of expr)`` : variables-of function retrieves a sorted list of the unique variables in a given expression
```
Input: 
(variables-of '(+ 1 x y (* x y z)))
Output: 
'(x y z)
```
<hr>

``(gradient expr respects)`` : gradient function allows you to calculate gradient of a multivariable expression (given explicitly the list of variables)

```
Input: 
(gradient '(+ 1 x y (* x y z)) '(x y z))
Output: 
'((+ (* y z) 1) (+ (* x z) 1) (* x y))
```

<p align="right">(<a href="#top">back to top</a>)</p>
<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- CONTACT -->
## Contact

Mosab Mohamed - [@IVIosab](https://t.me/IVIosab) - mosab.f.r@gmail.com

Project Link: [https://github.com/IVIosab/racket-symbolic-differentation](https://github.com/IVIosab/racket-symbolic-differentation)

<p align="right">(<a href="#top">back to top</a>)</p>
