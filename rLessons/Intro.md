Intoduction to R and RStudio
========================================================

*Note: This material is a mash-up from Jenny Bryan's [course materials on r] (http://www.stat.ubc.ca/~jenny/STAT545A/quick-index.html) and Karthik Ram's [material from the Canberra Software Carpentry R Bootcamp](https://github.com/swcarpentry/2013-10-09-canberra).  Anything good is there becuase of Jenny and Karthik.  Mistakes are all mine.*

# Getting started with RStudio

RStudio is a free and open source R integrated development environment. It provides a built in editor, works on all platforms (including on servers) and provides many advantages such as integration with version control and project management.


**Basic layout**

  * Console (entire left)
  * Workspace/History (tabbed in upper right)
  * Files/Plots/Packages/Help (tabbed in lower right)

You can change the default location of the panes:
[http://www.rstudio.com/ide/docs/using/customizing#pane-layout](http://www.rstudio.com/ide/docs/using/customizing#pane-layout)

Go into the Console, where we interact directly with the R kernel.

Make an assignment and then inspect the object you just created.


```r
x <- 3 * 4
x
```

```
## [1] 12
```


All R statements where you create objects -- "assignments" -- have this
form:


```r
objectName <- value
```


You will make lots of assignments and the operator `<-` is a pain to
type. Don't be lazy and use `=`, although it would work, because it
will just sow confusion later. Instead, utilize RStudio's keyboard
shortcut:

* In windows and linux, press alt and the minus sign: alt + -
* On Mac OS, press option and the minus sign: alt + -

*Note: This won't work on the web hosted version of RStudio since these shortcuts are used for other purposes*

Notice that RStudio automagically surrounds `<-` with spaces, which
demonstrates a useful code formatting practice. Code is miserable to
read on a good day. Give your eyes a break and use spaces (Note: There is a package called `formatR` that can also help you format code nicely.)

RStudio offers many handy [keyboard shortcuts](http://www.rstudio.com/ide/docs/using/keyboard_shortcuts).

Object names cannot start with a digit and cannot contain certain
other characters such as a comma or a space. You will be wise to adopt a convention for demarcating words in names.

```
iUseCamelCase
other.people.use.periods
even_others_use_underscores
```

R has a mind-blowing collection of built-in functions that are
accessed like so

```
functionName(arg1 = val1, arg2 = val2, and so on)
```

Let's try using `seq()` which helps make regular sequences of numbers
and, while we're at it, demo more helpful features of RStudio.

Type `se` and hit TAB. A pop up shows you possible completions. Specify `seq()` by typing more to disambiguate or using the up/down arrows to select. Notice the floating tool-tip-type help that pops up, reminding you of a function's arguments. If you want even more help, press F1 as directed to get the full documentation in the help tab of the lower right pane. Now open the parentheses and notice the automatic addition of the closing parenthesis and the placement of cursor in the middle.  Type the arguments `1,10` and hit
return. RStudio also exits the parenthetical expression for you.  IDEs
are great.


```r
seq(1, 10)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```


The above also demonstrates something about how R resolves function
arguments. You can always specify in `name = value` form. But if you
do not, R attempts to resolve by position. So above, it is assumed
that we want a sequence `from = 1` that goes `to = 1`. Since we didn't
specify step size, the default value of `by` in the function
definition is used, which ends up being 1 in this case. For functions
I call often, I might use this resolve by position for the first
argument or maybe the first two. After that, I always use `name =
value`.


Make this assignment and notice similar help with quotation marks.


```r
yo <- "hello world"
```


If you just make an assignment, you don't get to see the value, so
then you're tempted to immediately inspect.


```r
y <- seq(1, 10)
y
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```


This common action can be shortened by surrounding the assignment with
parentheses, which causes assignment and "print to screen" to happen.


```r
(y <- seq(1, 10))
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```


Not all functions have (or require) arguments:  


```r
date()
```

```
## [1] "Wed Jan 08 11:16:20 2014"
```

```r
# see list of files in your current directory
dir()
```

```
##  [1] "avgX.txt"       "data-types.png" "Data.html"      "Data.md"       
##  [5] "Data.Rmd"       "DataViz.html"   "DataViz.md"     "DataViz.Rmd"   
##  [9] "figure"         "Functions.html" "Functions.md"   "Functions.Rmd" 
## [13] "Intro.html"     "Intro.md"       "Intro.Rmd"      "myPlot.pdf"    
## [17] "README.html"    "README.md"      "README.Rmd"     "rLessons.Rproj"
## [21] "uriBootcamp"
```

```r
# or
list.files()
```

```
##  [1] "avgX.txt"       "data-types.png" "Data.html"      "Data.md"       
##  [5] "Data.Rmd"       "DataViz.html"   "DataViz.md"     "DataViz.Rmd"   
##  [9] "figure"         "Functions.html" "Functions.md"   "Functions.Rmd" 
## [13] "Intro.html"     "Intro.md"       "Intro.Rmd"      "myPlot.pdf"    
## [17] "README.html"    "README.md"      "README.Rmd"     "rLessons.Rproj"
## [21] "uriBootcamp"
```


Now look at your workspace -- in the upper right pane. The workspace is
where user-defined objects accumulate. You can also get a listing of
these objects with commands:


```r
objects()
```

```
## [1] "x"  "y"  "yo"
```

```r
ls()
```

```
## [1] "x"  "y"  "yo"
```


If you want to remove something you can do this


```r
rm(y)
```


To remove everything:


```r
rm(list = ls())
```


### Workspace and working directory

One day you will need to quit R, go do something else and return to
your analysis later.

One day you will have multiple analyses going that use R and you want
to keep them separate.

One day you will need to bring data from the outside world into R and
send numerical results and figures from R back out into the world.

To handle these real life situations, you need to make two decisions:

* What about your analysis is "real", i.e. you will save it as your
  lasting record of what happened?

* Where does your analysis "live"?

#### Workspace, .RData

As a beginning R user, it's OK to consider your workspace
"real". _Very soon_, I urge you to evolve to the next level, where you consider your saved R scripts as "real". (In either case, of course the input data is very much real and requires preservation!) With theinput data and the R code you used, you can reproduce _everything_. You can make your analysis fancier. You can get to the bottom of puzzling results and discover and fix bugs in your code. You can reuse the code to conduct similar analyses in new projects. You can remake a figure with different aspect ratio or save is as TIFF instead of PDF. You are ready to take questions. You are ready for the
future.

If you regard your workspace as "real" (saving and reloading all the
time), if you need to redo analysis ... you're going to either redo a
lot of typing (making mistakes all the way) or will have to mine your
R history for the commands you used. Rather than [becoming an expert on managing the R history](http://www.rstudio.com/ide/docs/using/history),
a better use of your time and psychic energy is to keeping your "good" R code in a script for future reuse.

Because it can be useful sometimes, note the commands you've recently
run appear in the History pane.

But you don't have to choose right now and the two strategies are not
incompatible. Let's demo the save / reload the workspace approach.

Upon quitting R, you have to decide if you want to save your
workspace, for potential restoration the next time you launch R. Depending on your set up, R or your IDE, eg RStudio, will probably prompt you to make this decision.

Quit R/Rstudio, either from the menu, using a keyboard shortcut, or by typing `q()` in the Console. You'll get a prompt like this:

> Save workspace image to ~/.Rdata?

_Note where the workspace image is to be saved_ and then click `Save`.

Using your favorite method, visit the directory where image was saved
and verify there is a file named `.RData`. You will also see a file
`.Rhistory`, holding the commands submitted in your recent session.

Restart RStudio. In the Console you will see a line like this:


```r
[Workspace loaded from ~/.RData]
```


indicating that your workspace has been restored. Look in the Workspace pane and you'll see the same objects as before. In the History tab of the same pane, you should also see your command history.You're back in business. This way of starting and stopping analytical work will not serve you well for long but it's a start.

#### Working directory

Any process running on your computer has a notion of its "working directory". In R, this is where R will look, by default, for files you ask it to load. It also where, by default, any files you write to disk will go. Chances are your current working directory is the directory we inspected above, i.e. the one where RStudio wanted to save the workspace.

You can explicitly check your working directory with:

```r
getwd()
```

```
## [1] "D:/DATA/DataInformatics/SoftwareCarpentry/2014-01-13-uri/rLessons"
```



```r
# then you can change it with:
setwd("~/Documents/project-name/")
```


It is also displayed at the top of the RStudio console.

As a beginning R user, it's OK let your home directory or any other weird directory on your computer be R's working directory. _Very soon_, I urge you to evolve to the next level, where you organize your analytical projects into directories and, when working on project A, set R's working directory to the associated directory.


You can also use RStudio's Files pane to navigate to a directory and then set it as working directory from the menu: Session --> Set Working Directory --> To Files Pane Location. (You'll see even more options there). Or within the Files pane, choose __More__ and __Set As Working Directory__. 

*Note: This is not a recommended practice since it is not done programatically and harder to replicate.*

But there's a better way. A way that also puts you on the path to managing your R work like an expert.

### RStudio projects

Keeping all the files associated with a project organized together -- input data, R scripts, analytical results, figures -- is such a wise and common practice that RStudio has built-in support for this via it's _projects_.

More info: [http://www.rstudio.com/ide/docs/using/projects](http://www.rstudio.com/ide/docs/using/projects)

Let's make one to use for the rest of this workshop. To do this you can use the `Projects menu` in the upper right corner or from the `File` menu. But let's use what we learned earlier when playing around with the shell first, but we will do it directly from RStudio.


```r
# Check you present working directory
getwd()
```

```
## [1] "D:/DATA/DataInformatics/SoftwareCarpentry/2014-01-13-uri/rLessons"
```

```r

# Now lets use system()
system("mkdir ./uriBootcamp")

# Now, in R move to your new directory
setwd("/uriBootCamp")
getwd()
```

```
## [1] "D:/uriBootCamp"
```



The directory name you choose here will be the project name. Call it whatever you want (or follow me for convenience).

We still need to create an RStudio project, called `uriBootcamp`.  Use the dropdown in the upper right or `File:New Project:Existing Directory.` Browse to the directory we just created and click on `Create Project` 

Now double check that the "home" directory for your project is the working directory of our current R process:


```r
getwd()
```

```
## [1] "D:/DATA/DataInformatics/SoftwareCarpentry/2014-01-13-uri/rLessons"
```


Let's enter a few commands in the Console, as if we are just beginning an analytical project:


```r
a <- 2
b <- -3
sigSq <- 0.5
x <- runif(40)
y <- a + b * x + rnorm(40, sd = sqrt(sigSq))
(avgX <- mean(x))
```

```
## [1] 0.4786
```

```r
write(avgX, "avgX.txt")
plot(x, y, xlab = "myX", ylab = "myY", main = "My First Plot")
abline(a, b, col = "purple")
```

![plot of chunk unnamed-chunk-16](figure/unnamed-chunk-16.png) 

```r
dev.print(pdf, "myPlot.pdf")
```

```
## pdf 
##   2
```


Let's say this is a good start of an analysis and your ready to start preserving the logic and code. Visit the History tab of the upper right pane. Select these commands. Click "To Source". Now you have a new pane containing a nascent R script. Click on the floppy disk to save. Give it a name ending in `.R`, I used `projectStartDemo.R` and note that, by default, it will go in the directory associated with your project.

Quit RStudio. Inspect the folder associate with your project if you wish. Maybe view the PDF in an external viewer.

Restart RStudio. Notice that things, by default, restore to where we were earlier, e.g. objects in the workspace, the command history, which files are open for editing, where we are in the file system browser, the working directory for the R process, etc. These are all Good Things.

Change some things about your code. Top priority would be to set a sample size `n` at the top, e.g. `n <- 40`, and then replace all the hard-wired 40's with `n`. Change some other minor-but-detectable stuff, i.e. alter the sample size `n`, the slope of the line `b`,the color of the line ... whatever. Practice the different ways to re-run the code:
  * Walk through line by line by keyboard shortcut (command + enter) or mouse (click Run in the upper right corner of editor pane).
  * Source the entire document -- equivalent to entering `source('~/uriBootcamp/projectStartDemo.R')` in the Console -- by keyboard shortcut (shift command S) or mouse (click Source in the upper right corner of editor pane or select from the mini-menu accessible from the associated down triangle).
  * Source with echo from the Source mini-menu.
  
Visit your figure in an external viewer to verify that the PDF is changing as you expect.

In your favorite OS-specific way, search your files for "myPlot.pdf" and presumably you will find the PDF itself (no surprise) but _also the script that created it (`uriBootcamp/projectStartDemo.R`)_. This latter phenomenon is a huge win. One day you will want to remake a figure or just simply understand where it came from. If you rigorously save figures to file __with R code and not ever ever ever the mouse or the clipboard__, you will sing my praises one day. Trust me.

### Organizing code as scripts

It is traditional to save R scripts with a `.R` or `.r` suffix. Follow this convention unless you have some extraordinary reason not to. 

Comments start with one or more `#` symbols. Use them. RStudio helps you (de)comment selected lines with `Ctrl + Shift + C` (windows and linux) or `Command + Shift + C` (mac).

Clean out the workspace, ie pretend like you've just revisited this project after a long absence.  The broom icon or `rm(list = ls())`. Good idea to do this, restart R (available from the Session menu), re-run your analysis to truly check that the code you're saving is complete and correct (or at least rule out obvious problems!).

This workflow will serve you well in the future:

* Create an RStudio project for an analytical project
* Keep inputs there (we'll soon talk about importing)
* Keep scripts there; edit them, run them in bits or as a whole from
  there
* Keep outputs there (like the PDF written above)

Avoid using the mouse for pieces of your analytical workflow, such as loading a dataset or saving a figure. Terribly important for reproducility and for making it possible to retrospectively determine how a numerical table or PDF was actually produced (searching on local disk on filename, among .R files, will lead to the relevant script).

Many long-time users never save the workspace, never save `.RData` files (I'm one of them), never save or consult the history. Once/if you get to that point, there are options available in RStudio to disable the loading of .RData and permanently suppress the prompt on exit to save the workspace (go to Tools->Options->General).

### Packages and Package management

R is an extensible system and many people share useful code they have developed as a _package_ via CRAN and/or github. To install a package from CRAN, for example the [`ggplot2`](http://ggplot2.org) a very popular package for data visualization, here is one way to do it in the R console (there are others).


```r
install.packages("ggplo2", dependencies = TRUE)
```

```
## Error: trying to use CRAN without setting a mirror
```

We will use this package tomorrow, so go ahead and install it!

Another package we won't have time to cover in this bootcamp [`knitr`](http://yihui.name/knitr/), which facilitates the creation of dynamic reports. You can install it in the same way.
```
  install.packages("knitr", dependencies = TRUE)
```

Packages are updated frequently, thus the packages you have installed may become out of date.  Two functions can help you manage your installed pakcages. Use `old.packages()` to keep track of what's out of date.  `update.packages()` - with package name will update a single package. Otherwise it will update all interactively. This can take a while if you haven't done it recently. To update everything without any user intervention, use the `ask = F` argument.


```r
update.packages(ask = FALSE)
```


**Quitting R**

At some point you may want to stop using R (never!) To do so, type in `quit()` or `q()` and answer `Y` to quit.

---

# Getting Help


## Diagnostic functions in R

**Super helpful functions**  
* `str()` - Compactly display the internal structure of an R object. Perhaps the most uesful diagnostic function in R.
* `class()` *Retrieves the internal class of an object*
* `mode()` *Get or set the type or storage mode of an object.*
* `length()` *Retrieve or set the dimension of an object.*  
* `dim()` *Retrieve or set the dimension of an object.*
* `R -- vanilla` - *Allows you to start a clean session of R. A great way to test whether your code is reproducible.*
* `sessionInfo()` *Print version information about R and attached or loaded packages.*  
* `options()` *Allow the user to set and examine a variety of global options which affect the way in which R computes and displays its results.*

`str()` is your best friend

`str` is short for structure. You can use it on any object. Try the following:


```r
x <- 1:10
class(x)
```

```
## [1] "integer"
```

```r
mode(x)
```

```
## [1] "numeric"
```

```r
str(x)
```

```
##  int [1:10] 1 2 3 4 5 6 7 8 9 10
```





---

## Seeking help

There are various ways to seek help on R.

If you are searching for help on a specific function that is in a package loaded into your namespace:

```
?function_name
```

If you're not sure what package it belongs to:

```
??function_name
```

This will search across all installed packages in your library and pop up several options

Another recent package that's really useful in this context is called `Rdocumentation`. It searches across all packages on CRAN even if you do not have it installed locally.

![](https://github.com/swcarpentry/2013-10-09-canberra/raw/master/01-R-basics/rdocumentation.png)

to install:


```r
library("devtools")
install_github("Rdocumentation", "jonathancornelissen")
```

```
## Installing github repo Rdocumentation/master from jonathancornelissen
## Downloading Rdocumentation.zip from https://github.com/jonathancornelissen/Rdocumentation/archive/master.zip
## Installing package from C:\Users\jhollist\AppData\Local\Temp\2\RtmpyOlBh8/Rdocumentation.zip
## Installing Rdocumentation
## "C:/PROGRA~1/R/R-30~1.2/bin/x64/R" --vanilla CMD INSTALL  \
##   "C:\Users\jhollist\AppData\Local\Temp\2\RtmpyOlBh8\devtools228c8f66bf3\Rdocumentation_package-master"  \
##   --library="C:/Program Files/R/R-3.0.2/library" --install-tests
```

```r
library("Rdocumentation")
```

```
## 
## Attaching package: 'Rdocumentation'
## 
## The following objects are masked from 'package:utils':
## 
##     ?, help
```

```r
# then all ?function searches go through the web If you do load this package
# and want to remove it because of lack of internet, use
detach("Rdocumentation")
```

```
## Error: invalid 'name' argument
```



## Seeking help from peers

* Always share some example data to help others replicate your problem.

for example, the function `dput()` can help recreate R objects by simply pasting the output into another R terminal. Just `dput` a few rows for testing purposes.

e.g.


```r
dput(head(iris))
```

```
## structure(list(Sepal.Length = c(5.1, 4.9, 4.7, 4.6, 5, 5.4), 
##     Sepal.Width = c(3.5, 3, 3.2, 3.1, 3.6, 3.9), Petal.Length = c(1.4, 
##     1.4, 1.3, 1.5, 1.4, 1.7), Petal.Width = c(0.2, 0.2, 0.2, 
##     0.2, 0.2, 0.4), Species = structure(c(1L, 1L, 1L, 1L, 1L, 
##     1L), .Label = c("setosa", "versicolor", "virginica"), class = "factor")), .Names = c("Sepal.Length", 
## "Sepal.Width", "Petal.Length", "Petal.Width", "Species"), row.names = c(NA, 
## 6L), class = "data.frame")
```

```r

structure(list(Sepal.Length = c(5.1, 4.9, 4.7, 4.6, 5, 5.4), Sepal.Width = c(3.5, 
    3, 3.2, 3.1, 3.6, 3.9), Petal.Length = c(1.4, 1.4, 1.3, 1.5, 1.4, 1.7), 
    Petal.Width = c(0.2, 0.2, 0.2, 0.2, 0.2, 0.4), Species = structure(c(1L, 
        1L, 1L, 1L, 1L, 1L), .Label = c("setosa", "versicolor", "virginica"), 
        class = "factor")), .Names = c("Sepal.Length", "Sepal.Width", "Petal.Length", 
    "Petal.Width", "Species"), row.names = c(NA, 6L), class = "data.frame")
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



* Use the `sessionInfo()` function to share your current namespace and package versions. Super helpful for others to help debug your issues.

The `knitr` function `stitch()` automatically includes this information. Try it on any example R script.


```r
stitch("my_script.R")
```

```
## Warning: cannot open file 'my_script.R': No such file or directory
```

```
## Error: cannot open the connection
```



## search StackOverflow

9 times out of 10, the answers you are seeking have already been answered on stack overflow. Search using the `[r]`

[http://stackoverflow.com/questions/tagged/r](http://stackoverflow.com/questions/tagged/r)

![](https://github.com/swcarpentry/2013-10-09-canberra/raw/master/01-R-basics/stackoverflow.png)


## Other resources

CRAN task Views: [http://cran.at.r-project.org/web/views](http://cran.at.r-project.org/web/views)

R mailing lists:
 - [R-help](https://stat.ethz.ch/mailman/listinfo/r-help)
 - [R-sig-ecology](https://stat.ethz.ch/mailman/listinfo/r-sig-ecology)
 



# Some useful links

[Working in the console (RStudio)](http://www.rstudio.com/ide/docs/using/console)

[RStudio keyboard shortcuts](http://www.rstudio.com/ide/docs/using/keyboard_shortcuts)

[Big list of RStudio documentation](http://www.rstudio.com/ide/docs/)
