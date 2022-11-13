iteration
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

\##Z scores

Let’s compute the z - score version of a list of numbers

z score = point estimate - mean of the sample (mu) all over the standard
deviation

rnorm is the R function that simulates random variates having a
specified normal distribution

``` r
x_vec = rnorm(25, mean = 7, sd = 4)
  

x_vec - mean(x_vec) / sd(x_vec)
```

    ##  [1]  5.1234278  4.7851123 -9.6311905  3.8049634  6.7017041 -0.4392398
    ##  [7]  4.7336723  7.3546468  7.9831664  5.9249802  3.8134787  4.1219024
    ## [13] -6.6712758  5.9763688  1.3047758 10.4969318  8.9418308  2.1045082
    ## [19]  2.9162919  3.2728074 11.3713193  0.4957691  3.7406366  5.5307985
    ## [25]  5.3715920

\*Suppose you want to generate multiple z scores; How to create a
function of what I want it to do

-   inside of the function it is going to look for x and recognize that
    there is something to do based on that

-   syntax

(name of custom function) = function(ARGUMENTS) {

BODY OF FUNCTION

}

Here you are using a generic x:

-   Instead of putting “z” to have it show, you can have it “return(z)”
-   you can save the function itself by labeling it z and that way you
    can have the return object show as part of your overall function

``` r
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)

  z
}
```

you will think it hasnt done anything but now you have created a
function called z_scores()

``` r
z_scores(x = x_vec)

z_scores(x = 1:10)
z_scores(x = 3)
z_scores(x = "my name is jeff")
```

what if i want to make it fancier

-   give me an error if I just have a point estimate (not a sample)
-   give me an error if I do not have \#s

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)) {
    stop("Z scores only work for numbers")
  }
  
  if(length(x) < 3) {
    stop("Z scores really only work if you have more than 3 numbers")
  }
}
```

## Let’s have multiple outputs

Let’s just get the mean and sd from the vector input

-   to make it return both of these objects you have to combine both of
    these by creating a tibble

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("Z scores only work for numbers")
  }
  
  if(length(x) < 3) {
    stop("Z scores really only work if you have more than 3 numbers")
  }
  
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
}
```

Now test

``` r
mean_and_sd(x = x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.06  4.63

``` r
mean_and_sd(x = 1:10)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1   5.5  3.03

``` r
mean_and_sd(x = rbinom(1000, 1, .5))
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.549 0.498

## How to write simulations/ functions that have multiple inputs & outputs

-   specifically writing simulation
-   making up data when you have data that is made up
-   what if ran this over and over and over –\> what is the sample mean
    and sample sdev

Notice how you can vary the values manually/rerun for the same inputs
and each yields diff results because diff sample is pulled

\*\* important you inserted the mean value and stdard value yourself

function(ARGUMENTS)

\*\* If I were to rerun below several times, I would get different means
and sd as well \*\* to make them closer, you would increase the sample
size (and to vary could vary e.g. the mean) \*\* below 3 inputs you care
about

``` r
x_vec = rnorm(n = 2500, mean = 7, sd = 4)

tibble(
  mean = mean(x_vec),
  sd = sd(x_vec)
)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.90  4.04

Can I do this using a function? DUH.

-   below: writing a function that allows you to manipulate any of the
    inputs e.g. 

function_name = function(ARGUMENTS) {

CODE TO DO STUFF }

-   BELOW x is a vector being created within the function

-   you are creating default values when your x n_obs is running

also:

``` r
sim_mean_sd = function(n_obs, true_mean = 7, true_sd = 4) {

x = rnorm(n_obs, mean = true_mean, sd = true_sd)

tibble(
  mean = mean(x),
  sd = sd(x)
)

}
```

but does it work?

``` r
sim_mean_sd(n_obs = 25, true_mean = 100, true_sd = 5)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.5  5.90

Do I have to name all the things i have?

If you do not name, it will assume that you are putting thigns in order
i.e sim_mean_sd(25) assumes n_obs = 25 b/c first argument in your
argument

-   if you name assumes same order i.e. true mean = 10, n_obs = 25000,
    sd = 7,

*Same thing*

``` r
sim_mean_sd(25000, 10, 7)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.97  6.99

``` r
sim_mean_sd(true_mean=10, n_obs = 25000, 7)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0  6.99

\##Fixing bad stuff:

\*\* 1 page of reviews \*\* Okay, complete, but how would you scale this
up to get 10 pages of reviews or 1000?! \*\* without functions, you
would simply change 10x - 1000x from &pageNumber = 1 to n

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=5"

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
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

## Let’s write a function to get reviews

still form function(ARGUMENT) { DO STUFF}

-   negate = true (take it away)
-   negate it there is a space or ^\$

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

reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text)

reviews 

}
```

## Let’s try with a URL

-   now you can vary by a URL link each time and have it read for you

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=3"

read_page_reviews(url)
```

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

## What good does this do me?

1.  Start with base url that is everything but the page number

*office hours: why do we need to skip a certain page again?*

-   for me it doesn’t work on pg 4
-   because someone put in an image and the css tag does not work
    because it is not loadable

``` r
base_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
```

``` r
vec_url = str_c(base_url, c(1,2,3,4, 5))

dynamite_reviews = 
  bind_rows(
    read_page_reviews(vec_url[1]),
    read_page_reviews(vec_url[2]),
    read_page_reviews(vec_url[3]),
    read_page_reviews(vec_url[4]),
    read_page_reviews(vec_url[5])
  )
```

## Creating a function using LOTR data

-   Can you write a function that allows you to vary the range, and
    movie name

``` r
fellowship_ring = readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")

lotr_tidy = bind_rows(fellowship_ring, two_towers, return_king) %>%
  janitor::clean_names() %>%
  gather(key = sex, value = words, female:male) %>%
  mutate(race = str_to_lower(race)) %>% 
  select(movie, everything()) 
```

TO

``` r
lotr_load_and_tidy = function(path, range, movie_name) {
  
  df = readxl::read_excel(path, range = range) %>%
    janitor::clean_names() %>%
    gather(key = sex, value = words, female:male) %>%
    mutate(race = str_to_lower(race),
           movie = movie_name)
  
  df
  
}

lotr_tidy = 
  bind_rows(
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "B3:D6", "fellowship_ring"),
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "F3:H6", "two_towers"),
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "J3:L6", "return_king")) %>%
  select(movie, everything()) 
```

## Variable Scoping and names

-   above, you were working on functions that were created INSIDE a
    function

*Does this function exist outside? NO IT STAYS INSIDE THE FUNCTION*

Impact: \* debugging and writing function quirks \* have you created the
rigth thing \* *you may have run it in the environment just to check it
and oops you accidentally created a function outside of the function
technically because you highlighted it* \* did you make sure you are
reusing/recycling the same variable outside of your function \* *careful
becuase you also may have already created a variable outside of your
fucntion and are now calling it in the function – you are not creating
the variable but you are telling R to search for that variable in your R
studio instance = SCOPING*

*Why is it better to create an input*

*You can return what you want in the tibble, but don’t create a new
variable*

``` r
sim_mean_sd = function(n_obs, true_mean = 7, true_sd = 4) {

x_inside__of_function = rnorm(n_obs, true_mean, true_sd)

tibble(
  mean = mean(x),
  sd = sd(x)
)

}
```
