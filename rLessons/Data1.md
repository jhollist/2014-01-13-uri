Dealing with Data in R
======================

*Note: This material is a mash-up from Jenny Bryan's [course materials
on r] (http://www.stat.ubc.ca/\~jenny/STAT545A/quick-index.html) and
Karthik Ram's [material from the Canberra Software Carpentry R
Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra). Anything
good is there becuase of Jenny and Karthik. Mistakes are all mine.*

Index
-----

-   [R Data Structures](#r-data-structures)
-   [File I/O](#file-io)
-   [Data Subsetting and
    Manipulation](#data-subsetting-and-manipulation)

R Data Structures
-----------------

-   To make the best of the R language, you'll need a strong
    understanding of the basic data types and data structures and how to
    operate on those.

-   **Very Important** to understand because these are the things you
    will manipulate on a day-to-day basis in R. Most common source of
    frustration among beginners.

-   Everything in `R` is an object.

`R` has 5 basic atomic classes

-   character
-   numeric (real or decimal)
-   integer
-   logical
-   complex

  Example                                          Type
  ------------------------------------------------ -----------
  "a", "swc"                                       character
  2, 15.5                                          numeric
  2 (usually add a `L` at end to denote integer)   integer
  `TRUE`, `FALSE`                                  logical
  1+4i                                             complex

~~~~ {.r}
typeof()  # what is it?
length()  # how long is it? What about two dimensional objects?
attributes()  # does it have any metadata?
~~~~

R also has many data structures. These include

-   vector
-   list
-   matrix
-   data frame
-   factors
-   tables

### Vectors

A vector is the most common and basic data structure in `R` and is
pretty much the workhorse of R. Vectors can be of two types:

-   atomic vectors
-   lists

**Atomic Vectors** A vector can be a vector of characters, logical,
integers or numeric.

Create an empty vector with `vector()`

~~~~ {.r}
x <- vector()
x
# with a pre-defined length
x <- vector(length = 10)
x
# with a length and type
vector("character", length = 10)
vector("numeric", length = 10)
vector("integer", length = 10)
vector("logical", length = 10)
~~~~

The general pattern is `vector(class of object, length)`. You can also
create vectors by concactenating them using the `c()` function.

Various examples:

~~~~ {.r}
x <- c(1, 2, 3)
~~~~

x is a numeric vector. These are the most common kind. They are numeric
objects and are treated as double precision real numbers. To explicitly
create integers, add a `L` at the end.

~~~~ {.r}
x1 <- c(1L, 2L, 3L)
~~~~

You can also have logical vectors.

~~~~ {.r}
y <- c(TRUE, TRUE, FALSE, FALSE)
~~~~

Finally you can have character vectors:

~~~~ {.r}
z <- c("Alec", "Dan", "Rob", "Karthik")
~~~~

**Examine your vector**

~~~~ {.r}
typeof(z)
length(z)
class(z)
str(z)
~~~~

Question: Do you see a property that's common to all these vectors
above?

**Add elements**

~~~~ {.r}
z <- c(z, "Annette")
z
~~~~

More examples of vectors

~~~~ {.r}
x <- c(0.5, 0.7)
x <- c(TRUE, FALSE)
x <- c(T, F)
x <- c("a", "b", "c", "d", "e")
x <- 9:100
x <- c(i + 0, 2 + 4)
~~~~

You can also create vectors as sequence of numbers

~~~~ {.r}
series <- 1:10
seq(10)
seq(1, 10, by = 0.1)
~~~~

**Other objects**

`Inf` is infinity. You can have positive or negative infinity.

~~~~ {.r}
1/0
# [1] Inf
1/Inf
# [1] 0
~~~~

`NaN` means Not a number. it's an undefined value.

~~~~ {.r}
0/0
NaN.
~~~~

Each object has an attribute. Attribues can be part of an object of R.
These include

-   names
-   dimnames
-   length
-   class
-   attributes (contain metadata)

For a vector, `length(vector_name)` is just the total number of
elements.

**What happens when you mix types?**

R will create a resulting vector that is the least common denominator.
The coercion will move towards the one that's easiest to coerce to.

**Guess what the following do without running them first**

~~~~ {.r}
xx <- c(1.7, "a")
xx <- c(TRUE, 2)
xx <- c("a", TRUE)
~~~~

This is called implicit coercion. You can also coerce vectors explicitly
using the `as.<class_name>`. Example

~~~~ {.r}
as.numeric()
as.character()
~~~~

When you coerce an existing numeric vector with `as.numeric()`, it does
nothing.

~~~~ {.r}
x <- 0:6
as.numeric(x)
as.logical(x)
as.character(x)
as.complex(x)
~~~~

Sometimes coercions, especially nonsensical ones wont work.

~~~~ {.r}
x <- c("a", "b", "c")
as.numeric(x)
as.logical(x)
# both don't work
~~~~

**Sometimes there is implicit conversion**

~~~~ {.r}
1 < "2"
"1" > 2
~~~~

### Matrix

Matrices are a special vector in R. They are not a separate class of
object but simply a vector but now with dimensions added on to it.
Matrices have rows and columns.

~~~~ {.r}
m <- matrix(nrow = 2, ncol = 2)
m
dim(m)
# same as
attributes(m)
~~~~

Matrices are constructed columnwise.

~~~~ {.r}
m <- matrix(1:6, nrow = 2, ncol = 3)
~~~~

Other ways to construct a matrix

~~~~ {.r}
m <- 1:10
dim(m) <- c(2, 5)
~~~~

This takes a vector and transform into a matrix with 2 rows and 5
columns.

Another way is to bind columns or rows using `cbind()` and `rbind()`.

~~~~ {.r}
x <- 1:3
y <- 10:12
cbind(x, y)
# or
rbind(x, y)
~~~~

### Factors

Factors are special vectors that represent categorical data. Factors can
be ordered or unordered and are important when for modelling functions
such as `lm()` and `glm()` and also in plot methods.

Factors can only contain pre-defined values.

Factors are pretty much integers that have labels on them. While factors
look (and often behave) like character vectors, they are actually
integers under the hood, and you need to be careful when treating them
like strings. Some string methods will coerce factors to strings, while
others will throw an error.

Sometimes factors can be left unordered. Example: male, female

Other times you might want factors to be ordered (or ranked). Example:
low, medium, high.

Underlying it's represented by numbers 1,2,3.

They are better than using simple integer labels because factors are
what are called self describing. male and female is more descriptive
than 1s and 2s. Helpful when there is no additional metadata.

Which is male? 1 or 2? You wouldn't be able to tell with just integer
data. Factors have this information built in.

Factors can be created with `factor()`. Input is a character vector.

~~~~ {.r}
x <- factor(c("yes", "no", "no", "yes", "yes"))
x
~~~~

`table(x)` will return a frequency table.

`unclass(x)` strips out the class information.

In modeling functions, importnat to know whta baseline levels is. This
is the first factor but by default the ordering is determined by
alphabetical order of words entered. You can change this by speciying
the levels.

~~~~ {.r}
x <- factor(c("yes", "no", "yes"), levels = c("yes", "no"))
~~~~

### Data frame

A data frame is a very important data type in R. It's pretty much the de
facto data structure for most tabular data and what we use for
statistics.

data frames can have additional attributes such as `rownames()`. This
can be useful for annotating data, like subject\_id or sample\_id. But
most of the time they are not used.

e.g. `rownames()` useful for annotating data. subject names. other times
they are not useful.

-   Data frames Usually created by read.csv and read.table.

-   Can convert to `matrix` with `data.matrix()`

-   Coercion will force and not always what you expect.

-   Can also create with `data.frame()` function.

With and data frame, you can do `nrow(df)` and `ncol(df)` rownames are
usually 1..n.

**Combining data frames**

~~~~ {.r}
df <- data.frame(id = letters[1:10], x = 1:10, y = rnorm(10))
df
~~~~

`cbind(df, data.frame(z = 4))`

When you combine column wise, only row numbers need to match. If you are
adding a vector, it will get repeated.

**Useful functions**\
`head()` - see first 5 rows `tail()` - see last 5 rows `dim()` - see
dimensions `nrow()` - number of rows `ncol()` - number of columns
`str()` - structure of each column `names()` - will list column names
for a data.frame (or any object really). `summary()` - summarizes each
column in a data frame

A data frame is a special type of list where every element of a list has
same length.

See that it is actually a special list:

    > is.list(iris)
    [1] TRUE
    > class(iris)
    [1] "data.frame"
     > 

--

**Naming objects**

Other R objects can also have names not just true for data.frames.
Adding names is helpful since it's useful for readable code and self
describing objects.

~~~~ {.r}
x <- 1:3
names(x) <- c("karthik", "ram", "rocks")
x
~~~~

Lists can also have names.

~~~~ {.r}
x <- as.list(1:10)
names(x) <- letters[seq(x)]
x
~~~~

Finally matrices can have names and these are called `dimnames`

~~~~ {.r}
m <- matrix(1:4, nrow = 2)
dimnames(m) <- list(c("a", "b"), c("c", "d"))
# first element = rownames second element = colnames
~~~~

### Refresher on data types

![](https://raw.github.com/swcarpentry/2013-10-09-canberra/master/01-R-basics/data-types.png)

File I/O
--------

### Input output operations

### Inputting data

~~~~ {.r}
x <- scan("data_file.txt")  #Example Data created by Jeff Hollister
# add a separator
x <- scan("messy_data.txt", what = " ", sep = "\n")  #Example Data created by Karthik Ram
# or read data from the console
x <- scan()
# keep entering values and hit an empty return key to end Note the class of
# the data
class(x)
~~~~

Reading single lines (e.g. user input)

~~~~ {.r}
variable <- readline()
class(variable)
# or provide more information
variable <- readline("Enter number of simulations: ")
~~~~

### Reading files

Most plain text files can be read with `read.table` or variants thereof
(such as `read.csv`).

~~~~ {.r}
df <- read.table("data.dat", header = TRUE)  #Example Data created by Jeff Hollister
# Note class of df
class(df)
~~~~

or using `readLines`

~~~~ {.r}
dt <- readLines("messy_data.txt")
~~~~

### Files from the web

    url <- "https://raw.github.com/karthikram/ggplot-lecture/master/climate.csv"
    my_data <- read.csv(url, header = TRUE)

### Local file operations

One can list files from any local source as well. This is review from
the first R lesson, but handy to repeat here.

~~~~ {.r}
list.files()
file.info()
dir()
file.exists()
getwd()
setwd()
~~~~

### Subsetting lists

Subsetting a list works in exactly the same way as subsetting an atomic
vector. Subsetting a list with [ will always return a list:
``` [[`` and ```\$\`, as described below, let you pull out the
components of the list.

~~~~ {.r}
x <- as.list(1:10)

x[1:5]

# What class is this object?
~~~~

To extract individual elements inside a list, use the `[[` operator

~~~~ {.r}
# to get element 5

x[[5]]

class(x[[5]])

another_variable <- x[[5]]
~~~~

Or using their names

~~~~ {.r}
names(x) <- letters[1:5]

x$a
x[["a"]]
~~~~

### Subsetting matrices

~~~~ {.r}
a <- matrix(1:9, nrow = 3)
colnames(a) <- c("A", "B", "C")
a[1:2, ]
a[c(T, F, T), c("B", "A")]
a[0, -2]
~~~~

### Subsetting data frames

~~~~ {.r}
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df[df$x == 2, ]
df[c(1, 3), ]

# There are two ways to select columns from a data frame Like a list:
df[c("x", "z")]

# Like a matrix
df[, c("x", "z")]


# There's an important difference if you select a simple column: matrix
# subsetting simplifies by default, list subsetting does not.
df["x"]

df[, "x"]
~~~~

### Combining data frames

We have already seen some ways to combine vectors together and create
`data.frames` using `rbind` to add rows and `cbind` to combine columns.
You can accomplish in additional ways as well

~~~~ {.r}
# Review of creating data frame with rbind() and cbind()
x <- c(1, 2, 3, 4, 5)
y <- c(5, 6, 7, 8, 9)
df <- rbind(x, y)
# coerce to data.frame. rbind creates a matrix with row.names
as.data.frame(df)
df <- cbind(x, y)
# coerce to data.frame. rbind creates a matrix with row.names
as.data.frame(df)
# you can also use data.frame(), similar results to cbind
df <- data.frame(x, y)
df
~~~~

These methods work well if your vectors are of the same length, but that
is not often the case. A very common situation is two data frames that
share an ID value and you need to merge these together. In other words,
a join. That cna be accomplished with `merge`.

~~~~ {.r}
# create test data.frame 1
df1 <- data.frame(id = c(1:10), data1 = seq(1, 20, 2), data2 = jitter(seq(1, 
    20, 2), 10))

# create test data.frame 2
df2 <- data.frame(id = c(1, 1, 2, 2, 3, 4, 7, 8, 8, 9, 10, 11, 12, 12, 4), data1 = seq(1, 
    30, 2), data2 = jitter(seq(1, 30, 2), 10))

# now we can merge
dfm <- merge(df1, df2, by = "id")
dfm
dim(dfm)

# often we want to make sure all of one df is preserved
dfm <- merge(df1, df2, by = "id", all.x = TRUE)
dfm
dim(dfm)
~~~~

Apply Functions
---------------

One of the greatest joys of vectorized operations is being able to use
the entire family of `apply` functions that are available in base `R`.

These include:

~~~~ {.r}
apply
by
lapply
tapply
sapply
~~~~

### apply

~~~~ {.r}
m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
# 1 is the row index 2 is the column index
apply(m, 1, sum)
apply(m, 2, sum)
apply(m, 1, mean)
apply(m, 2, mean)
~~~~

### by

~~~~ {.r}
head(iris)
by(iris[, 1:2], iris[, "Species"], summary)
by(iris[, 1:2], iris[, "Species"], sum)
~~~~

### tapply

Two examples

~~~~ {.r}
df <- data.frame(names = sample(c("A", "B", "C"), 10, rep = T), length = rnorm(10))
tapply(df$length, df$names, mean)

# Now with a more familiar dataset
tapply(iris$Petal.Length, iris$Species, mean)
~~~~

### lapply (and llply)

What it does: Returns a list of same length as the input. Each element
of the output is a result of applying a function to the corresponding
element.

~~~~ {.r}
my_list <- list(a = 1:10, b = 2:20)
lapply(my_list, mean)
~~~~

### sapply

sapply is a more user friendly version of `lapply` and will return a
list of matrix where appropriate.

Let's work with the same list we just created.

~~~~ {.r}
my_list
x <- sapply(my_list, mean)
class(x)
~~~~

### replicate

An extremely useful function to generate datasets for simulation
purposes.

~~~~ {.r}
replicate(10, rnorm(10))
replicate(10, rnorm(10), simplify = TRUE)
# The final arguments turns the result into a vector or matrix if possible.
~~~~

### mapply

Its more or less a multivariate version of `sapply`. It applies a
function to all corresponding elements of each argument.

example:

~~~~ {.r}
list_1 <- list(a = c(1:10), b = c(11:20))
list_2 <- list(c = c(21:30), d = c(31:40))
mapply(sum, list_1$a, list_1$b, list_2$c, list_2$d)
~~~~

### plyr and reshape

Two packages that are very widely used for data manipulation are `plyr`
and `reshape` The argument for their use is that they simplify many of
the common data manipulation tasks, althought they do require a slightly
different way of thinking about manipulating data in R. As such, they
are a bit beyond the scope of what we are covering. But they are widely
used enough and powerful enough to bare mention.

For more on these, see Karthik Ram's Canberra Bootcamp Materials on
[`plyr` and
`reshape`](https://github.com/swcarpentry/2013-10-09-canberra/blob/master/03-data-manipulation/03-split-apply.md)

Data Manipulation Exercise
--------------------------

In our last exercise, we pulled in three datasets, created data frames
from them and answered some basic questions about those data frames. One
of the first things you will notice is that EPA is very fond of large,
verbose data structures. There is a lot more informaiton in these three
data frames than we really would like. The purpose of this exercise is
to pull out information we would like to keep, create a single data
frame for some future analysis, do some basic analysis, and write the
resulting data frame to disc. As before, all steps must be done in R and
save your commands in a commented script.

1.  Subset your station information data frame so that you only have the
    following columns: SITE\_ID, VISIT\_NO, SITE\_TYPE, STATE\_NAME
2.  Subset your water quality information so that you have: SITE\_ID,
    NTL, PTL, CHLA
3.  Susbset your landuse data frame so that you have: SITE\_ID,
    PCT\_DEVELOPED\_BUFR, PCT\_FOREST\_BUFR, PCT\_AGRIC\_BUFR, and
    PCT\_WETLAND\_BUFR.
4.  Subset your streamlined verison of station information (created in
    step 1) so that only the first visit of the probability lakes are
    returned. First visits are those records that have a VISIT\_NO of 1
    and probability lakes are SITE\_TYPE of PROB\_lake.
5.  Create a single data frame that combines the station information
    from step 4, the water quality info from step 2, and the landuse
    information from step 3.
6.  How many rows and columns in this new data frame? Is it what you
    expected?
7.  Calculate the mean, median, min, and max (1st and 3rd quartiles ok
    too) for each of the data columns. You can do this many ways, but
    you have the tools to do it in one, very clean line of code!
8.  Once you are certain your new data frame has all the proper rows and
    columns, write the data frame out to a .csv file. We will be using
    this file as input later, so keep track of where you put it.
