![R logo](https://www.r-project.org/Rlogo.png)

# Getting started with R

R is a free and open-source, high-level programming language and software environment used primarily for statistical computing and graphics. 

## R at Healthify:
R is currently used for web scraping and data processing tasks.

## First steps:
Start by downloading and installing [R](http://cran.r-project.org/) and [RStudio](http://www.rstudio.com/ide/), the R IDE (optional, but highly recommended).  
Alternatively, run from your terminal:

```
$ brew tap science
$ brew install r
$ brew cask install rstudio
```

After installing R, you can open the R shell.  
In your terminal, simply use:

```
$ R
```
You will see something like this:

```
R version 3.3.1 (2016-06-21) -- "Bug in Your Hair"
Copyright (C) 2016 The R Foundation for Statistical Computing
Platform: x86_64-apple-darwin15.6.0 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

>
```

To run an existing R script from your terminal, use:

```
$ Rscript your_r_file_here.R
```

## Getting help:
The following R commands (as well as any other R command) work both in the R shell and in RStudio:

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

## Useful links to get started:
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

## Unit testing with package RUnit:

### Links to get started:
- [RUnit homepage](https://cran.r-project.org/web/packages/RUnit/index.html)
- [RUnit manual](https://cran.r-project.org/web/packages/RUnit/RUnit.pdf)
- [RUnit primer](https://cran.r-project.org/web/packages/RUnit/vignettes/RUnit.pdf)

### Example:
To test a function `SquareInteger` in file `my_file.R`:

1. Create a directory called `tests`
2. Create a file `tests/runit_square_integer.R` with function `TestSquareInteger` (code see below)
3. Create test script `run_tests.R` (code see below)
4. Run test script `run_tests.R`

Code of `tests/runit_square_integer.R`:

```
TestSquareInteger <- function() {
  checkEquals(1, SquareInteger(1)) # checks if SquareInteger(1) returns 1
  checkEquals(4, SquareInteger(2)) # checks if SquareInteger(2) returns 4
  checkEquals(9, SquareInteger(3)) # checks if SquareInteger(3) returns 9
  checkException(SquareInteger('foo'), 'Unable to square a string')
}
```

Code of `run_tests.R`:

```
library('RUnit')
source('my_file.R')
test.suite <- defineTestSuite(name = 'foobar',
                              dirs = file.path('tests'),
                              testFileRegexp = '^runit\\.R$')
test.result <- runTestSuite(test.suite)
printTextProtocol(test.result)
```
