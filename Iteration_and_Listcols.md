Iteration and Listcols
================
Briana Lettsome
November 4, 2018

Lecture Octuber30th
===================

Lists
-----

``` r
l = list(vec_numeric = 5:8,
         mat         = matrix(1:8, 2, 4),
         vec_logical = c(TRUE, FALSE),
         summary     = summary(rnorm(1000)))

l
## $vec_numeric
## [1] 5 6 7 8
## 
## $mat
##      [,1] [,2] [,3] [,4]
## [1,]    1    3    5    7
## [2,]    2    4    6    8
## 
## $vec_logical
## [1]  TRUE FALSE
## 
## $summary
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
## -3.00805 -0.69737 -0.03532 -0.01165  0.68843  3.81028
# A list is some arbitrary collection for information. 
# I can put whatever I want in a list.

# I can access the elements of this list by putting the '$' notation:

l$vec_numeric
## [1] 5 6 7 8
# Double brackets can be use to eextrace either 1st, 1nd, or 3rd element:
l[[1]]
## [1] 5 6 7 8

l[[2]]
##      [,1] [,2] [,3] [,4]
## [1,]    1    3    5    7
## [2,]    2    4    6    8

l[[3]]
## [1]  TRUE FALSE
```

for loops
---------

``` r

df = data_frame(
  a = rnorm(20, 3, 1),
  b = rnorm(20, 0, 5),
  c = rnorm(20, 10, .2),
  d = rnorm(20, -3, 1)
)

is.list(df)
## [1] TRUE
# Above code is asking "is this a list?"

df[[1]]
##  [1] 4.134965 4.111932 2.129222 3.210732 3.069396 1.337351 3.810840
##  [8] 1.087654 1.753247 3.998154 2.459127 2.783624 1.378063 1.549036
## [15] 3.350910 2.825453 2.408572 1.665973 1.902701 5.036104
# Will give me that 1st vetor

df[[2]]
##  [1] -1.63244797  3.87002606  3.92503200  3.81623040  1.47404380
##  [6] -6.26177962 -5.04751876  3.75695597 -6.54176756  2.63770049
## [11] -2.66769787 -1.99188007 -3.94784725 -1.15070568  4.38592421
## [16]  2.26866589 -1.16232074  4.35002762  8.28001867 -0.03184464
# Will give me that 2nd vector
```

Let me get a function

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

I can apply to my dataframe.

``` r
 mean_and_sd(df[[1]])
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1  2.70  1.12
# This should use mean and std on 1st column in my dataframe or, equivalently, 1st list element in this object

# Can repeat for the other list elements
mean_and_sd(df[[2]])
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1 0.416  4.08
mean_and_sd(df[[3]])
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1  10.1 0.191
mean_and_sd(df[[4]])
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1 -3.43  1.18

# This makes the code chunk lookt a bit chunky, so this is a problem. Need a cleaner way
```

Write a for loop

``` r
output = vector("list", length = 4)

# The '4' is the same length as my input object, my input object is a list of length '4', "I have 4 columns in my dataframe"

output[[1]] = mean_and_sd(df[[1]])
output[[2]] = mean_and_sd(df[[2]])
output[[3]] = mean_and_sd(df[[3]])
output[[4]] = mean_and_sd(df[[4]])

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(df[[i]])
}
# now function is being applied to every element in the list
```

Map statements
--------------

Using map to replace the 'for' loop with 'map

``` r
output = map(df, mean_and_sd)
# df will be treated as a list
```

``` r
df %>%
  select(a, b, c) %>%
  map(mean_and_sd)
## $a
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1  2.70  1.12
## 
## $b
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1 0.416  4.08
## 
## $c
## # A tibble: 1 x 2
##    mean    sd
##   <dbl> <dbl>
## 1  10.1 0.191
# Applying map function to selelct columns
```

Let's me try a different function

``` r
output = map(df, median)

# computes median for 1st element and subsequent elements

output = map(df, summary)
```

Map variant
-----------

``` r
output = map_df(df, mean_and_sd)

# This makes a dataframe of my output

output = map_dbl(df, median)
```

Code syntax (45:50)
-------------------

Be clear about arguments

``` r
output = map(.x = df, ~mean(x = .x, na.rm = FALSE))
```
