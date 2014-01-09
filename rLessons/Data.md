Dealing with Data in R
========================================================

*Note: This material is a mash-up from Jenny Bryan's [course materials on r] (http://www.stat.ubc.ca/~jenny/STAT545A/quick-index.html) and Karthik Ram's [material from the Canberra Software Carpentry R Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra).  Anything good is there becuase of Jenny and Karthik.  Mistakes are all mine.*

## Index
 - [R Data Structures](#r-data-structures)
 - [File I/O](#file-io)
 - [Data Subsetting and Manipulation](#data-subsetting-and-manipulation)

## R Data Structures
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


### Matrix

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

### List

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

### Factors

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


### Data frame

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
## 1   a  1  0.17806
## 2   b  2 -0.18527
## 3   c  3  0.17601
## 4   d  4  0.04711
## 5   e  5 -0.43921
## 6   f  6 -1.17539
## 7   g  7  0.16760
## 8   h  8 -1.48294
## 9   i  9  0.71051
## 10  j 10  1.05606
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
`summary()` - summarizes each column in a data frame

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


### Missing values

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

### Refresher on data types

![](https://raw.github.com/swcarpentry/2013-10-09-canberra/master/01-R-basics/data-types.png)


## File I/O

### Input output operations

### Inputting data


```r
x <- scan("data_file.txt")  #Example Data created by Jeff Hollister
# add a separator
x <- scan("messy_data.txt", what = " ", sep = "\n")  #Example Data created by Karthik Ram
# or read data from the console
x <- scan()
# keep entering values and hit an empty return key to end Note the class of
# the data
class(x)
```

```
## [1] "numeric"
```

Reading single lines (e.g. user input)


```r
variable <- readline()
```

```r
class(variable)
```

```
## [1] "character"
```

```r
# or provide more information
variable <- readline("Enter number of simulations: ")
```

```
## Enter number of simulations:
```



### Reading files  
Most plain text files can be read with `read.table` or variants thereof (such as `read.csv`).


```r
df <- read.table("data.dat", header = TRUE)  #Example Data created by Jeff Hollister
# Note class of df
class(df)
```

```
## [1] "data.frame"
```


or using `readLines`


```r
dt <- readLines("messy_data.txt")
```

```
## Warning: incomplete final line found on 'messy_data.txt'
```


### Files from the web

```
url <- "https://raw.github.com/karthikram/ggplot-lecture/master/climate.csv"
my_data <- read.csv(url, header = TRUE)
```

### Local file operations

One can list files from any local source as well.  This is review from the first R lesson, but handy to repeat here.


```r
list.files()
```

```
##  [1] "avgX.txt"       "data-types.png" "data.dat"       "Data.html"     
##  [5] "Data.md"        "Data.Rmd"       "data_file.txt"  "DataViz.html"  
##  [9] "DataViz.md"     "DataViz.Rmd"    "figure"         "Functions.html"
## [13] "Functions.md"   "Functions.Rmd"  "Intro.html"     "Intro.md"      
## [17] "Intro.Rmd"      "messy_data.txt" "myPlot.pdf"     "README.html"   
## [21] "README.md"      "README.Rmd"     "rLessons.Rproj"
```

```r
file.info()
```

```
## Error: invalid filename argument
```

```r
dir()
```

```
##  [1] "avgX.txt"       "data-types.png" "data.dat"       "Data.html"     
##  [5] "Data.md"        "Data.Rmd"       "data_file.txt"  "DataViz.html"  
##  [9] "DataViz.md"     "DataViz.Rmd"    "figure"         "Functions.html"
## [13] "Functions.md"   "Functions.Rmd"  "Intro.html"     "Intro.md"      
## [17] "Intro.Rmd"      "messy_data.txt" "myPlot.pdf"     "README.html"   
## [21] "README.md"      "README.Rmd"     "rLessons.Rproj"
```

```r
file.exists()
```

```
## Error: invalid 'file' argument
```

```r
getwd()
```

```
## [1] "D:/DATA/DataInformatics/SoftwareCarpentry/2014-01-13-uri/rLessons"
```

```r
setwd()
```

```
## Error: argument "dir" is missing, with no default
```


---

## Output

### Writing files

Saving files is easy in R. We have loaded the `iris` dataset into our memory, if not type `data(iris)` in the conosle. Can you save this back to a `csv` file to disk with the name `tgac_iris.csv`?

What commands did you use?


### Short term storage


```r
saveRDS(iris, file = "tgac_iris.rds")
iris_data <- readRDS("tgac_iris.rds")
```

This is great for short term storage. All factors and other modfications to the dataset will be preserved. However, only R can read these data back and not the best option if you want to keep the file stored in the easiest format.

### Long-term storage


```r
write.csv(iris, file = "tgac_iris.csv", row.names = FALSE)
```


### Easy to store compressed files to save space:


```r
write.csv(diamonds, file = bzfile("diamonds.csv.bz2"), row.names = FALSE)
```


### Reading is even easier:


```r
diamonds5 <- read.csv("diamonds.csv.bz2")
```


Files stored with `saveRDS()` are automatically compressed.

### Now a database example (teaser for tomorrow)

There are many options for using R to access data stored in a databse.  One simple and lightweight example is using `sqlite` and the R Package `RSQLite`.

This is a bit more involved, but wanted to show this simply as an example of what is possible.  Skys the limit on what type of database you can use.





```r
# If not already installed install.packages('RSQLite')
library(RSQLite)
```

```
## Loading required package: DBI
```

```r
con <- dbConnect(dbDriver("SQLite"), dbname = "diamonds.sqlite")
dbWriteTable(con, "tDiamonds", diamonds5)
```

```
## [1] TRUE
```

```r
list.files(pattern = "^dia")
```

```
## [1] "diamonds.csv.bz2" "diamonds.sqlite"
```

```r
# And to prove it did someting read the new table back in
sqliteDiamonds <- dbReadTable(con, "tDiamonds")
head(sqliteDiamonds)
```

```
##   carat       cut color clarity depth table price    x    y    z
## 1  0.23     Ideal     E     SI2  61.5    55   326 3.95 3.98 2.43
## 2  0.21   Premium     E     SI1  59.8    61   326 3.89 3.84 2.31
## 3  0.23      Good     E     VS1  56.9    65   327 4.05 4.07 2.31
## 4  0.29   Premium     I     VS2  62.4    58   334 4.20 4.23 2.63
## 5  0.31      Good     J     SI2  63.3    58   335 4.34 4.35 2.75
## 6  0.24 Very Good     J    VVS2  62.8    57   336 3.94 3.96 2.48
```

```r
dbDisconnect(con)
```

```
## [1] TRUE
```


### Data Structure and File I/O Exercise

For this exercise we will practice what we have learned so far.  You will download three files, create data.frames, and answer some questions about the data.  Only use R for these steps and save everything as a script with simple comments.

1. Download the [2007 National Lakes Assessment Station Information](http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_SampledLakeInformation_20091113.csv)
2. Download the [2007 National Lakes Assessment Water Quality Data](http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_WaterQuality_20091123.csv)
3. Download the [2007 National Lakes Assessment Buffer Landuse](http://water.epa.gov/type/lakes/assessmonitor/lakessurvey/upload/NLA2007_Buffer_Landuse_Metrics_20091022.csv)
5. Create a data frame for each file.
6. How many rows are in the water quality data? How many columns?
7. How many rows and columns are in the Landuse data?
8. What is the name of the first column in each dataframe
9. What class of data is this column?
10. What are the max, min, and mean values for `PCT_DEVELOPED_BUFR` in the buffer landuse data?
11. What are the max, min and mean values for `NTL` in the water quality data?


## Data Subsetting and Manipulation

### Subsetting data

R has many powerful subset operators and mastering them will allow you to easily perform complex operation on any kind of dataset. Allows you to manipulate data very succinctly.

As the last section for this topic we'll cover:

* The three subsetting operators,
* The six types of subsetting,
* Important difference in subsetting behaviour for different objects 
* Using subsetting in conjunction with assignment


### Subsetting atomic vectors


```r
x <- c(5.4, 6.2, 7.1, 4.8)
```


**We can subset this in 5 ways**

1.) Using positive integers


```r
x[1]
```

```
## [1] 5.4
```

```r
x[c(3, 1)]
```

```
## [1] 7.1 5.4
```

```r
# We can duplicate indices
x[c(1, 1)]
```

```
## [1] 5.4 5.4
```

```r
# Real numbers are silently truncated to integers
x[c(2.1, 2.9)]
```

```
## [1] 6.2 6.2
```


2.) Using negative integers


```r
# skip the first element
x[-1]
```

```
## [1] 6.2 7.1 4.8
```

```r
# skip the first and the fifth
x[-c(1, 5)]
```

```
## [1] 6.2 7.1 4.8
```


3.) Using logical operators


```r
x[c(TRUE, TRUE, FALSE, FALSE)]
```

```
## [1] 5.4 6.2
```

```r
# or based on a condition
x[x > 3]
```

```
## [1] 5.4 6.2 7.1 4.8
```

```r
x[which(x > 3)]
```

```
## [1] 5.4 6.2 7.1 4.8
```

```r
# Also see `which.max()` and `which.min()`
```


4.) with nothing


```r
x[]
```

```
## [1] 5.4 6.2 7.1 4.8
```

```r
# useful when dealing with 2 or higher dimensional objects
```


5.) with zero


