Functions, Control Structures and Documentation
========================================================

*Note: This lesson is mostly from Karthik Ram's [material from the Canberra Software Carpentry R Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra).  Anything good is there becuase of Karthik.  Mistakes are all mine.*

# Lesson Index:
- [Functions](#functions)
- [Control Structures](#control-strucutres)
- [Documentation](#documentation)
- [Exercise](#function-exercise)

# Functions 

Functions represent one of the most powerful features of the R language. By creating functions, you can go past just using what others have written and create your own.

After a while operations at the interactive prompt are not enough. You have special operations that need to be grouped together. That's when you'll need to write functions.

* Basic of how to write functions
* Lexical scoping and scoping rules
* Example

Functions are created using the `function()` directive.
They are stored as any other R object.  They are of class "function"

**Basic syntax**

```
func <- function(<take some input>)  {
    # run a whole bunch of your code
}
```

Functions are first class objects. Treat them the same way as any other objects. This means you can pass them to other functions (as you would do with say vectors). Very useful feature for statistical analyses. We can also nest functions inside each other.

Return value is the very last R expression to be evaluated. No special expression for returning although you can use return()

Functions have named arguments and these can have default values. Arguments used inside a function definition are called formal arguments.

use `formals()` to see what they are.

Not every function makes use of all formal arguments.

Function arguments can be missing or might just use default values. Formal arguments are included in the function definition.

If a function has 10 arguments, you don't have to specify all of them. 

R function arguments can be matched positionally or by name.


```r
args(sd)
```

```
## function (x, na.rm = FALSE) 
## NULL
```

Takes x, a vector of data. 
Default is `na.rm` is false.
So you don't have to do anything.


```r
dat <- rnorm(1000)
sd(dat)  # Matched positionally
```

```
## [1] 0.9818
```

```r
sd(x = dat)  # matched by name
```

```
## [1] 0.9818
```

```r
sd(x = dat, na.rm = FALSE)
```

```
## [1] 0.9818
```

```r
# When naming you can switch them but not recommended
sd(na.rm = FALSE, dat)
```

```
## [1] 0.9818
```


Even though it's allowed, don't switch positions on names.

See 


```r
args(lm)
```

```
## function (formula, data, subset, weights, na.action, method = "qr", 
##     model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, 
##     contrasts = NULL, offset, ...) 
## NULL
```


It's good to name arguments when you have a long list to work through. Arguments can also be partially matched.


```r
add <- function(x = 1, y = 2) {
    x + y
}
```


R does what's known as lazy evaluation. Arguments to functions are lazily evaluated. Common model in many languages. Only evaluated as they are needed.


```r
add <- function(a, b) {
    a^2
}
```


The function call never uses b. So calling `f(10)` will not produce an error because `a` got positionally matched.

Another example of lazy evaluation



```r
add <- function(a, b) {
    print(a^2)
    print(b^2)
}
```


`add(10)` will work until it reaches a point where the function will break.

**The three dot operator.**

`...` are used when extending another function and you don't want to copy the entire list of arguments from the original function.

Also useful when the number of arguments cannot be known in advance.

e.g. `paste()`

takes variable number of arguments. Function does not know how many things it's going to paste together.


## Writing functions in R

If you have to repeat the same few lines of code more than once, then you really need to write a function. Functions are a fundamental building block of R. You use them all the time in R and it's not that much harder to string functions together (or write entirely new ones from scratch) to do more.

* R functions are objects just like anything else. 
* By default, R function arguments are lazy - they're only evaluated if they're actually used:
* Every call on a R object is almost always a function call.

### Basic components of a function

* The `body()`, the code inside the function.
* The `formals()`, the "formal" argument list, which controls how you can call the function.
* The `environment()`` which determines how variables referred to inside the function are found.
* `args()` to list arguments.


```r
f <- function(x) x
f
```

```
## function(x) x
```

```r

formals(f)
```

```
## $x
```

```r

environment(f)
```

```
## <environment: R_GlobalEnv>
```


**Question: How do we delete this function from our environment?**

## More on environments
Variables defined inside functions exist in a different environment than the global environment. However, if a function is not defined inside one, it will look one level above.

For example.

```
x <- 2
g <- function() { 
  y <- 1
  c(x, y)
}  
g()
rm(x, g)
```

Same rule applies for nested functions.

A first useful function.


```r
first <- function(x, y) {
    z <- x + y
    return(z)
}
```



```r
add <- function(a, b) {
    return(a + b)
}

add(c(1, 2, 3, 4), 1)
```

```
## [1] 2 3 4 5
```


## What does this function return?


```r
x <- 5
f <- function() {
    y <- 10
    c(x = x, y = y)
}
f()
```

```
##  x  y 
##  5 10
```


## What does this function return?


```r
x <- 5
g <- function() {
    x <- 20
    y <- 10
    c(x = x, y = y)
}
g()
```

```
##  x  y 
## 20 10
```


## What does this function return??


```r
x <- 5
h <- function() {
    y <- 10
    i <- function() {
        z <- 20
        c(x = x, y = y, z = z)
    }
    i()
}
h()
```

```
##  x  y  z 
##  5 10 20
```



## Functions with pre defined values


```r
temp <- function(a = 1, b = 2) {
    return(a + b)
}
```


## Functions usually return the last value it computed

```
f <- function(x) {
  if (x < 10) {
    0
  } else {
    10
  }
}
f(5)
f(15)
```

# Control Structures

# Documentation

# Function Exercise
