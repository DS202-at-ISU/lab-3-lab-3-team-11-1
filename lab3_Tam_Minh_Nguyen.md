lab_3_assignment
================
Tam Minh Nguyen
2024-11-11

<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.4.2

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
colnames(av)
```

    ##  [1] "URL"                         "Name.Alias"                 
    ##  [3] "Appearances"                 "Current."                   
    ##  [5] "Gender"                      "Probationary.Introl"        
    ##  [7] "Full.Reserve.Avengers.Intro" "Year"                       
    ##  [9] "Years.since.joining"         "Honorary"                   
    ## [11] "Death1"                      "Return1"                    
    ## [13] "Death2"                      "Return2"                    
    ## [15] "Death3"                      "Return3"                    
    ## [17] "Death4"                      "Return4"                    
    ## [19] "Death5"                      "Return5"                    
    ## [21] "Notes"

``` r
deaths <- av %>%
  select(Name.Alias, starts_with("Death")) %>%
  pivot_longer(
    cols = starts_with("Death"),
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(Time = parse_number(Time)) %>%
  mutate(Death = na_if(Death, ""))
deaths
```

    ## # A tibble: 865 × 3
    ##    Name.Alias                     Time Death
    ##    <chr>                         <dbl> <chr>
    ##  1 "Henry Jonathan \"Hank\" Pym"     1 YES  
    ##  2 "Henry Jonathan \"Hank\" Pym"     2 <NA> 
    ##  3 "Henry Jonathan \"Hank\" Pym"     3 <NA> 
    ##  4 "Henry Jonathan \"Hank\" Pym"     4 <NA> 
    ##  5 "Henry Jonathan \"Hank\" Pym"     5 <NA> 
    ##  6 "Janet van Dyne"                  1 YES  
    ##  7 "Janet van Dyne"                  2 <NA> 
    ##  8 "Janet van Dyne"                  3 <NA> 
    ##  9 "Janet van Dyne"                  4 <NA> 
    ## 10 "Janet van Dyne"                  5 <NA> 
    ## # ℹ 855 more rows

``` r
returns <- av %>%
  select(Name.Alias, starts_with("Return")) %>%
  pivot_longer(
    cols = starts_with("Return"),
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(Time = parse_number(Time)) %>%
  mutate(Return = na_if(Return, ""))
returns
```

    ## # A tibble: 865 × 3
    ##    Name.Alias                     Time Return
    ##    <chr>                         <dbl> <chr> 
    ##  1 "Henry Jonathan \"Hank\" Pym"     1 NO    
    ##  2 "Henry Jonathan \"Hank\" Pym"     2 <NA>  
    ##  3 "Henry Jonathan \"Hank\" Pym"     3 <NA>  
    ##  4 "Henry Jonathan \"Hank\" Pym"     4 <NA>  
    ##  5 "Henry Jonathan \"Hank\" Pym"     5 <NA>  
    ##  6 "Janet van Dyne"                  1 YES   
    ##  7 "Janet van Dyne"                  2 <NA>  
    ##  8 "Janet van Dyne"                  3 <NA>  
    ##  9 "Janet van Dyne"                  4 <NA>  
    ## 10 "Janet van Dyne"                  5 <NA>  
    ## # ℹ 855 more rows

``` r
death_summary <- deaths %>%
  group_by(Name.Alias) %>%
  summarise(Total_Deaths = sum(tolower(Death) == "yes", na.rm = TRUE))
death_summary
```

    ## # A tibble: 163 × 2
    ##    Name.Alias              Total_Deaths
    ##    <chr>                          <int>
    ##  1 ""                                 7
    ##  2 "\"Giulietta Nefaria\""            1
    ##  3 "Adam"                             1
    ##  4 "Adam Brashear"                    0
    ##  5 "Alani Ryan"                       0
    ##  6 "Alex Summers"                     0
    ##  7 "Alexis"                           0
    ##  8 "Alias: Jonas"                     1
    ##  9 "Amadeus Cho"                      0
    ## 10 "America Chavez"                   0
    ## # ℹ 153 more rows

``` r
return_summary<- returns %>%
  group_by(Name.Alias) %>%
  summarise(Total_Returns = sum(tolower(Return) == "yes", na.rm = TRUE))

return_summary
```

    ## # A tibble: 163 × 2
    ##    Name.Alias              Total_Returns
    ##    <chr>                           <int>
    ##  1 ""                                  2
    ##  2 "\"Giulietta Nefaria\""             0
    ##  3 "Adam"                              0
    ##  4 "Adam Brashear"                     0
    ##  5 "Alani Ryan"                        0
    ##  6 "Alex Summers"                      0
    ##  7 "Alexis"                            0
    ##  8 "Alias: Jonas"                      0
    ##  9 "Amadeus Cho"                       0
    ## 10 "America Chavez"                    0
    ## # ℹ 153 more rows

``` r
average_deaths <- mean(death_summary$Total_Deaths)
average_deaths
```

    ## [1] 0.5460123

``` r
### Fast check###
hulk_deaths <- deaths %>%
  filter(Name.Alias == "Hulk") %>%
  summarise(Hulk_Total_Deaths = sum(tolower(Death) == "yes", na.rm = TRUE))
hulk_deaths
```

    ## # A tibble: 1 × 1
    ##   Hulk_Total_Deaths
    ##               <int>
    ## 1                 0

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.
>
> The Quote I fast-check is “What’s more, if you’re a fan of the MCU,
> nobody is safe. Of the nine Avengers we see on screen — Iron Man,
> Hulk, Captain America, Thor, Hawkeye, Black Widow, Scarlet Witch,
> Quicksilver and The Vision — every single one of them has died at
> least once in the course of their time Avenging in the comics.”
> However, in the dataset, Hulk die 0 times

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