```r
x[0]
```

```
## numeric(0)
```

```r
# helpful for generating test data or creating empty objects
```



6.) Referencing objects by their names


```r
y <- setNames(x, letters[1:4])

y[c("d", "c", "a")]
```

```
##   d   c   a 
## 4.8 7.1 5.4
```

```r

# We can also repeat indices
y[c("a", "a", "a")]
```

```
##   a   a   a 
## 5.4 5.4 5.4
```

```r

# Names are always matched exactly, not partially
z <- c(abc = 1, def = 2)
z[c("a", "d")]
```

```
## <NA> <NA> 
##   NA   NA
```


---

### Subsetting lists

Subsetting a list works in exactly the same way as subsetting an atomic vector. Subsetting a list with [ will always return a list: `[[`` and `$`, as described below, let you pull out the components of the list.


```r
x <- as.list(1:10)

x[1:5]
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 2
## 
## [[3]]
## [1] 3
## 
## [[4]]
## [1] 4
## 
## [[5]]
## [1] 5
```

```r

# What class is this object?
```


To extract individual elements inside a list, use the `[[` operator


```r
# to get element 5

x[[5]]
```

```
## [1] 5
```

```r

class(x[[5]])
```

```
## [1] "integer"
```

```r

another_variable <- x[[5]]
```


Or using their names


```r
names(x) <- letters[1:5]

x$a
```

```
## [1] 1
```

```r
x[["a"]]
```

```
## [1] 1
```


### Subsetting matrices


```r
a <- matrix(1:9, nrow = 3)
colnames(a) <- c("A", "B", "C")
a[1:2, ]
```

```
##      A B C
## [1,] 1 4 7
## [2,] 2 5 8
```

```r
a[c(T, F, T), c("B", "A")]
```

```
##      B A
## [1,] 4 1
## [2,] 6 3
```

```r
a[0, -2]
```

```
##      A C
```


### Subsetting data frames


```r
df <- data.frame(x = 1:3, y = 3:1, z = letters[1:3])
df[df$x == 2, ]
```

```
##   x y z
## 2 2 2 b
```

```r
df[c(1, 3), ]
```

```
##   x y z
## 1 1 3 a
## 3 3 1 c
```

```r

# There are two ways to select columns from a data frame Like a list:
df[c("x", "z")]
```

```
##   x z
## 1 1 a
## 2 2 b
## 3 3 c
```

```r

# Like a matrix
df[, c("x", "z")]
```

```
##   x z
## 1 1 a
## 2 2 b
## 3 3 c
```

```r


# There's an important difference if you select a simple column: matrix
# subsetting simplifies by default, list subsetting does not.
df["x"]
```

```
##   x
## 1 1
## 2 2
## 3 3
```

```r

df[, "x"]
```

```
## [1] 1 2 3
```


### Combining data frames
We have already seen some ways to combine vectors together and create `data.frames` using `rbind` to add rows and `cbind` to combine columns.  You can accomplish in additional ways as well


```r
# Review of creating data frame with rbind() and cbind()
x <- c(1, 2, 3, 4, 5)
y <- c(5, 6, 7, 8, 9)
df <- rbind(x, y)
# coerce to data.frame. rbind creates a matrix with row.names
as.data.frame(df)
```

```
##   V1 V2 V3 V4 V5
## x  1  2  3  4  5
## y  5  6  7  8  9
```

```r
df <- cbind(x, y)
# coerce to data.frame. rbind creates a matrix with row.names
as.data.frame(df)
```

```
##   x y
## 1 1 5
## 2 2 6
## 3 3 7
## 4 4 8
## 5 5 9
```

```r
# you can also use data.frame(), similar results to cbind
df <- data.frame(x, y)
df
```

```
##   x y
## 1 1 5
## 2 2 6
## 3 3 7
## 4 4 8
## 5 5 9
```


These methods work well if your vectors are of the same length, but that is not often the case.  A very common situation is two data frames that share an ID value and you need to merge these together.  In other words, a join.  That cna be accomplished with `merge`.


```r
# create test data.frame 1
df1 <- data.frame(id = c(1:10), data1 = seq(1, 20, 2), data2 = jitter(seq(1, 
    20, 2), 10))

# create test data.frame 2
df2 <- data.frame(id = c(1, 1, 2, 2, 3, 4, 7, 8, 8, 9, 10, 11, 12, 12, 4), data1 = seq(1, 
    30, 2), data2 = jitter(seq(1, 30, 2), 10))

# now we can merge
dfm <- merge(df1, df2, by = "id")
dfm
```

```
##    id data1.x data2.x data1.y data2.y
## 1   1       1  4.3670       1  -1.088
## 2   1       1  4.3670       3   0.805
## 3   2       3 -0.2672       5   1.809
## 4   2       3 -0.2672       7   3.058
## 5   3       5  8.7069       9   5.896
## 6   4       7  5.7604      29  29.393
## 7   4       7  5.7604      11   8.310
## 8   7      13 14.4657      13  11.157
## 9   8      15 15.8341      15  18.961
## 10  8      15 15.8341      17  13.692
## 11  9      17 13.5309      19  21.236
## 12 10      19 15.6919      21  23.216
```

```r
dim(dfm)
```

```
## [1] 12  5
```

```r

# often we want to make sure all of one df is preserved
dfm <- merge(df1, df2, by = "id", all.x = TRUE)
dfm
```

```
##    id data1.x data2.x data1.y data2.y
## 1   1       1  4.3670       1  -1.088
## 2   1       1  4.3670       3   0.805
## 3   2       3 -0.2672       5   1.809
## 4   2       3 -0.2672       7   3.058
## 5   3       5  8.7069       9   5.896
## 6   4       7  5.7604      29  29.393
## 7   4       7  5.7604      11   8.310
## 8   5       9  7.7518      NA      NA
## 9   6      11 14.5120      NA      NA
## 10  7      13 14.4657      13  11.157
## 11  8      15 15.8341      15  18.961
## 12  8      15 15.8341      17  13.692
## 13  9      17 13.5309      19  21.236
## 14 10      19 15.6919      21  23.216
```

```r
dim(dfm)
```

```
## [1] 14  5
```



## Apply Functions

One of the greatest joys of vectorized operations is being able to use the entire family of `apply` functions that are available in base `R`.

These include:


```r
apply
by
lapply
tapply
sapply
```


### apply


```r
m <- matrix(c(1:10, 11:20), nrow = 10, ncol = 2)
# 1 is the row index 2 is the column index
apply(m, 1, sum)
```

```
##  [1] 12 14 16 18 20 22 24 26 28 30
```

```r
apply(m, 2, sum)
```

```
## [1]  55 155
```

```r
apply(m, 1, mean)
```

```
##  [1]  6  7  8  9 10 11 12 13 14 15
```

```r
apply(m, 2, mean)
```

```
## [1]  5.5 15.5
```


### by


```r
head(iris)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```

```r
by(iris[, 1:2], iris[, "Species"], summary)
```

```
## iris[, "Species"]: setosa
##   Sepal.Length   Sepal.Width  
##  Min.   :4.30   Min.   :2.30  
##  1st Qu.:4.80   1st Qu.:3.20  
##  Median :5.00   Median :3.40  
##  Mean   :5.01   Mean   :3.43  
##  3rd Qu.:5.20   3rd Qu.:3.67  
##  Max.   :5.80   Max.   :4.40  
## -------------------------------------------------------- 
## iris[, "Species"]: versicolor
##   Sepal.Length   Sepal.Width  
##  Min.   :4.90   Min.   :2.00  
##  1st Qu.:5.60   1st Qu.:2.52  
##  Median :5.90   Median :2.80  
##  Mean   :5.94   Mean   :2.77  
##  3rd Qu.:6.30   3rd Qu.:3.00  
##  Max.   :7.00   Max.   :3.40  
## -------------------------------------------------------- 
## iris[, "Species"]: virginica
##   Sepal.Length   Sepal.Width  
##  Min.   :4.90   Min.   :2.20  
##  1st Qu.:6.22   1st Qu.:2.80  
##  Median :6.50   Median :3.00  
##  Mean   :6.59   Mean   :2.97  
##  3rd Qu.:6.90   3rd Qu.:3.17  
##  Max.   :7.90   Max.   :3.80
```

```r
by(iris[, 1:2], iris[, "Species"], sum)
```

```
## iris[, "Species"]: setosa
## [1] 421.7
## -------------------------------------------------------- 
## iris[, "Species"]: versicolor
## [1] 435.3
## -------------------------------------------------------- 
## iris[, "Species"]: virginica
## [1] 478.1
```



### tapply
Two examples


```r
df <- data.frame(names = sample(c("A", "B", "C"), 10, rep = T), length = rnorm(10))
tapply(df$length, df$names, mean)
```

```
##      A      C 
## 0.4885 0.3941
```

```r

# Now with a more familiar dataset
tapply(iris$Petal.Length, iris$Species, mean)
```

```
##     setosa versicolor  virginica 
##      1.462      4.260      5.552
```


### lapply (and llply)

What it does: Returns a list of same length as the input. 
Each element of the output is a result of applying a function to the corresponding element.


```r
my_list <- list(a = 1:10, b = 2:20)
lapply(my_list, mean)
```

```
## $a
## [1] 5.5
## 
## $b
## [1] 11
```


### sapply

sapply is a more user friendly version of `lapply` and will return a list of matrix where appropriate.


Let's work with the same list we just created.


```r
my_list
```

```
## $a
##  [1]  1  2  3  4  5  6  7  8  9 10
## 
## $b
##  [1]  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

```r
x <- sapply(my_list, mean)
class(x)
```

```
## [1] "numeric"
```


### replicate

An extremely useful function to generate datasets for simulation purposes. 


```r
replicate(10, rnorm(10))
```

```
##           [,1]      [,2]    [,3]     [,4]     [,5]     [,6]    [,7]
##  [1,]  1.66308  0.044509  1.2416  2.37673 -0.82670  0.44726  0.1814
##  [2,]  0.29835 -0.903444  0.6392  0.02934  1.03842 -1.06205 -0.4398
##  [3,]  2.17281 -0.453990  0.4601 -1.13494 -0.13733 -0.06475 -0.2644
##  [4,]  1.36323  1.068193  1.0089 -0.97083  0.32675 -0.27380 -0.6187
##  [5,] -1.07731  0.446741 -0.2572  1.32514 -0.52586  0.49821 -1.2644
##  [6,]  0.95766  1.028948 -1.6641 -0.91528 -0.09342  0.21262  0.5120
##  [7,] -1.44977  0.420626 -0.4329  1.72711  1.28575 -3.10033 -0.3574
##  [8,]  0.06528  0.005283 -0.3899 -0.74244  0.21826  0.26756  0.4636
##  [9,]  0.72390 -0.276455  0.7795 -1.16906 -2.22665  1.82559 -0.4305
## [10,]  2.13020 -0.919585  1.4928 -0.84508 -0.55438  0.49618  1.4325
##          [,8]    [,9]   [,10]
##  [1,]  0.0564  1.8128 -1.1035
##  [2,] -1.5429  0.4975  1.0418
##  [3,] -1.6458  1.1148 -1.5454
##  [4,]  0.2788  0.3825  2.4103
##  [5,]  1.4706 -0.1871 -0.5678
##  [6,]  0.3102  0.2379 -0.2963
##  [7,]  0.5849 -1.2737 -1.4094
##  [8,] -0.9493 -1.1133 -0.4185
##  [9,]  1.2911 -0.6562 -0.2928
## [10,]  1.8656 -0.3348  0.9275
```

```r
replicate(10, rnorm(10), simplify = TRUE)
```

```
##           [,1]     [,2]    [,3]    [,4]     [,5]     [,6]     [,7]
##  [1,] -0.05062  0.76605  0.4129 -1.3031 -0.63892  0.06684  1.09651
##  [2,] -0.41684 -1.30624 -0.1016  0.8928 -0.07010  0.35660 -0.16446
##  [3,] -2.39829  1.43396  0.7677  0.8303 -0.33554 -1.39478 -0.65685
##  [4,]  0.27723  0.27061 -0.2465  0.3409  0.08316 -0.80134 -0.21248
##  [5,] -1.02388 -0.42223  0.4120  0.9072  0.08739 -0.26016 -0.17814
##  [6,] -0.78666 -0.40045 -0.1169 -0.3124 -0.22550  0.20390  0.07874
##  [7,] -0.80293 -1.31422  1.1578  0.8897 -1.15955 -1.13035 -1.25889
##  [8,] -1.49918 -1.75651 -1.6756  0.5012 -1.76292  0.75512  0.72316
##  [9,] -0.74667 -0.06388  0.8673  0.9095  0.88111 -1.35123 -0.11749
## [10,] -1.74170 -1.07805  0.8819  2.3620  2.06898  0.40648  0.67020
##           [,8]     [,9]   [,10]
##  [1,]  0.35951  0.86754  1.0343
##  [2,]  0.82352  0.60903  0.5952
##  [3,] -1.78886  0.74012 -0.1712
##  [4,] -0.37177  0.49831  1.2945
##  [5,] -0.50439 -0.02069 -1.5190
##  [6,]  3.20718  0.38943 -0.1524
##  [7,]  0.08045  0.45820 -0.7806
##  [8,]  0.19905  0.53624 -2.3006
##  [9,]  1.10417 -1.46239 -0.6767
## [10,] -3.74021  0.93200 -0.1351
```

```r
# The final arguments turns the result into a vector or matrix if possible.
```


### mapply
Its more or less a multivariate version of `sapply`. It applies a function to all corresponding elements of each argument. 

example:


```r
list_1 <- list(a = c(1:10), b = c(11:20))
list_2 <- list(c = c(21:30), d = c(31:40))
mapply(sum, list_1$a, list_1$b, list_2$c, list_2$d)
```

```
##  [1]  64  68  72  76  80  84  88  92  96 100
```



### plyr and reshape

Two packages that are very widely used for data manipulation are `plyr` and `reshape`  The argument for their use is that they simplify many of the common data manipulation tasks, althought they do require a slightly different way of thinking about manipulating data in R.  As such, they are a bit beyond the scope of what we are covering.  But they are widely used enough and powerful enough to bare mention.

For more on these, see Karthik Ram's Canberra Bootcamp Materials on [`plyr` and `reshape`](https://github.com/swcarpentry/2013-10-09-canberra/blob/master/03-data-manipulation/03-split-apply.md)

## Data Manipulation Exercise
In our last exercise, we pulled in three datasets, created data frames from them and answered some basic questions about those data frames.  One of the first things you will notice is that EPA is very fond of large, verbose data structures.  There is a lot more informaiton in these three data frames than we really would like.  The purpose of this exercise is to pull out information we would like to keep, create a single data frame for some future analysis, do some basic analysis, and write the resulting data frame to disc. As before, all steps must be done in R and save your commands in a commented script.

1. Subset your station information data frame so that you only have the following columns: SITE_ID, VISIT_NO, SITE_TYPE, STATE_NAME
2. Subset your water quality information so that you have: SITE_ID, NTL, PTL, CHLA 
3. Susbset your landuse data frame so that you have: SITE_ID, PCT_DEVELOPED_BUFR,  PCT_FOREST_BUFR, PCT_AGRIC_BUFR, and PCT_WETLAND_BUFR.
4. Subset your streamlined verison of station information (created in step 1) so that only the first visit of the probability lakes are returned.  First visits are those records that have a VISIT_NO of 1 and probability lakes are SITE_TYPE of PROB_lake.
5. Create a single data frame that combines the station information from step 4, the water quality info from step 2, and the landuse information from step 3. 
6. How many rows and columns in this new data frame?  Is it what you expected?
7. Calculate the mean, median, min, and max (1st and 3rd quartiles ok too) for each of the data columns.  You can do this many ways, but you have the tools to do it in one, very clean line of code!
8. Once you are certain your new data frame has all the proper rows and columns, write the data frame out to a .csv file.  We will be using this file as input later, so keep track of where you put it.



