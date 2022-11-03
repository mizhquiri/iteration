Writing Functions
================

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## 
    ## Attaching package: 'rvest'
    ## 
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

## Z - scores!!

Let’s compute the z score version of a list of numbers. - testing
incrementally, mean = 0 and variance = 1

``` r
x_vec = rnorm(25, mean = 7, sd = 4)


(x_vec - mean(x_vec))/ sd(x_vec)
```

    ##  [1] -0.54237274 -1.40365042  0.96366020 -0.16163644  0.81583950 -0.51036261
    ##  [7]  0.93832124  0.06644563 -0.89237255 -0.52976052  0.58093456 -1.26973855
    ## [13] -0.64676387 -1.37072437 -1.05270531  0.30858015 -0.02887161 -1.33667097
    ## [19]  0.82482530 -0.92025672  1.86121829  1.33286328  0.83892384  0.39413597
    ## [25]  1.74013871

Suppose you want to do this often.

``` r
z_scores = function(ARGUMENTS) {
  
  BODY OF FUNCTION
}
```

Here, you are telling function R you have a function X - instead of
return you could put z

``` r
z_scores = function(x) {
 z =  (x - mean(x))/sd(x)
 return(z)
}
```

Now you have a z score function

``` r
z_scores(x = x_vec)
```

    ##  [1] -0.54237274 -1.40365042  0.96366020 -0.16163644  0.81583950 -0.51036261
    ##  [7]  0.93832124  0.06644563 -0.89237255 -0.52976052  0.58093456 -1.26973855
    ## [13] -0.64676387 -1.37072437 -1.05270531  0.30858015 -0.02887161 -1.33667097
    ## [19]  0.82482530 -0.92025672  1.86121829  1.33286328  0.83892384  0.39413597
    ## [25]  1.74013871
