Making sense of the corpus of scientific papers on capture-recapture
---

Motivation
----------

My objective is to make a list of ecological questions and methods that
are addressed in these papers. I ended up with more than 5000 papers.
The bibliometric and text analyses were somehow useful, but I need to
dig a bit deeper to achieve the objective. Here how I will proceed.

Methodological papers
---------------------

First, I isolate the methodological journals. To do so, I focus the
search on journals that have published more than 10 papers about
capture-recapture over the last 10 years:

``` r
library(tidyverse)
raw_dat <- read_csv(file = 'crdat.csv')
```

    ## Parsed with column specification:
    ## cols(
    ##   title = col_character(),
    ##   keywords = col_character(),
    ##   journal = col_character(),
    ##   authors = col_character(),
    ##   abstract = col_character()
    ## )

``` r
raw_dat %>% 
  group_by(journal) %>%
  filter(n() > 10) %>%
  ungroup() %>%
  count(journal)
```

    ## # A tibble: 107 x 2
    ##    journal                                                   n
    ##    <chr>                                                 <int>
    ##  1 AFRICAN JOURNAL OF ECOLOGY                               15
    ##  2 AFRICAN JOURNAL OF MARINE SCIENCE                        21
    ##  3 AMERICAN MIDLAND NATURALIST                              11
    ##  4 AMERICAN NATURALIST                                      14
    ##  5 AMPHIBIA-REPTILIA                                        20
    ##  6 ANIMAL BEHAVIOUR                                         11
    ##  7 ANIMAL CONSERVATION                                      25
    ##  8 ANNALS OF APPLIED STATISTICS                             18
    ##  9 AQUATIC CONSERVATION-MARINE AND FRESHWATER ECOSYSTEMS    17
    ## 10 ARDEA                                                    12
    ## # … with 97 more rows

By inspecting the list, I end up with these journals:

``` r
methods <- raw_dat %>% 
  filter(journal %in% c('BIOMETRICS',
                        'ECOLOGICAL MODELLING',
                        'JOURNAL OF AGRICULTURAL BIOLOGICAL AND ENVIRONMENTAL STATISTICS',
                        'METHODS IN ECOLOGY AND EVOLUTION',
                        'ANNALS OF APPLIED STATISTICS',
                        'ENVIRONMENTAL AND ECOLOGICAL STATISTICS'))
```

The number of papers published in each of them is:

``` r
methods %>%
  count(journal)
```

    ## # A tibble: 6 x 2
    ##   journal                                                             n
    ##   <chr>                                                           <int>
    ## 1 ANNALS OF APPLIED STATISTICS                                       18
    ## 2 BIOMETRICS                                                         45
    ## 3 ECOLOGICAL MODELLING                                               26
    ## 4 ENVIRONMENTAL AND ECOLOGICAL STATISTICS                            20
    ## 5 JOURNAL OF AGRICULTURAL BIOLOGICAL AND ENVIRONMENTAL STATISTICS    33
    ## 6 METHODS IN ECOLOGY AND EVOLUTION                                   77

Now I export the 219 papers published in these methodological journals
in a csv file:

``` r
raw_dat %>% 
  filter(journal %in% c('BIOMETRICS',
                        'ECOLOGICAL MODELLING',
                        'JOURNAL OF AGRICULTURAL BIOLOGICAL AND ENVIRONMENTAL STATISTICS',
                        'METHODS IN ECOLOGY AND EVOLUTION',
                        'ANNALS OF APPLIED STATISTICS',
                        'ENVIRONMENTAL AND ECOLOGICAL STATISTICS')) %>%
  write_csv('papers_in_methodological_journals.csv')
```

The next step is to annotate this file to determine the methods used. I’m afraid `R` cannot help, and I had to do it by hand. I read the >200 titles and abstracts and added my tags in an extra column. Took me 2 hours or so. The task was cumbersome but very interesting. I enjoyed seeing what my colleagues have been working on. There were a few papers that I didn't know the existence of, and several others that was pleased to spot again. The results are in [this file](https://github.com/oliviergimenez/capture-recapture-review/blob/master/papers_in_methodological_journals_annotated.csv).

By focusing the annotation on the methodological journals, I ignore all the methodological papers that have been published in other non-methodological journals like, among others, Ecology, Journal of Applied Ecology, Conservation Biology and Plos One which welcome methods. I address this issue below. In brief, I scanned the corpus of ecological papers and tagged all methodological papers; I also moved them to the [file of methodological papers](https://github.com/oliviergimenez/capture-recapture-review/blob/master/papers_in_methodological_journals_annotated.csv) and added a column to keep track of the paper original (methodological vs ecological corpus).

Ecological papers
-----------------

Second, I isolate the ecological journals. To do so, I focus the search
on journals that have published more than 50 papers about
capture-recapture over the last 10 years, and I exclude the
methodological journals

``` r
ecol <- raw_dat %>% 
  filter(!journal %in% c('BIOMETRICS',
                        'ECOLOGICAL MODELLING',
                        'JOURNAL OF AGRICULTURAL BIOLOGICAL AND ENVIRONMENTAL STATISTICS',
                        'METHODS IN ECOLOGY AND EVOLUTION',
                        'ANNALS OF APPLIED STATISTICS',
                        'ENVIRONMENTAL AND ECOLOGICAL STATISTICS')) %>%
  group_by(journal) %>%
  filter(n() > 50) %>%
  ungroup()

ecol %>% 
  count(journal, sort = TRUE)
```

    ## # A tibble: 16 x 2
    ##    journal                                                n
    ##    <chr>                                              <int>
    ##  1 PLOS ONE                                             219
    ##  2 JOURNAL OF WILDLIFE MANAGEMENT                       170
    ##  3 ECOLOGY AND EVOLUTION                                116
    ##  4 ECOLOGY                                              101
    ##  5 BIOLOGICAL CONSERVATION                               99
    ##  6 JOURNAL OF ANIMAL ECOLOGY                             80
    ##  7 JOURNAL OF MAMMALOGY                                  73
    ##  8 JOURNAL OF APPLIED ECOLOGY                            72
    ##  9 NORTH AMERICAN JOURNAL OF FISHERIES MANAGEMENT        65
    ## 10 POPULATION ECOLOGY                                    60
    ## 11 CANADIAN JOURNAL OF FISHERIES AND AQUATIC SCIENCES    55
    ## 12 WILDLIFE RESEARCH                                     55
    ## 13 FISHERIES RESEARCH                                    54
    ## 14 OECOLOGIA                                             54
    ## 15 TRANSACTIONS OF THE AMERICAN FISHERIES SOCIETY        54
    ## 16 JOURNAL OF INSECT CONSERVATION                        51

``` r
ecol %>%
  nrow()
```

    ## [1] 1378

``` r
ecol %>%
  write_csv('papers_in_ecological_journals.csv')
```

Again, I inspected the papers one by one. Took me several hours as there were >1000 papers! I mainly focused my reading on the titles. I didn't annotate the papers.
