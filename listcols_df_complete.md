Iteration and list columns
================

------------------------------------------------------------------------

## Lists

``` r
vec_numeric = 5:8
vec_logical = c(TRUE, FALSE, TRUE, TRUE)
```

Lets look at a list

``` r
l = list(
  vec_numeric = 5:8,
  mat         = matrix(1:8, 2, 4),
  vec_logical = c(TRUE, FALSE),
  summary     = summary(rnorm(1000))
)
```

Accessing list items

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[3]]
```

    ## [1]  TRUE FALSE

``` r
l[["mat"]]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8

## Loops!

Let’s write a `for` loop to take the mean and SD of four samples from a
normal distribution

rnorm(n,mean = ?, sd = 1)

``` r
list_norm = 
  list(
    a = rnorm(20, 5, 4),
    b = rnorm(20, -12, 3),
    c = rnorm(20, 17, .4),
    d = rnorm(20, 100, 1)
  )
```

Here’s my function

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Z scores only work for numbers")
  }
  
  if (length(x) < 3) {
    stop("Z scores really only work if you have three or more numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

Let’s try to make this work. - right now I am not saving the results -
do you really want to repeat each input by copying and pasting?

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.46  4.48

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1  3.63

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.385

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.7  1.25

Let’s a `for` loop instead.

-   make it scalable’ -keep track of input and output

1.define your input (in this case “list_norm”) 2. output should be of
vector type = list and length 4 because 4 samples 3. output is just an
empty list 4. i is a placeholder 5. notice that if your vector length
(m) exceeds the number of loops in 1:n such that m \> n, you will have
“null” values in your list 6. if your n (number of loops) exceeds your
input value (length of list_norm) than you will get an error of
subscript out of bounds 7. FYI you can’t pipe into a forloop

``` r
output = vector("list", length = 4)
for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
}
output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.46  4.48
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1  3.63
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.385
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.7  1.25

## can we map??

we can map!! (same output) - simpler process - map for this input list
and apply this function - no need to define output list in advance

map(input_name, function )

``` r
map(list_norm, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.46  4.48
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -12.1  3.63
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.385
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.7  1.25

so … what about other functions?

``` r
map(list_norm, summary)
```

    ## $a
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -5.0275  0.8586  5.3466  4.4604  7.5318 13.4145 
    ## 
    ## $b
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -20.106 -13.784 -11.699 -12.061  -9.404  -6.715 
    ## 
    ## $c
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   16.49   16.84   17.03   17.07   17.25   17.94 
    ## 
    ## $d
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   97.74   98.79   99.46   99.70  100.48  102.42

map variants …

variants, summary —\> you could easily change your function will give
you a simplified vector of what you were starting out with - each time
you are getting a dataframe, you could bind rows

you could also define map(list_norm, median) as equal to output to save
it FYI

using dbl and df would help you show \#s (changes how they appear) and
bind rows respectively

``` r
map(list_norm, median)
```

    ## $a
    ## [1] 5.346592
    ## 
    ## $b
    ## [1] -11.69924
    ## 
    ## $c
    ## [1] 17.03379
    ## 
    ## $d
    ## [1] 99.46151

``` r
map_dbl(list_norm, median)
```

    ##          a          b          c          d 
    ##   5.346592 -11.699240  17.033789  99.461510

``` r
map_df(list_norm, median)
```

    ## # A tibble: 1 × 4
    ##       a     b     c     d
    ##   <dbl> <dbl> <dbl> <dbl>
    ## 1  5.35 -11.7  17.0  99.5

``` r
output = map_df(list_norm, mean_and_sd)
```

## list columns …

-   only issue with dataframes is that you have to use the same \# of
    elements
-   your list_norm technically has 4 elements!
-   inception time!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    norm = list_norm
  )
listcol_df[["norm"]]
```

    ## $a
    ##  [1]  3.3508584  7.6703021  2.2578156  6.3605255  8.6522362  0.8996209
    ##  [7]  6.9329497 13.4144755  7.4856078  1.6799738  5.6257281 -0.2998567
    ## [13]  5.0674557 -5.0274872  7.7076042 -0.3242990  0.7355441 10.8632547
    ## [19]  6.3606260 -0.2052014
    ## 
    ## $b
    ##  [1] -13.147952 -11.846264 -11.759434 -20.105650 -12.871909  -6.715143
    ##  [7] -18.377193 -14.681201 -15.240016  -7.936746  -6.720236 -16.643881
    ## [13] -11.084541 -11.639046  -9.133616  -9.869796 -13.484616  -9.413014
    ## [19] -11.163166  -9.376668
    ## 
    ## $c
    ##  [1] 17.04923 17.11959 17.18704 17.66994 16.93676 16.93413 17.60540 16.56413
    ##  [9] 16.48952 16.73105 16.65084 16.97676 16.59077 17.93782 16.87439 17.36857
    ## [17] 17.01835 17.10055 17.37468 17.21565
    ## 
    ## $d
    ##  [1]  98.56136  98.97018  98.48478  99.55633 101.67175  98.61179 102.42193
    ##  [8]  99.81023  98.85579  99.98498  98.06364 100.92569  97.74110  99.09770
    ## [15] 101.04204 100.16081 101.06490 100.32662  99.28374  99.36669

``` r
output = map(listcol_df[["norm"]], mean_and_sd)
```

can we add list columns, and then what - fyi you could do map_df as well
instead of map below - map across the normal function that I want to map

``` r
listcol_df %>% 
  mutate(
    m_sd = map(norm, mean_and_sd)
  ) %>% 
  select(-norm)
```

    ## # A tibble: 4 × 2
    ##   name  m_sd            
    ##   <chr> <named list>    
    ## 1 a     <tibble [1 × 2]>
    ## 2 b     <tibble [1 × 2]>
    ## 3 c     <tibble [1 × 2]>
    ## 4 d     <tibble [1 × 2]>

## What about something more realistic…

-   when do you actually want to apply the map function in reality?
-   when does data with this structure actually come into play?

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\jsmiz\AppData\Local/Cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-09-29 10:36:13 (8.418)

    ## file min/max dates: 1869-01-01 / 2022-09-30

    ## using cached file: C:\Users\jsmiz\AppData\Local/Cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-09-29 10:36:23 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\jsmiz\AppData\Local/Cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-09-29 10:36:28 (0.952)

    ## file min/max dates: 1999-09-01 / 2022-09-30

you could think about converting each weather station id (n = 3) and
collapse them so that all the data is specific to each dataframe

let’s nest within weather stations… - R asks you to name it as data -
date:tmin are going to be collapsed in the listcol tibble “data”

``` r
weather_nest_df = 
  weather_df %>% 
  nest(data = date:tmin)
```

Really is a list column! - no really go through all

*why not use \$ sign?* - because i don’t want to take out my variable
from my df - that’s how you make mistakes

*why use \[\[\]\]* So you can troubleshoot – happens a lot lol don’t use
dollar signs though Most folks who write code using tidyverse tools –\>
they just don’t use \$ signs

*look into a whileloop: eh nice to know*

``` r
weather_nest_df[["data"]]
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

\##WHat if you wanted to do an analysis on your dataframe of dataframe?
i.e. mini regression fitting model of tmax on tmin for an input data
frame

-   if you give me a df, i can do that

take out the data column (actually a list col) and the first element

``` r
weather_nest_df[["data"]][[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

``` r
lm(tmax ~ tmin, data = weather_nest_df[["data"]][[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest_df[["data"]][[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
lm(tmax ~ tmin, data = weather_nest_df[["data"]][[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest_df[["data"]][[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

``` r
lm(tmax ~ tmin, data = weather_nest_df[["data"]][[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest_df[["data"]][[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

Let’s write a short lil ol function that if you give me a dataframe what
regression comes out of that dataframe

…and you can map across this list of dataframes the weather_lm function

-   given an input list and a function, apply this function to the i to
    n element of the list

``` r
weather_lm = function(df) {
  
  lm(tmax ~ tmin, data = df)
  
}
weather_lm(weather_nest_df[["data"]][[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
### more scalable
map(weather_nest_df[["data"]], weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

Can i do all this in a tidy way - aka not taking it out of the df - want
to be able to know how this is linked together not to extract it

1.  nested
2.  created a function
3.  created a listcol

..This is a lot of work

``` r
weather_nest_df %>% 
  mutate(
    model = map(data, weather_lm)
  )
```

    ## # A tibble: 3 × 4
    ##   name           id          data               model 
    ##   <chr>          <chr>       <list>             <list>
    ## 1 CentralPark_NY USW00094728 <tibble [365 × 4]> <lm>  
    ## 2 Waikiki_HA     USC00519397 <tibble [365 × 4]> <lm>  
    ## 3 Waterhole_WA   USS0023B17S <tibble [365 × 4]> <lm>

YUP

\##unnesting get back what you had before ; reverse of nest

``` r
weather_nest_df %>% 
  unnest(data)
```

    ## # A tibble: 1,095 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

## Napoleon!!

Here’s my scraping function that works for a single page

``` r
read_page_reviews = function(url) {
  
  dynamite_html = read_html(url)
  review_titles = 
    dynamite_html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    dynamite_html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    dynamite_html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim() %>% 
    str_subset("The media could not be loaded.", negate = TRUE) %>% 
    str_subset("^$", negate = TRUE)
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
    )
  
  reviews
  
}
```

What we did last time:

``` r
base_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
vec_url = str_c(base_url, c(1, 2, 3, 4, 5))
dynamite_reviews = 
  bind_rows(
    read_page_reviews(vec_url[1]),
    read_page_reviews(vec_url[2]),
    read_page_reviews(vec_url[3]),
    read_page_reviews(vec_url[4]),
    read_page_reviews(vec_url[5])
  )


###more tidy and eficient

map(vec_url, read_page_reviews)
```

    ## [[1]]
    ## # A tibble: 10 × 3
    ##    title                                      stars text                        
    ##    <chr>                                      <dbl> <chr>                       
    ##  1 Goofy movie                                    5 "I used this movie for a mi…
    ##  2 Lol hey it’s Napoleon. What’s not to love…     5 "Vote for Pedro"            
    ##  3 Still the best                                 5 "Completely stupid, absolut…
    ##  4 70’s and 80’s Schtick Comedy                   5 "…especially funny if you h…
    ##  5 Amazon Censorship                              5 "I hope Amazon does not cen…
    ##  6 Watch to say you did                           3 "I know it's supposed to be…
    ##  7 Best Movie Ever!                               5 "We just love this movie an…
    ##  8 Quirky                                         5 "Good family film"          
    ##  9 Funny movie - can't play it !                  1 "Sony 4k player won't even …
    ## 10 A brilliant story about teenage life           5 "Napoleon Dynamite delivers…
    ## 
    ## [[2]]
    ## # A tibble: 10 × 3
    ##    title                                           stars text                   
    ##    <chr>                                           <dbl> <chr>                  
    ##  1 "HUHYAH"                                            5 Spicy                  
    ##  2 "Cult Classic"                                      4 Takes a time or two to…
    ##  3 "Sweet"                                             5 Timeless Movie. My Gra…
    ##  4 "Cute"                                              4 Fun                    
    ##  5 "great collectible"                                 5 one of the greatest mo…
    ##  6 "Iconic, hilarious flick ! About friend ship ."     5 Who doesn’t love this …
    ##  7 "Funny"                                             5 Me and my dad watched …
    ##  8 "Low budget but okay"                               3 This has been a classi…
    ##  9 "Disappointing"                                     2 We tried to like this,…
    ## 10 "Favorite movie \U0001f37f"                         5 This is one of my favo…
    ## 
    ## [[3]]
    ## # A tibble: 10 × 3
    ##    title                                                             stars text 
    ##    <chr>                                                             <dbl> <chr>
    ##  1 none                                                                  5 "thi…
    ##  2 Great movie                                                           5 "Vot…
    ##  3 Get this to improve your nunchuck and bowstaff skills. Dancing i…     5 "Got…
    ##  4 Incredible Movie                                                      5 "Fun…
    ##  5 Always loved this movie!                                              5 "I h…
    ##  6 Great movie                                                           5 "Bou…
    ##  7 The case was damaged                                                  3 "It …
    ##  8 It’s classic                                                          5 "Cle…
    ##  9 Irreverent comedy                                                     5 "If …
    ## 10 Great classic!                                                        5 "Fun…
    ## 
    ## [[4]]
    ## # A tibble: 10 × 3
    ##    title                                                             stars text 
    ##    <chr>                                                             <dbl> <chr>
    ##  1 Most Awesomsomest Movie EVER!!!                                       5 "Thi…
    ##  2 Always a favorite                                                     5 "I r…
    ##  3 It’s not working the disc keeps showing error when I tried other…     1 "It’…
    ##  4 Gosh!                                                                 5 "Eve…
    ##  5 An Acquired Taste                                                     1 "Thi…
    ##  6 What is this ?                                                        4 "Nic…
    ##  7 Napoleon Dynamite                                                     2 "I w…
    ##  8 Great movie                                                           5 "Gre…
    ##  9 Good movie                                                            5 "Goo…
    ## 10 Came as Described                                                     5 "Cam…
    ## 
    ## [[5]]
    ## # A tibble: 10 × 3
    ##    title                                                           stars text   
    ##    <chr>                                                           <dbl> <chr>  
    ##  1 Oddly on my list of keepers.                                        5 "Good …
    ##  2 Low budget fun                                                      5 "Oddba…
    ##  3 On a scale of 1 to 10 this rates a minus                            1 "This …
    ##  4 I always wondered...                                                5 "what …
    ##  5 Audio/video not synced                                              1 "I tho…
    ##  6 Kind of feels like only a bully would actually laugh at this...     1 "...as…
    ##  7 movie                                                               5 "good …
    ##  8 An Overdose of Comical Cringe                                       5 "Excel…
    ##  9 Glad I never wasted money on this                                   2 "I rem…
    ## 10 A little disappointed                                               3 "The c…

``` r
####now, to make it more efficient and in a df
###### remember: read_page_reviews = a function you defined and basically generating a dataset --> write a function that takes an input and then maps across a list of inputs and creates an output 
####### notice that for the final product page_url is functionally same as vec_url

napoleon_reviews = 
  tibble(
    page = 1:5, ##this was entered after page_url was tested
    page_url = str_c(base_url, page)
  ) %>% 
  mutate(
    reviews = map(page_url, read_page_reviews)
  )


###OH: when did he nest it? Technically, everything was already unique so no need to nest? You have a column that is all tibbles ---> expand all the inception-dataframes

napoleon_reviews %>% 
  select(-page_url) %>% 
  unnest(reviews)
```

    ## # A tibble: 50 × 4
    ##     page title                                      stars text                  
    ##    <int> <chr>                                      <dbl> <chr>                 
    ##  1     1 Goofy movie                                    5 "I used this movie fo…
    ##  2     1 Lol hey it’s Napoleon. What’s not to love…     5 "Vote for Pedro"      
    ##  3     1 Still the best                                 5 "Completely stupid, a…
    ##  4     1 70’s and 80’s Schtick Comedy                   5 "…especially funny if…
    ##  5     1 Amazon Censorship                              5 "I hope Amazon does n…
    ##  6     1 Watch to say you did                           3 "I know it's supposed…
    ##  7     1 Best Movie Ever!                               5 "We just love this mo…
    ##  8     1 Quirky                                         5 "Good family film"    
    ##  9     1 Funny movie - can't play it !                  1 "Sony 4k player won't…
    ## 10     1 A brilliant story about teenage life           5 "Napoleon Dynamite de…
    ## # … with 40 more rows
