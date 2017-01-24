Basic Stats
================
jeff macinnes
January 24, 2017

-   [The Data](#the-data)
    -   [Quick data summary](#quick-data-summary)
    -   [Including Plots](#including-plots)

The goal here is to provide an overview on some basic statistical approaches that you can use in analyzing your AVB eye-tracking data. We'll cover the ideas behind these approaches -- identifying dependent/independent variables, deciding on appropriate analyses, interpretting results -- as well as the tools to run these analyses yourself. We'll be using RStudio, so if you haven't already download and install both R and RStudio.

The Data
========

In order to have some data to play with, there is a simulated datafile in `data/exampleData.tsv`

This dataset has 40 subjects. For each subject we have values for IQ, and fixation time on the nose of various image categories. So we have:

-   Dependent Variables (DV):
    -   **IQ**: subject IQ score
    -   **noseDwellTime**: time spent fixating on the nose

And we measured **noseDwellTime** across 3 different image categories. So, one independent variable (IV) with 3 levels

-   Independent Variable levels:
    -   **movie**: faces of movie stars
    -   **athlete**: faces of athletes
    -   **nobel**: faces of Nobel laureates

So, lets's load the dataset:

``` r
# if you haven't already, set your working directory to 'stats_intro'
#setwd("<dirPath>/stats_intro")

# read the data, store in variable called 'dt
dt <- read.csv('data/exampleData.tsv', sep='\t', row.names=1)

# print the first few rows
head(dt, n=5)
```

    ##            IQ noseMovie noseAthlete noseNobel
    ## subject_1 100    199.78      317.33    415.08
    ## subject_2  57    228.94      261.23    368.76
    ## subject_3  96    316.19      262.32    382.98
    ## subject_4  82    232.23      294.83    328.46
    ## subject_5  89    277.99      288.21    307.44

You can see all of our variables. Each subject has an IQ score and average noseDwellTimes for Movie stars, Athletes, and Nobel.

Notice that our table has one row per subject, and multiple *observations* for each subject in different columns. When the table is organized like this, it is referred to as *wide-format*

### Quick data summary

Descriptive statistics are ways to summarize *your data*. That is, they don't infer any conclusions from your data to the larger population from which your subjects come (you'll need **inferential statistics** for that...don't worry, it's coming). Let's get some basic descriptive stats on this dataset.

``` r
# if you need to install the psych library:
# install.packages("psych", dependencies=TRUE)
library(psych)  # this is an external library that has some really useful tools within it

describe(dt)
```

    ##             vars  n   mean    sd median trimmed   mad    min    max  range
    ## IQ             1 40  94.95 15.71  95.00   95.06 11.86  57.00 128.00  71.00
    ## noseMovie      2 40 249.81 26.58 250.50  249.33 31.50 199.78 316.19 116.41
    ## noseAthlete    3 40 298.36 24.30 297.72  297.86 23.60 250.35 358.75 108.40
    ## noseNobel      4 40 342.80 46.71 342.10  342.65 54.63 234.73 431.02 196.29
    ##              skew kurtosis   se
    ## IQ          -0.04    -0.27 2.48
    ## noseMovie    0.22    -0.68 4.20
    ## noseAthlete  0.17    -0.18 3.84
    ## noseNobel   -0.07    -0.48 7.38

You can see, for instance, that subjects spent an average of 249.81 ms looking at the noses of movie stars, and 342.80 ms looking at the noses of Nobel laureates. In this sample, participants spent longer (on average) looking at the noses of Nobel winners than movie stars. But that's all you can say. You cannot generalize that conclusion to the population at large.

Yet, the whole point about doing an experiment is because we want to say something about how people behave *in general*. In the case of our eye-tracking experiments, we want to say something about how healthy college-age individuals view stimuli from different image categories. We collect data by grabbing a (hopefully) random sample of participants, and running them on the experiment. Our **population** here is all healthy college-age individuals, and our **sample** is the group of 40 or so participants that we've collected.

In order to generalize the findings from our **sample** to the **population** we need to think about **inferential statistics**.

Including Plots
---------------
