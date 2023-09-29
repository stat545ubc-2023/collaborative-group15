Team Troubleshooting Deliverable 2
================

There are **11 code chunks with errors** in this Rmd. Your objective is
to fix all of the errors in this worksheet. For the purpose of grading,
each erroneous code chunk is equally weighted.

Note that errors are not all syntactic (i.e., broken code)! Some are
logical errors as well (i.e. code that does not do what it was intended
to do).

## Exercise 1: Exploring with `select()` and `filter()`

[MovieLens](https://dl.acm.org/doi/10.1145/2827872) are a series of
datasets widely used in education, that describe movie ratings from the
MovieLens [website](https://movielens.org/). There are several MovieLens
datasets, collected by the [GroupLens Research
Project](https://grouplens.org/datasets/movielens/) at the University of
Minnesota. Here, we load the MovieLens 100K dataset from Rafael Irizarry
and Amy Gill’s R package,
[dslabs](https://cran.r-project.org/web/packages/dslabs/dslabs.pdf),
which contains datasets useful for data analysis practice, homework, and
projects in data science courses and workshops. We’ll also load other
required packages.

``` r
# old code:
# load.packages(dslabs)
# load.packages(tidyverse)
# load.packages(stringr)
# install.packages("devtools") # Do not run this if you already have this package installed! 
# devtools::install_github("JoeyBernhardt/singer")
# load.packages(gapminder)
# install.packages("dslabs") 

# load.packages is not the correct syntax to call libraries. We changed the load.packages() to library():

#fixed code: 
library("dslabs")
library("tidyverse")
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.7     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library("stringr")
library("devtools") 
```

    ## Loading required package: usethis

``` r
devtools::install_github("JoeyBernhardt/singer")
```

    ## Skipping install of 'singer' from a github remote, the SHA1 (2b4fe9cb) has not changed since last install.
    ##   Use `force = TRUE` to force installation

``` r
library("gapminder")
```

    ## 
    ## Attaching package: 'gapminder'

    ## The following object is masked from 'package:dslabs':
    ## 
    ##     gapminder

Let’s have a look at the dataset! My goal is to:

- Find out the “class” of the dataset.
- If it isn’t a tibble already, coerce it into a tibble and store it in
  the variable “movieLens”.
- Have a quick look at the tibble, using a *dplyr function*.

``` r
#old code:
# class(dslabs::movielens)
# movieLens <- as_tibble(dslabs::movielens)
# dim(movieLens)

#fixed code:
class(movielens)
```

    ## [1] "data.frame"

``` r
movieLens <- as_tibble(movielens)
is_tibble(movieLens) # checking here with a dplyr function that this is a tibble
```

    ## [1] TRUE

``` r
dplyr::glimpse(movieLens) # having a quick look at our data using a dplyr function
```

    ## Rows: 100,004
    ## Columns: 7
    ## $ movieId   <int> 31, 1029, 1061, 1129, 1172, 1263, 1287, 1293, 1339, 1343, 13…
    ## $ title     <chr> "Dangerous Minds", "Dumbo", "Sleepers", "Escape from New Yor…
    ## $ year      <int> 1995, 1941, 1996, 1981, 1989, 1978, 1959, 1982, 1992, 1991, …
    ## $ genres    <fct> Drama, Animation|Children|Drama|Musical, Thriller, Action|Ad…
    ## $ userId    <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ rating    <dbl> 2.5, 3.0, 3.0, 2.0, 4.0, 2.0, 2.0, 2.0, 3.5, 2.0, 2.5, 1.0, …
    ## $ timestamp <int> 1260759144, 1260759179, 1260759182, 1260759185, 1260759205, …

``` r
# Explanation of solution:
# We do not need to specify the library name in the code before accessing the dataframe, because movieLens is an object rather than a function within the dslabs package. We removed "dslabs::" from the code. We are looking at data with dplyr's glimpse rather that 'dim' because this gives a better overview and I don't think dim is from dplyr.
```

Now that we’ve had a quick look at the dataset, it would be interesting
to explore the rows (observations) in some more detail. I’d like to
consider the movie entries that…

- belong *exclusively* to the genre *“Drama”*; \#\[1\]
- don’t belong *exclusively* to the genre *“Drama”*; \#\[2\]
- were filmed *after* the year 2000; \#\[3\]
- were filmed in 1999 *or* 2000; \#\[4\]
- have *more than* 4.5 stars, and were filmed *before* 1995. \#\[5\]

``` r
filter(movieLens, genres == "Drama")  #[1] - Original syntax retained 
```

    ## # A tibble: 7,757 × 7
    ##    movieId title                             year genres userId rating timestamp
    ##      <int> <chr>                            <int> <fct>   <int>  <dbl>     <int>
    ##  1      31 Dangerous Minds                   1995 Drama       1    2.5    1.26e9
    ##  2    1172 Cinema Paradiso (Nuovo cinema P…  1989 Drama       1    4      1.26e9
    ##  3    1293 Gandhi                            1982 Drama       1    2      1.26e9
    ##  4      62 Mr. Holland's Opus                1995 Drama       2    3      8.35e8
    ##  5     261 Little Women                      1994 Drama       2    4      8.35e8
    ##  6     300 Quiz Show                         1994 Drama       2    3      8.35e8
    ##  7     508 Philadelphia                      1993 Drama       2    4      8.35e8
    ##  8     537 Sirens                            1994 Drama       2    4      8.35e8
    ##  9    2702 Summer of Sam                     1999 Drama       3    3.5    1.30e9
    ## 10    3949 Requiem for a Dream               2000 Drama       3    5      1.30e9
    ## # ℹ 7,747 more rows

``` r
filter(movieLens, !genres == "Drama") # [2] - Original syntax retained 
```

    ## # A tibble: 92,247 × 7
    ##    movieId title                            year genres  userId rating timestamp
    ##      <int> <chr>                           <int> <fct>    <int>  <dbl>     <int>
    ##  1    1029 Dumbo                            1941 Animat…      1    3      1.26e9
    ##  2    1061 Sleepers                         1996 Thrill…      1    3      1.26e9
    ##  3    1129 Escape from New York             1981 Action…      1    2      1.26e9
    ##  4    1263 Deer Hunter, The                 1978 Drama|…      1    2      1.26e9
    ##  5    1287 Ben-Hur                          1959 Action…      1    2      1.26e9
    ##  6    1339 Dracula (Bram Stoker's Dracula)  1992 Fantas…      1    3.5    1.26e9
    ##  7    1343 Cape Fear                        1991 Thrill…      1    2      1.26e9
    ##  8    1371 Star Trek: The Motion Picture    1979 Advent…      1    2.5    1.26e9
    ##  9    1405 Beavis and Butt-Head Do America  1996 Advent…      1    1      1.26e9
    ## 10    1953 French Connection, The           1971 Action…      1    4      1.26e9
    ## # ℹ 92,237 more rows

``` r
# filter(movieLens, year >= 2000) [3]: old syntax 
filter(movieLens, year > 2000) #[3] - removed equal sign in the filter so now we are excluding movies filmed *in* 2000
```

    ## # A tibble: 25,481 × 7
    ##    movieId title                             year genres userId rating timestamp
    ##      <int> <chr>                            <int> <fct>   <int>  <dbl>     <int>
    ##  1    5349 Spider-Man                        2002 Actio…      3    3      1.30e9
    ##  2    5669 Bowling for Columbine             2002 Docum…      3    3.5    1.30e9
    ##  3    6377 Finding Nemo                      2003 Adven…      3    3      1.30e9
    ##  4    7153 Lord of the Rings: The Return o…  2003 Actio…      3    2.5    1.30e9
    ##  5    7361 Eternal Sunshine of the Spotles…  2004 Drama…      3    3      1.30e9
    ##  6    8622 Fahrenheit 9/11                   2004 Docum…      3    3.5    1.30e9
    ##  7    8636 Spider-Man 2                      2004 Actio…      3    3      1.30e9
    ##  8   44191 V for Vendetta                    2006 Actio…      3    3.5    1.30e9
    ##  9   48783 Flags of Our Fathers              2006 Drama…      3    4.5    1.30e9
    ## 10   50068 Letters from Iwo Jima             2006 Drama…      3    4.5    1.30e9
    ## # ℹ 25,471 more rows

``` r
# filter(movieLens, year == 1999 | month == 2000) [4]: old syntax
filter(movieLens, year == 1999 | year == 2000) # [4] - changed month to year
```

    ## # A tibble: 9,088 × 7
    ##    movieId title                             year genres userId rating timestamp
    ##      <int> <chr>                            <int> <fct>   <int>  <dbl>     <int>
    ##  1    2694 Big Daddy                         1999 Comedy      3    3      1.30e9
    ##  2    2702 Summer of Sam                     1999 Drama       3    3.5    1.30e9
    ##  3    2762 Sixth Sense, The                  1999 Drama…      3    3.5    1.30e9
    ##  4    2841 Stir of Echoes                    1999 Horro…      3    4      1.30e9
    ##  5    2858 American Beauty                   1999 Drama…      3    4      1.30e9
    ##  6    2959 Fight Club                        1999 Actio…      3    5      1.30e9
    ##  7    3510 Frequency                         2000 Drama…      3    4      1.30e9
    ##  8    3949 Requiem for a Dream               2000 Drama       3    5      1.30e9
    ##  9   27369 Daria: Is It Fall Yet?            2000 Anima…      3    3.5    1.30e9
    ## 10    2628 Star Wars: Episode I - The Phan…  1999 Actio…      4    5      9.50e8
    ## # ℹ 9,078 more rows

``` r
filter(movieLens, rating > 4.5, year < 1995) # [5] - Original syntax retained 
```

    ## # A tibble: 8,386 × 7
    ##    movieId title                             year genres userId rating timestamp
    ##      <int> <chr>                            <int> <fct>   <int>  <dbl>     <int>
    ##  1     265 Like Water for Chocolate (Como …  1992 Drama…      2      5    8.35e8
    ##  2     266 Legends of the Fall               1994 Drama…      2      5    8.35e8
    ##  3     551 Nightmare Before Christmas, The   1993 Anima…      2      5    8.35e8
    ##  4     589 Terminator 2: Judgment Day        1991 Actio…      2      5    8.35e8
    ##  5     590 Dances with Wolves                1990 Adven…      2      5    8.35e8
    ##  6     592 Batman                            1989 Actio…      2      5    8.35e8
    ##  7     318 Shawshank Redemption, The         1994 Crime…      3      5    1.30e9
    ##  8     356 Forrest Gump                      1994 Comed…      3      5    1.30e9
    ##  9    1197 Princess Bride, The               1987 Actio…      3      5    1.30e9
    ## 10     260 Star Wars: Episode IV - A New H…  1977 Actio…      4      5    9.50e8
    ## # ℹ 8,376 more rows

While filtering for *all movies that do not belong to the genre drama*
above, I noticed something interesting. I want to filter for the same
thing again, this time selecting variables **title and genres first,**
and then *everything else*. But I want to do this in a robust way, so
that (for example) if I end up changing `movieLens` to contain more or
less columns some time in the future, the code will still work. Hint:
there is a function to select “everything else”…

``` r
movieLens %>%
  filter(!genres == "Drama") %>%
  # Error here: we should use everything function to select other atttributes. Change from inumerate to everything() function.
  # select(title, genres, year, rating, timestamp)
  select(title, genres, everything())
```

    ## # A tibble: 92,247 × 7
    ##    title                           genres  movieId  year userId rating timestamp
    ##    <chr>                           <fct>     <int> <int>  <int>  <dbl>     <int>
    ##  1 Dumbo                           Animat…    1029  1941      1    3      1.26e9
    ##  2 Sleepers                        Thrill…    1061  1996      1    3      1.26e9
    ##  3 Escape from New York            Action…    1129  1981      1    2      1.26e9
    ##  4 Deer Hunter, The                Drama|…    1263  1978      1    2      1.26e9
    ##  5 Ben-Hur                         Action…    1287  1959      1    2      1.26e9
    ##  6 Dracula (Bram Stoker's Dracula) Fantas…    1339  1992      1    3.5    1.26e9
    ##  7 Cape Fear                       Thrill…    1343  1991      1    2      1.26e9
    ##  8 Star Trek: The Motion Picture   Advent…    1371  1979      1    2.5    1.26e9
    ##  9 Beavis and Butt-Head Do America Advent…    1405  1996      1    1      1.26e9
    ## 10 French Connection, The          Action…    1953  1971      1    4      1.26e9
    ## # ℹ 92,237 more rows

## Exercise 2: Calculating with `mutate()`-like functions

Some of the variables in the `movieLens` dataset are in *camelCase* (in
fact, *movieLens* is in camelCase). Let’s clean these two variables to
use *snake_case* instead, and assign our post-rename object back to
“movieLens”.

``` r
movieLens <- movieLens %>%
  # To rename we should use equal sign "=" to assign value not the "==" which compares the 2 variable not assign them. We changed "==" to "=" in the code.
  # rename(user_id == userId,
        #  movie_id == movieId)
  rename(user_id = userId,
  movie_id = movieId)

# Aside/NB the var 'timestamp' could also be converted to snake_case
```

As you already know, `mutate()` defines and inserts new variables into a
tibble. There is *another mystery function similar to `mutate()`* that
adds the new variable, but also drops existing ones. I wanted to create
an `average_rating` column that takes the `mean(rating)` across all
entries, and I only want to see that variable (i.e drop all others!) but
I forgot what that mystery function is. Can you remember?

``` r
# The mutate() function can be replaced by transmute() to achieve the goal of drop all other variables. We replace mutate() to transmute() here.
# mutate(movieLens,
#        average_rating = mean(rating))
  movieLens %>%
  transmute(average_rating = mean(rating))
```

    ## # A tibble: 100,004 × 1
    ##    average_rating
    ##             <dbl>
    ##  1           3.54
    ##  2           3.54
    ##  3           3.54
    ##  4           3.54
    ##  5           3.54
    ##  6           3.54
    ##  7           3.54
    ##  8           3.54
    ##  9           3.54
    ## 10           3.54
    ## # ℹ 99,994 more rows

``` r
# ------ ASIDE: if you wanted to see the avg rating *by movie*, but across users, we could use:
movieLens %>%
group_by(title) %>%
transmute(average_rating = mean(rating))
```

    ## # A tibble: 100,004 × 2
    ## # Groups:   title [8,832]
    ##    title                                   average_rating
    ##    <chr>                                            <dbl>
    ##  1 Dangerous Minds                                   3.18
    ##  2 Dumbo                                             3.70
    ##  3 Sleepers                                          3.55
    ##  4 Escape from New York                              3.31
    ##  5 Cinema Paradiso (Nuovo cinema Paradiso)           4.26
    ##  6 Deer Hunter, The                                  3.86
    ##  7 Ben-Hur                                           3.89
    ##  8 Gandhi                                            3.98
    ##  9 Dracula (Bram Stoker's Dracula)                   3.30
    ## 10 Cape Fear                                         3.71
    ## # ℹ 99,994 more rows

## Exercise 3: Calculating with `summarise()`-like functions

Alone, `tally()` is a short form of `summarise()`. `count()` is
short-hand for `group_by()` and `tally()`.

Each entry of the movieLens table corresponds to a movie rating by a
user. Therefore, if more than one user rated the same movie, there will
be several entries for the same movie. I want to find out how many times
each movie has been reviewed, or in other words, how many times each
movie title appears in the dataset.

``` r
movieLens %>%
  group_by(title) %>%
  tally()
```

    ## # A tibble: 8,832 × 2
    ##    title                                n
    ##    <chr>                            <int>
    ##  1 ¡Three Amigos!                      31
    ##  2 ...And God Spoke                     1
    ##  3 ...And Justice for All              13
    ##  4 'burbs, The                         19
    ##  5 'Hellboy': The Seeds of Creation     1
    ##  6 'Neath the Arizona Skies             1
    ##  7 'night Mother                        3
    ##  8 'Round Midnight                      2
    ##  9 'Salem's Lot                         1
    ## 10 'Til There Was You                   4
    ## # ℹ 8,822 more rows

Without using `group_by()`, I want to find out how many movie reviews
there have been for each year.

``` r
# If not grouping by year we need to use the count() function to count the number of years. Use count() instead of tally() here and rename the result n to a new variable "review_count".
# movieLens %>%
#   tally(year)

movieLens %>%
  count(year) %>%
  rename(review_count = n)
```

    ## # A tibble: 104 × 2
    ##     year review_count
    ##    <int>        <int>
    ##  1  1902            6
    ##  2  1915            2
    ##  3  1916            1
    ##  4  1917            2
    ##  5  1918            2
    ##  6  1919            1
    ##  7  1920           15
    ##  8  1921           12
    ##  9  1922           28
    ## 10  1923            3
    ## # ℹ 94 more rows

Both `count()` and `tally()` can be grouped by multiple columns. Below,
I want to count the number of movie reviews by title and rating, and
sort the results.

``` r
movieLens %>%
  # Instead of using a turple, we could directly list the variable used for count() function. Remove the c() in code.
  # count(c(title, rating), sort = TRUE)
  count(title, rating, sort = TRUE)
```

    ## # A tibble: 28,297 × 3
    ##    title                              rating     n
    ##    <chr>                               <dbl> <int>
    ##  1 Shawshank Redemption, The               5   170
    ##  2 Pulp Fiction                            5   138
    ##  3 Star Wars: Episode IV - A New Hope      5   122
    ##  4 Forrest Gump                            4   113
    ##  5 Schindler's List                        5   109
    ##  6 Godfather, The                          5   107
    ##  7 Forrest Gump                            5   102
    ##  8 Silence of the Lambs, The               4   102
    ##  9 Fargo                                   5   100
    ## 10 Silence of the Lambs, The               5   100
    ## # ℹ 28,287 more rows

``` r
# ------ ASIDE: If you find it more intuitive to see the number of ratings at each level sorted *by movie*, we can append the 'arrange' line to our code:
movieLens %>%
  count(title, rating, sort = TRUE) %>%
  arrange(title)
```

    ## # A tibble: 28,297 × 3
    ##    title                  rating     n
    ##    <chr>                   <dbl> <int>
    ##  1 ¡Three Amigos!            3       9
    ##  2 ¡Three Amigos!            4       8
    ##  3 ¡Three Amigos!            3.5     5
    ##  4 ¡Three Amigos!            2       4
    ##  5 ¡Three Amigos!            4.5     2
    ##  6 ¡Three Amigos!            1       1
    ##  7 ¡Three Amigos!            1.5     1
    ##  8 ¡Three Amigos!            5       1
    ##  9 ...And God Spoke          1       1
    ## 10 ...And Justice for All    4       5
    ## # ℹ 28,287 more rows

Not only do `count()` and `tally()` quickly allow you to count items
within your dataset, `add_tally()` and `add_count()` are handy shortcuts
that add an additional columns to your tibble, rather than collapsing
each group.

## Exercise 4: Calculating with `group_by()`

We can calculate the mean rating by year, and store it in a new column
called `avg_rating`:

``` r
movieLens %>%
  group_by(year) %>%
  summarize(avg_rating = mean(rating))
```

    ## # A tibble: 104 × 2
    ##     year avg_rating
    ##    <int>      <dbl>
    ##  1  1902       4.33
    ##  2  1915       3   
    ##  3  1916       3.5 
    ##  4  1917       4.25
    ##  5  1918       4.25
    ##  6  1919       3   
    ##  7  1920       3.7 
    ##  8  1921       4.42
    ##  9  1922       3.80
    ## 10  1923       4.17
    ## # ℹ 94 more rows

Using `summarize()`, we can find the minimum and the maximum rating by
title, stored under columns named `min_rating`, and `max_rating`,
respectively.

``` r
### ERROR HERE ###
movieLens %>%
  # Instead of using mutate we could use the function summarize to save only min_ and max_rating in the final result. change from mutate() to summarize in the final result. However, when we use call 'summarize' we need to combine with the 'group_by' call to avoid collapsing across movies. (Below we change from mutate() to summarize + group_by)

  # mutate(min_rating = min(rating), 
  #        max_rating = max(rating))
group_by(title) %>%
  summarize(min_rating = min(rating), 
         max_rating = max(rating))
```

    ## # A tibble: 8,832 × 3
    ##    title                            min_rating max_rating
    ##    <chr>                                 <dbl>      <dbl>
    ##  1 ¡Three Amigos!                          1          5  
    ##  2 ...And God Spoke                        1          1  
    ##  3 ...And Justice for All                  1          5  
    ##  4 'burbs, The                             1.5        4.5
    ##  5 'Hellboy': The Seeds of Creation        2          2  
    ##  6 'Neath the Arizona Skies                0.5        0.5
    ##  7 'night Mother                           5          5  
    ##  8 'Round Midnight                         0.5        4  
    ##  9 'Salem's Lot                            3.5        3.5
    ## 10 'Til There Was You                      0.5        4  
    ## # ℹ 8,822 more rows

## Exercise 5: Scoped variants with `across()`

`across()` is a newer dplyr function (`dplyr` 1.0.0) that allows you to
apply a transformation to multiple variables selected with the
`select()` and `rename()` syntax. For this section, we will use the
`starwars` dataset, which is built into R. First, let’s transform it
into a tibble and store it under the variable `starWars`.

``` r
starWars <- as_tibble(starwars)
```

We can find the mean for all columns that are numeric, ignoring the
missing values:

``` r
starWars %>%
  summarise(across(where(is.numeric), function(x) mean(x, na.rm=TRUE)))
```

    ## # A tibble: 1 × 3
    ##   height  mass birth_year
    ##    <dbl> <dbl>      <dbl>
    ## 1   174.  97.3       87.6

We can find the minimum height and mass within each species, ignoring
the missing values:

``` r
starWars %>%
group_by(species) %>%
 #summarise(across("height", "mass", function(x) min(x, na.rm=TRUE)))
summarize(across(c(height, mass), function(x) min(x, na.rm=TRUE))) # we have told R that we are summarizing across both height and mass by bracketing these terms and also indicated that height and mass are columns with the 'c' notation that precedes the brackets. 
```

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## Warning in min(x, na.rm = TRUE): no non-missing arguments to min; returning Inf

    ## # A tibble: 38 × 3
    ##    species   height  mass
    ##    <chr>      <int> <dbl>
    ##  1 Aleena        79    15
    ##  2 Besalisk     198   102
    ##  3 Cerean       198    82
    ##  4 Chagrian     196   Inf
    ##  5 Clawdite     168    55
    ##  6 Droid         96    32
    ##  7 Dug          112    40
    ##  8 Ewok          88    20
    ##  9 Geonosian    183    80
    ## 10 Gungan       196    66
    ## # ℹ 28 more rows

``` r
# Regarding warning about: 'no non-missing arguments to min' ##
# I think just means that for some species there are no values for mass and/or height, therefore the min function can't be applied to this group data - (For instance there is no mass returned in species Chagrian and therefore the minimum mass indicated for Chafrian is 'Inf'). I'm pretty sure this cal still does what we want it to.
# EDIT: prof. has addressed ^ below.
```

Note that here R has taken the convention that the minimum value of a
set of `NA`s is `Inf`.

## Exercise 6: Making tibbles

Manually create a tibble with 4 columns:

- `birth_year` should contain years 1998 to 2005 (inclusive);
- `birth_weight` should take the `birth_year` column, subtract 1995, and
  multiply by 0.45;
- `birth_location` should contain three locations (Liverpool, Seattle,
  and New York).

``` r
# We need to enclose the "birth_location" with quotes, here we add quotes for each birth_location
# fakeStarWars <- tribble(
#   ~name,            ~birth_weight,  ~birth_year, ~birth_location
#   "Luke Skywalker",  1.35      ,   1998        ,  Liverpool, England,
#   "C-3PO"         ,  1.80      ,   1999        ,  Liverpool, England,
#   "R2-D2"         ,  2.25      ,   2000        ,  Seattle, WA,
#   "Darth Vader"   ,  2.70      ,   2001        ,  Liverpool, England,
#   "Leia Organa"   ,  3.15      ,   2002        ,  New York, NY,
#   "Owen Lars"     ,  3.60      ,   2003        ,  Seattle, WA,
#   "Beru Whitesun Iars", 4.05   ,   2004        ,  Liverpool, England,
#   "R5-D4"         ,  4.50      ,   2005        ,  New York, NY,
# )

fakeStarWars <- tribble(
  ~name,            ~birth_weight,  ~birth_year, ~birth_location,
  "Luke Skywalker",  1.35      ,   1998        ,  "Liverpool, England",
  "C-3PO"         ,  1.80      ,   1999        ,  "Liverpool, England",
  "R2-D2"         ,  2.25      ,   2000        ,  "Seattle, WA",
  "Darth Vader"   ,  2.70      ,   2001        ,  "Liverpool, England",
  "Leia Organa"   ,  3.15      ,   2002        ,  "New York, NY",
  "Owen Lars"     ,  3.60      ,   2003        ,  "Seattle, WA",
  "Beru Whitesun Iars", 4.05   ,   2004        ,  "Liverpool, England",
  "R5-D4"         ,  4.50      ,   2005        ,  "New York, NY"
)

head(fakeStarWars)
```

    ## # A tibble: 6 × 4
    ##   name           birth_weight birth_year birth_location    
    ##   <chr>                 <dbl>      <dbl> <chr>             
    ## 1 Luke Skywalker         1.35       1998 Liverpool, England
    ## 2 C-3PO                  1.8        1999 Liverpool, England
    ## 3 R2-D2                  2.25       2000 Seattle, WA       
    ## 4 Darth Vader            2.7        2001 Liverpool, England
    ## 5 Leia Organa            3.15       2002 New York, NY      
    ## 6 Owen Lars              3.6        2003 Seattle, WA

``` r
# Check that the birth weight values are 45% of the difference between the birth year and 1995
ifelse(fakeStarWars$birth_weight!=((fakeStarWars$birth_year-1995)*0.45), "FIXME", "itsFine")
```

    ## [1] "itsFine" "itsFine" "itsFine" "itsFine" "itsFine" "itsFine" "itsFine"
    ## [8] "itsFine"

## Attributions

Thanks to Icíar Fernández-Boyano for writing most of this document, and
Albina Gibadullina, Diana Lin, Yulia Egorova, and Vincenzo Coia for
their edits.
