Dealing with Data in R
========================================================

*Note: This material is a mash-up from Jenny Bryan's [course materials on r] (http://www.stat.ubc.ca/~jenny/STAT545A/quick-index.html) and Karthik Ram's [material from the Canberra Software Carpentry R Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra).  Anything good is there becuase of Jenny and Karthik.  Mistakes are all mine.*

# Index
 - [R Data Structures](#r-data-structures)
 - [File I/O](#file-input-and-ouput)
 - [Control Structures](#control-structures)

# R Data Structures
* To make the best of the R language, you'll need a strong understanding of the basic data types and data structures and how to operate on those.

* **Very Important** to understand because these are the things you will manipulate on a day-to-day basis in R. Most common source of frustration among beginners.

* Everything in `R` is an object. 

`R` has 5 basic atomic classes

* character
* numeric (real or decimal)
* integer
* logical
* complex


| Example | Type |
| ------- | ---- |
| "a", "swc" | character |
| 2, 15.5 | numeric | 
| 2 (usually add a `L` at end to denote integer) | integer |
| `TRUE`, `FALSE` | logical |
| 1+4i | complex | 



```r
typeof()  # what is it?
length()  # how long is it? What about two dimensional objects?
attributes()  # does it have any metadata?
```


R also has many data structures. These include

* vector
* list
* matrix
* data frame
* factors
* tables


### Vectors
A vector is the most common and basic data structure in `R` and is pretty much the workhorse of R. Vectors can be of two types:

* atomic vectors
* lists

**Atomic Vectors**
A vector can be a vector of characters, logical, integers or numeric.

Create an empty vector with `vector()`


```r
x <- vector()
x
```

```
## logical(0)
```

```r
# with a pre-defined length
x <- vector(length = 10)
x
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

```r
# with a length and type
vector("character", length = 10)
```

```
##  [1] "" "" "" "" "" "" "" "" "" ""
```

```r
vector("numeric", length = 10)
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0
```

```r
vector("integer", length = 10)
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0
```

```r
vector("logical", length = 10)
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

The general pattern is `vector(class of object, length)`.  You can also create vectors by concactenating them using the `c()` function.

Various examples:


```r
x <- c(1, 2, 3)
```

x is a numeric vector. These are the most common kind. They are numeric objects and are treated as double precision real numbers. To explicitly create integers, add a `L` at the end.


```r
x1 <- c(1L, 2L, 3L)
```


You can also have logical vectors. 


```r
y <- c(TRUE, TRUE, FALSE, FALSE)
```


Finally you can have character vectors:


```r
z <- c("Alec", "Dan", "Rob", "Karthik")
```


**Examine your vector**  


```r
typeof(z)
```

```
## [1] "character"
```

```r
length(z)
```

```
## [1] 4
```

```r
class(z)
```

```
## [1] "character"
```

```r
str(z)
```

```
##  chr [1:4] "Alec" "Dan" "Rob" "Karthik"
```


Question: Do you see a property that's common to all these vectors above?

**Add elements**


```r
z <- c(z, "Annette")
z
```

```
## [1] "Alec"    "Dan"     "Rob"     "Karthik" "Annette"
```


More examples of vectors


```r
x <- c(0.5, 0.7)
x <- c(TRUE, FALSE)
x <- c(T, F)
x <- c("a", "b", "c", "d", "e")
x <- 9:100
x <- c(i + 0, 2 + 4)
```

```
## Error: object 'i' not found
```


You can also create vectors as sequence of numbers


```r
series <- 1:10
seq(10)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
seq(1, 10, by = 0.1)
```

```
##  [1]  1.0  1.1  1.2  1.3  1.4  1.5  1.6  1.7  1.8  1.9  2.0  2.1  2.2  2.3
## [15]  2.4  2.5  2.6  2.7  2.8  2.9  3.0  3.1  3.2  3.3  3.4  3.5  3.6  3.7
## [29]  3.8  3.9  4.0  4.1  4.2  4.3  4.4  4.5  4.6  4.7  4.8  4.9  5.0  5.1
## [43]  5.2  5.3  5.4  5.5  5.6  5.7  5.8  5.9  6.0  6.1  6.2  6.3  6.4  6.5
## [57]  6.6  6.7  6.8  6.9  7.0  7.1  7.2  7.3  7.4  7.5  7.6  7.7  7.8  7.9
## [71]  8.0  8.1  8.2  8.3  8.4  8.5  8.6  8.7  8.8  8.9  9.0  9.1  9.2  9.3
## [85]  9.4  9.5  9.6  9.7  9.8  9.9 10.0
```


**Other objects**

`Inf` is infinity. You can have positive or negative infinity.


```r
1/0
```

```
## [1] Inf
```

```r
# [1] Inf
1/Inf
```

```
## [1] 0
```

```r
# [1] 0
```



`NaN` means Not a number. it's an undefined value.


```r
0/0
```

```
## [1] NaN
```

```r
NaN.
```

```
## Error: object 'NaN.' not found
```


Each object has an attribute. Attribues can be part of an object of R. These include 

* names
* dimnames
* length
* class
* attributes (contain metadata)

For a vector, `length(vector_name)` is just the total number of elements.


**What happens when you mix types?**

R will create a resulting vector that is the least common denominator. The coercion will move towards the one that's easiest to coerce to.

**Guess what the following do without running them first**


```r
xx <- c(1.7, "a")
xx <- c(TRUE, 2)
xx <- c("a", TRUE)
```


This is called implicit coercion.  You can also coerce vectors explicitly using the `as.<class_name>`. Example


```r
as.numeric()
as.character()
```



When you coerce an existing numeric vector with `as.numeric()`, it does nothing.


```r
x <- 0:6
as.numeric(x)
```

```
## [1] 0 1 2 3 4 5 6
```

```r
as.logical(x)
```

```
## [1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
```

```r
as.character(x)
```

```
## [1] "0" "1" "2" "3" "4" "5" "6"
```

```r
as.complex(x)
```

```
## [1] 0+0i 1+0i 2+0i 3+0i 4+0i 5+0i 6+0i
```


Sometimes coercions, especially nonsensical ones wont work.


```r
x <- c("a", "b", "c")
as.numeric(x)
```

```
## Warning: NAs introduced by coercion
```

```
## [1] NA NA NA
```

```r
as.logical(x)
```

```
## [1] NA NA NA
```

```r
# both don't work
```


**Sometimes there is implicit conversion**

```r
1 < "2"
```

```
## [1] TRUE
```

```r
"1" > 2
```

```
## [1] FALSE
```


## Matrix

Matrices are a special vector in R. They are not a separate class of object but simply a vector but now with dimensions added on to it. Matrices have rows and columns. 


```r
m <- matrix(nrow = 2, ncol = 2)
m
```

```
##      [,1] [,2]
## [1,]   NA   NA
## [2,]   NA   NA
```

```r
dim(m)
```

```
## [1] 2 2
```

```r
# same as
attributes(m)
```

```
## $dim
## [1] 2 2
```


Matrices are constructed columnwise. 


```r
m <- matrix(1:6, nrow = 2, ncol = 3)
```


Other ways to construct a matrix


```r
m <- 1:10
dim(m) <- c(2, 5)
```


This takes a vector and transform into a matrix with 2 rows and 5 columns.


Another way is to bind columns or rows using `cbind()` and `rbind()`.


```r
x <- 1:3
y <- 10:12
cbind(x, y)
```

```
##      x  y
## [1,] 1 10
## [2,] 2 11
## [3,] 3 12
```

```r
# or
rbind(x, y)
```

```
##   [,1] [,2] [,3]
## x    1    2    3
## y   10   11   12
```


---

## List

In R lists act as containers. Unlike atomic vectors, its contents are not restricted to a single mode and can encompass any data type. Lists are sometimes called recursive vectors, because a list can contain other lists. This makes them fundamentally different from atomic vectors. 

List is a special vector. Each element can be a different class.



Create lists using `list` or coerce other objects using `as.list()`



```r
x <- list(1, "a", TRUE, 1 + 4)
```



```r
x <- 1:10
x <- as.list(x)
length(x)
```

```
## [1] 10
```


What is the class of `x[1]`?  
how about `x[[1]]`?


```r
xlist <- list(a = "Karthik Ram", b = 1:10, data = head(iris))
```


what is the length of this object?
what about its structure?

List can contain as many lists nested inside.


```r
temp <- list(list(list(list())))
temp
```

```
## [[1]]
## [[1]][[1]]
## [[1]][[1]][[1]]
## list()
```

```r
is.recursive(temp)
```

```
## [1] TRUE
```


Lists are extremely useful inside functions. You can "staple" together lots of different kinds of results into a single object that a function can return.

It doesn't print out like a vector. Prints a new line for each element.

Elements are indexed by double brackets. Single brackets will still return a(nother) list.


---

## Factors

Factors are special vectors that represent categorical data. Factors can be ordered or unordered and are important when for modelling functions such as `lm()` and `glm()` and also in plot methods.

Factors can only contain pre-defined values.

Factors are pretty much integers that have labels on them.  While factors look (and often behave) like character vectors, they are actually integers under the hood, and you need to be careful when treating them like strings. Some string methods will coerce factors to strings, while others will throw an error.

Sometimes factors can be left unordered. Example: male, female

Other times you might want factors to be ordered (or ranked). Example: low, medium, high. 


Underlying it's represented by numbers 1,2,3.


They are better than using simple integer labels because factors are what are called self describing. male and female is more descriptive than 1s and 2s. Helpful when there is no additional metadata.

Which is male? 1 or 2? You wouldn't be able to tell with just integer data. Factors have this information built in.

Factors can be created with `factor()`. Input is a character vector.


```r
x <- factor(c("yes", "no", "no", "yes", "yes"))
x
```

```
## [1] yes no  no  yes yes
## Levels: no yes
```


`table(x)` will return a frequency table.

`unclass(x)` strips out the class information.

In modeling functions, importnat to know whta baseline levels is.
This is the first factor but by default the ordering is determined by alphabetical order of words entered. You can change this by speciying the levels.


```r
x <- factor(c("yes", "no", "yes"), levels = c("yes", "no"))
```


## Data frame

A data frame is a very important data type in R. It's pretty much the de facto data structure for most tabular data and what we use for statistics.

data frames can have additional attributes such as `rownames()`. This can be useful for annotating data, like subject_id or sample_id. But most of the time they are not used.

e.g. `rownames()`
useful for annotating data. subject names.
other times they are not useful.

* Data frames Usually created by read.csv and read.table.

* Can convert to `matrix` with `data.matrix()`

* Coercion will force and not always what you expect.

* Can also create with `data.frame()` function.


With and data frame, you can do `nrow(df)` and `ncol(df)`
rownames are usually 1..n.

**Combining data frames**


```r
df <- data.frame(id = letters[1:10], x = 1:10, y = rnorm(10))
df
```

```
##    id  x        y
## 1   a  1  0.98345
## 2   b  2  0.10342
## 3   c  3  1.15444
## 4   d  4  1.81763
## 5   e  5  0.07442
## 6   f  6 -1.15988
## 7   g  7 -0.58382
## 8   h  8  0.16550
## 9   i  9 -1.18302
## 10  j 10 -0.30984
```


`cbind(df, data.frame(z = 4))`

When you combine column wise, only row numbers need to match. If you are adding a vector, it will get repeated.

**Useful functions**  
`head()` - see first 5 rows
`tail()` - see last 5 rows
`dim()` - see dimensions
`nrow()` - number of rows
`ncol()` - number of columns
`str()` - structure of each column
`names()` - will list column names for a data.frame (or any object really).

A data frame is a special type of list where every element of a list has same length.

See that it is actually a special list:

    > is.list(iris)
    [1] TRUE
    > class(iris)
    [1] "data.frame"
     > 
--

**Naming objects**  

Other R objects can also have names not just true for data.frames. Adding names is helpful since it's useful for readable code and self describing objects.


```r
x <- 1:3
names(x) <- c("karthik", "ram", "rocks")
x
```

```
## karthik     ram   rocks 
##       1       2       3
```


Lists can also have names.


```r
x <- as.list(1:10)
names(x) <- letters[seq(x)]
x
```

```
## $a
## [1] 1
## 
## $b
## [1] 2
## 
## $c
## [1] 3
## 
## $d
## [1] 4
## 
## $e
## [1] 5
## 
## $f
## [1] 6
## 
## $g
## [1] 7
## 
## $h
## [1] 8
## 
## $i
## [1] 9
## 
## $j
## [1] 10
```


Finally matrices can have names and these are called `dimnames`


```r
m <- matrix(1:4, nrow = 2)
dimnames(m) <- list(c("a", "b"), c("c", "d"))
# first element = rownames second element = colnames
```

---


## Missing values

dennoted by `NA` and/or `NaN` for undefined mathematical operations.


```r
is.na()
is.nan()
```


check for both.

NA values have a class. So you can have both an integer NA and a missing character NA.

Nan is also NA. But not the other way around.


```r
x <- c(1, 2, NA, 4, 5)
is.na(x)  #returns logical. shows third
```

```
## [1] FALSE FALSE  TRUE FALSE FALSE
```

```r
is.nan(x)  #none are NaN.
```

```
## [1] FALSE FALSE FALSE FALSE FALSE
```



```r
x <- c(1, 2, NA, NaN, 4, 5)
is.na(x)  #shows 2 TRUE.
```

```
## [1] FALSE FALSE  TRUE  TRUE FALSE FALSE
```

```r
is.nan(x)  #shows 1 TRUE
```

```
## [1] FALSE FALSE FALSE  TRUE FALSE FALSE
```


---

# Refresher on data types

![](https://raw.github.com/swcarpentry/2013-10-09-canberra/master/01-R-basics/data-types.png)


# File I/O


# Control Stuctures



