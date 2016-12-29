![R logo](https://www.r-project.org/Rlogo.png)

# Getting started with R

R is a free, open-source high-level programming language and software environment used primarily for statistical computing and graphics. 

## R at Healthify:
R is currently used for web scraping and data processing tasks.

## First steps:
Start by downloading [R](http://cran.r-project.org/) and [RStudio](http://www.rstudio.com/ide/), the R IDE (optional, but highly recommended).

## Getting help:
```
help.start()              # Load HTML help pages into browser
help(package)             # List help page for "package"
?package                  # Shortform for "help(package)"
help.search("keyword")    # Search help pages for "keyword"
?help                     # For more options
help(package=base)        # List tasks in package "base"
demo()                    # List topics for which demos are available
q()                       # Quit R
```

## Useful links:
- [R](https://www.r-project.org/) homepage
- Homepage of [CRAN](https://cran.r-project.org/), the Comprehensive R Archive Network
- [R cheat sheet](https://cran.r-project.org/doc/contrib/Short-refcard.pdf)
- [R style guide](https://google.github.io/styleguide/Rguide.xml) by Google
- [Rdocumentation.org](https://www.rdocumentation.org), a searchable database of R documentation
- [R-Bloggers](https://www.r-bloggers.com/how-to-learn-r-2/), an R blog aggregator
- [Stack Overflow](https://stackoverflow.com/questions/tagged/r), obviously
- The official tl;dr [Introduction to R](https://cran.r-project.org/doc/manuals/R-intro.html)
- [Cross Validated](https://stats.stackexchange.com/questions/tagged/r), a Q&A site on statistics, machine learning, data analysis, data mining, and data visualization 
- Coursera's [R Programming course](https://www.coursera.org/learn/r-programming)

## Unit testing in R:

We are using R package `RUnit` for unit testing.

### Links:
- [RUnit homepage](https://cran.r-project.org/web/packages/RUnit/index.html)
- [RUnit manual](https://cran.r-project.org/web/packages/RUnit/RUnit.pdf)
- [RUnit primer](https://cran.r-project.org/web/packages/RUnit/vignettes/RUnit.pdf)

### Example:
Let's assume we want to test a function `SquareInteger` that is stored in file `my_file.R`.  
To test function `SquareInteger`, we create a directory called `tests` that will store all of our test cases.  
In `tests`, we create a file called `runit_square_integer.R` that will contain our first set of tests.  
Each set of tests will go in a function inside of `runit_square_integer.R`, named with prefix `Test`, in this example `TestSquareInteger`.

Example for `runit.R`:

```
TestSquareInteger <- function() {
  checkEquals(1, SquareInteger(1)) # checks if SquareInteger(1) returns 1
  checkEquals(4, SquareInteger(2)) # checks if SquareInteger(2) returns 4
  checkEquals(9, SquareInteger(3)) # checks if SquareInteger(3) returns 9
  checkException(SquareInteger('foo'), 'Unable to square a string')
}
```
To run this set of tests, we need to create a file (arbitrarily) called `run_tests.R` that will act as a test suite and invoke all of the tests in your `tests` directory:

```
library('RUnit')
source('my_file.R')
test.suite <- defineTestSuite(name = 'foobar',
                              dirs = file.path('tests'),
                              testFileRegexp = '^runit\\.R$')
test.result <- runTestSuite(test.suite)
printTextProtocol(test.result)
```
