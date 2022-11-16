Lab9
================
Chen Chen 6381370662
2022-11-15

## Problem 2.

Creat a n x k matrix of Poission variables with mean lambda

``` r
set.seed(1235)
fun1 <- function(n = 100, k = 4, lambda = 4) {
  x <- NULL
  
  for (i in 1:n)
    x <- rbind(x, rpois(k, lambda))
  
  return(x)
}
f1 <- fun1(100,4)
mean(f1)
```

    ## [1] 4.1575

``` r
fun1alt <- function(n = 100, k = 4, lambda = 4) {
  # YOUR CODE HERE
  
  x<- matrix( rpois(n*k, lambda), ncol =4)
  
  return(x)
}
f1 <- fun1alt(50000,4)

# Benchmarking
microbenchmark::microbenchmark(
  fun1(),
  fun1alt()
)
```

    ## Unit: microseconds
    ##       expr    min       lq      mean   median       uq      max neval
    ##     fun1() 335.82 527.8500 562.84365 544.7315 571.4855 1011.904   100
    ##  fun1alt()  20.25  22.9835  48.81108  24.2985  26.7700 2325.480   100

``` r
d <- matrix(1:16, ncol =4)
d
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    5    9   13
    ## [2,]    2    6   10   14
    ## [3,]    3    7   11   15
    ## [4,]    4    8   12   16
