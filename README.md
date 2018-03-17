
<!-- README.md is generated from README.Rmd. Please edit that file -->
TidyPlusR: a tool for data wrangling
====================================

[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/dwyl/esta/issues)

[![HitCount](https://hitt.herokuapp.com/tidyplus_python.svg..)](https://github.com/tidyplus_python)

[![GitHub commit](https://img.shields.io/github/commits-since/UBC-MDS/tidyplus_python/v0.svg)](https://github.com/UBC-MDS/tidyplus_python/commit)

[![Downloads](https://img.shields.io/github/downloads/UBC-MDS/tidyplus_python/total.svg)](https://github.com/UBC-MDS/tidyplus_python/graphs/traffic)

[![forks](https://img.shields.io/github/forks/UBC-MDS/tidyplus_python.svg)](https://github.com/UBC-MDS/tidyplus_python/network)

[![issues](https://img.shields.io/github/issues/UBC-MDS/tidyplus_python.svg)](https://github.com/UBC-MDS/tidyplus_python/issues)

[![Build Status](https://travis-ci.org/UBC-MDS/tidyplusR.svg?branch=master)](https://travis-ci.org/UBC-MDS/tidyplusR)

Contributors:
-------------

-   `Akshi Chaudhary` : [akshi8](https://github.com/akshi8)
-   `Tina Qian` : [TinaQian2017](https://github.com/TinaQian2017)
-   `Xinbin Huang`: [xinbinhuang](https://github.com/xinbinhuang)

Latest
------

-   Date : March 18, 2018
-   Release : v4

About
-----

The `tidyplusR` package is an essential data cleaning package with features like missing value treatment, data manipulation and displaying data as markdown table for documents. The package adds a few additional functionality on the existing data wrangling packages in popular statistical software like R. The objective of this package is to provide a few specific functions to solve some of the pressing issues in data cleaning.

Installation
------------

You can install tidyplusR from github with:

``` r
# install.packages("devtools")
devtools::install_github("UBC-MDS/tidyplusR")
```

Functions included:
-------------------

> Three main parts including different functions in `tidyplusR`

-   `Data Manipulation` : Datatype cleansing
-   `typemix`
    -   The function helps to find the columns containing different types of data, like character and numeric. The input of the function is a data frame, and the output of the function will be a list of 3 data frames.
-   `cleanmix`
    -   The function helps to clean our data frame. After knowing the location of discrepancy of data types, one can use this function to keep a type of data in certain columns.
    -   Here, the input will be the output by `typemix` function, name of the column (a vector of the name of columns) that they want to clean, the type of data they want to work on, and if we want to keep or delete the certain type. The output will be a data frame like the original type but with specified data type in certain columns deleted.
-   `Missing Value Treatment` : Basic Imputation using `impute`

    -   Imputation: replace missing values in a column of a dataframe, or multiple columns of dataframe based on the `method` of imputation

    -   `(Method = 'Mean')` replace using mean
    -   `(Method = 'Median')` replace using median
    -   `(Method = 'Mode')` replace using mode

-   `Markdown Table`:

-   `md_new()`: This function creates a bare bone for generating a markdown table. Alignments, and size of the table can be input by users.
    -   Input: the size of table (number of rows and number of columns)
    -   Output: a character vector of the source code.
-   `md_data()`: This function converts a dataframe or matrix into a markdown table format.
    -   Input: a matrix or dataframe
    -   Output: a character vector of the source code.

Example
-------

This is a basic example which shows you how to solve a common problem:

#### Datatype cleansing

The section has two functions, typemix and cleanmix.

-   The input for typemix function is a `data frame`, and the output is a list of 3 data frames. The first one is the same as the input data frame, the second one tells you the location and types of data in the columns where there is type mixture. The third data frame is a summary of the second data frame.

-   The input for cleanmix function is the result from typemix function, the column(s) you want to work on, the type(s) of data you want to keep/delete, and if you want to keep/delete the instances specified.

``` r
library(tidyplusR)
dat<-data.frame(x1=c(1,2,3,"1.2.3"),
                x2=c("test","test",1,TRUE),
                x3=c(TRUE,TRUE,FALSE,FALSE))

typemix(dat) #
```

    ## [[1]]
    ##      x1   x2    x3
    ## 1     1 test  TRUE
    ## 2     2 test  TRUE
    ## 3     3    1 FALSE
    ## 4 1.2.3 TRUE FALSE
    ## 
    ## [[2]]
    ##          x1        x2 x3
    ## 1    number character NA
    ## 2    number character NA
    ## 3    number    number NA
    ## 4 character   logical NA
    ## 
    ## [[3]]
    ##   Column_ID number character logical
    ## 1         1      3         1       0
    ## 2         2      1         2       1

``` r
cleanmix(typemix(dat),column=c(1,2),type=c("number","character"))
```

    ##     x1   x2    x3
    ## 1    1 test  TRUE
    ## 2    2 test  TRUE
    ## 3    3 <NA> FALSE
    ## 4 <NA> <NA> FALSE

#### Missing Value imputation

-   This function requires a `dataframe` as an input for missing value treatment using mean/median/mode

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 2.2.1     v purrr   0.2.4
    ## v tibble  1.4.2     v dplyr   0.7.4
    ## v tidyr   0.8.0     v stringr 1.2.0
    ## v readr   1.1.1     v forcats 0.2.0

    ## -- Conflicts -------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# Dummy dataframe
dat <- data.frame(x=sample(letters[1:3],20,TRUE), 
                  y=sample(letters[1:3],20,TRUE),
                  w=as.numeric(sample(0:50,20,TRUE)),
                  z=sample(letters[1:3],20,TRUE), 
                  b = as.logical(sample(0:1,20,TRUE)),
                  a=sample(0:100,20,TRUE),
                  stringsAsFactors=FALSE)

dat[c(5,10,15),1] <- NA
dat[c(3,7),2] <- NA
dat[c(1,3,5),3] <- NA
dat[c(4,5,9),4] <- NA
dat[c(4,5,9),5] <- NA
dat[,4] <- factor(dat[,4] )
dat[c(4,5,9),6] <- NA

# Calling impute function
# method can be replaced by median and mean as well

impute(dat,method = "mode") %>% head()
```

    ##   x y    w z     b     a
    ## 1 b a 16.6 a FALSE 29.00
    ## 2 a a 46.0 c  TRUE  6.00
    ## 3 b a 16.6 c FALSE 10.00
    ## 4 c c 22.0 a  TRUE 11.81
    ## 5 c a 16.6 a  TRUE 11.81
    ## 6 a a 12.0 a FALSE 14.00

#### Markdown table

-   `md_new()` can create an empty markdown table by specifying the number of columns and number of rows.

``` r
## default: ncol = 2 and nrow = 2, alignment = "l"
md_new()
```

    ## 
    ## |    |    |
    ## |:---|:---|
    ## |    |    |
    ## |    |    |

``` r
## 3 by 3 table
md_new(nrow = 3, ncol = 3)
```

    ## 
    ## |    |    |    |
    ## |:---|:---|:---|
    ## |    |    |    |
    ## |    |    |    |
    ## |    |    |    |

``` r
## different alignments:
md_new(nrow = 1, align = "c")
```

    ## 
    ## |    |    |
    ## |:--:|:--:|
    ## |    |    |

``` r
md_new(nrow = 1, align = "r")
```

    ## 
    ## |    |    |
    ## |---:|---:|
    ## |    |    |

``` r
## providing header
h <- c("foo", "boo")
md_new(header = h)
```

    ## 
    ## | foo| boo|
    ## |:---|:---|
    ## |    |    |
    ## |    |    |

-   `md_data()` can create an markdown table given input as matrix of data frame.

``` r
md_data(mtcars, row.index = 1:3, col.index = 1:4)
```

    ## 
    ## |    |mpg|cyl|disp|hp|
    ## |:---|---:|---:|---:|---:|
    ## |Mazda RX4|21.0|6|160|110|
    ## |Mazda RX4 Wag|21.0|6|160|110|
    ## |Datsun 710|22.8|4|108|93|

``` r
## alignment to right
md_data(mtcars, row.index = 1:3, col.index = 1:4, align = "r")
```

    ## 
    ## |    |mpg|cyl|disp|hp|
    ## |:---|---:|---:|---:|---:|
    ## |Mazda RX4|21.0|6|160|110|
    ## |Mazda RX4 Wag|21.0|6|160|110|
    ## |Datsun 710|22.8|4|108|93|

``` r
## provide header
md_data(mtcars, row.index = 1:3, col.index = 1:4, header = c("a","b","c","d"))
```

    ## 
    ## |    |a|b|c|d|
    ## |:---|---:|---:|---:|---:|
    ## |Mazda RX4|21.0|6|160|110|
    ## |Mazda RX4 Wag|21.0|6|160|110|
    ## |Datsun 710|22.8|4|108|93|

``` r
## not include row names
md_data(mtcars, row.index = 1:3, col.index = 1:4, row.names = F)
```

    ## 
    ## |mpg|cyl|disp|hp|
    ## |---:|---:|---:|---:|
    ## |21|6|160|110|
    ## |21|6|160|110|
    ## |22.8|4|108|93|

Used Scenario
-------------

> Using Data Manipulation functionality

-   Users can use the package when they want to clean and wrangle their data. For example, if the data has not been cleaned yet, users can use function `typemix` to check where data is not clean and use `cleanmix` to clean data. Based on personal work experience, the mix of number and character is usually seen in the data collected from the survey. After clean data is ready, one can use the `Missing Value Treatment` to deal with missing data by EM algorithm. The `emphasizeon` function can be used to highlight the factors that he is interested in. After the wrangling of data, one can use function `Markdown Table` to output the data frame in a markdown format.

Existing features in R and Python ecosystem similar to `tidyplus`
-----------------------------------------------------------------

-   Data Manipulation
-   [dplyr](https://cran.r-project.org/web/packages/dplyr/vignettes/dplyr.html) and [tidyverse](https://cran.r-project.org/web/packages/tidyverse/index.html) these R libraries have very powerful data wrangling tools but with `tidyplus` user can explicitly perform string processing/ datatype conversion without affecting the overall column type (which is convenient when you have really messed up data with mix of strings and numbers)

-   Missing Value treatment
-   R doesn't have imputation methods which use `Mode` for missing value treatment, which can be useful for categorical and numeric variables [MICE](https://cran.r-project.org/web/packages/mice/index.html) package in R do provide limited imputation using mean, median, etc.
-   Markdown table in R
-   R has library [`Kable`](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html) which can output a dataset in the form of a markdown table but with `tidyplus` user will have more freedom with data types and formatting.

Ideas subject to change
-----------------------

-   As a part of the initial proposal, the above ideas can be implemented. However, some functionality are subject to change based on the project timeline or technical complexity

License
-------

[MIT](LICENSE.md)

Contributing
------------

This is an open source project. Please follow the guidelines below for contribution. - Open an issue for any feedback and suggestions. - For contributing to the project, please refer to [Contributing](CONTRIBUTING.md) for details.
