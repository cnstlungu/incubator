Data Analysis Project
================
Constantin Lungu
Thursday, April 16, 2015

*Disclaimer: This was created as an assignment for the class 'Data Science and Inferential Statistics' on Coursera.*

Introduction
============

The research question at hand is whether a relation between the race and the education level of people in the US exists. It would be interesting to see whether people from different backgrounds have equal access to different levels of education. Another reason to consider this is becuase access to education is one of the main traits of racial inequality, hence data pointing in this direction could be useful for sociological research.

Data
====

*Excerpted from the GSS project description*

The [GSS](https://en.wikipedia.org/wiki/General_Social_Survey) aims to gather data on contemporary American society in order to monitor and explain trends and constants in attitudes, behaviors, and attributes; to examine the structure and functioning of society in general as well as the role played by relevant subgroups; to compare the United States to other societies in order to place American society in comparative perspective and develop cross-national models of human society; and to make high-quality data easily accessible to scholars, students, policy makers, and others, with minimal cost and waiting.

Data collection
---------------

The GSS used computer assisted Personal Interviewing (CAPI). There are no printed questionnaires, but the showcards are still printed. Manual edits and keypunching are eliminated. Training includes learning how to operate CAPI, sampling procedures which were reviewed by having interviewers take a training quiz after they had studied the sampling instructions specific to this study.

### Sampling instructions

The 2010 NORC National Sample Design also contains 1,516 second-stage selections (segments) compared to 899 for the 2000 NORC National Sample Design.

### Pre-analysis data-processing

First, the relevant data for this issue was selected : race and degree.

``` r
kx  <- gss[,c("race","degree")]
```

Then, all rows with missing (NA) values were removed (1021 of out initial 57061) - so that only the rows with all the data remain.

``` r
kx  <- kx[complete.cases(kx),]
```

Cases
-----

The cases are the 56.051 people's answers : pairs of observations in our data set.

Variables
---------

We will be studying two categorial variables:

**race** - categorial - with levels:

``` r
levels(kx$race)
```

    ## [1] "White" "Black" "Other"

**degree**

``` r
levels(kx$degree)
```

    ## [1] "Lt High School" "High School"    "Junior College" "Bachelor"      
    ## [5] "Graduate"

Study
-----

The GSS study is an observational study - given the fact that it's observing people's traits. Sampling was done in order to capture a reliable image of the societal make-up.

Scope of inference - generalizability
-------------------------------------

The population of interest is the population of USA.

*Excerpted from the GSS project description*

The sample is a multi-stage area probability sample to the block or segment level. At the block level, however, quota sampling is used with quotas based on sex, age, and employment status. The cost of the quota samples is substantially lecost of a full probability sample of the same size, but there is, of course, the chance of sample biases mainly due to not-at-homes cost of a full probability sample of the same size, but there is, of course, the chance of sample biases mainly due to not-at-homes o canvass and interview only after 3:00 p.m. on weekdays or during the weekend or holidays.

The Primary Sampling Units (PSUs) employed are Standard Metropolitan Statistical Areas (SMSAs) or non-metropolitan counties selected in NORC's Master Sample. These SMSAs and counties were stratified by region, age, and race before selection.2

The units of selection of the second stage were block groups (BGs) and enumeration districts (EDs). These EDs and BGs were stratified according to race and income before selection.3 The third stage of selection was that of blocks.

The blocks were selected with probabilities proportional to size. In places without block statistics, measures of size for the blocks were obtained by field counting. The average cluster size is five respondents per cluster. This provides a suitable balance of precision and economy.

At the block or segment level, the interviewer begins a travel pattern at the first DU (dwelling unit) from the northwest corner of the block and proceeds in a specified direction until the quotas have been filled.

Judging by this, one can conclude that the results of the study can be generalized to the whole population of the USA.

Scope of inference - causality
------------------------------

This data **cannot** be used to establish causal relationship because this in an observational study - not an experiment. Correlation does not imply causation.

Exploratory data analysis
=========================

First, a summary of the data will be printed out:

``` r
summary(kx)
```

    ##     race                  degree     
    ##  White:45602   Lt High School:11822  
    ##  Black: 7716   High School   :29287  
    ##  Other: 2733   Junior College: 3070  
    ##                Bachelor      : 8002  
    ##                Graduate      : 3870

Alternatively, a plot with a table with both variables can be displayed.

``` r
table(kx)
```

    ##        degree
    ## race    Lt High School High School Junior College Bachelor Graduate
    ##   White           8732       24129           2420     6961     3360
    ##   Black           2365        4001            464      616      270
    ##   Other            725        1157            186      425      240

Next, the absolute proportion - what % of people have are of a race and have a specific level of education - will be analyzed.

``` r
prop.table(table(kx))
```

    ##        degree
    ## race    Lt High School High School Junior College    Bachelor    Graduate
    ##   White    0.155786694 0.430482953    0.043174966 0.124190469 0.059945407
    ##   Black    0.042193716 0.071381420    0.008278175 0.010989991 0.004817042
    ##   Other    0.012934649 0.020641915    0.003318406 0.007582380 0.004281815

To assist the data exploration process, it will be plotted onto a tile chart. The plot will yield insight into what the overall situation looks like.

``` r
plot(kx)
```

![](project_files/figure-markdown_github/unnamed-chunk-8-1.png)

Exploratory Data Analysis Conclusion
------------------------------------

Exploratory data analysis suggest that there is a sensible difference in the distribution. This is a first evidence that race is correlated with a certain degree of education.

Inference
=========

State hypotheses
----------------

**H0**: (Nothing going on) - Education **does not have** a relationship to race - they are independent, so education does not vary by race. **H1**: (Something going on) - Education and rate *do have* a relationship - they are dependent, so education does vary by race.

Check conditions
----------------

### 1. Independence

-   random sample -&gt; Data was collected by using random samples OK
-   56.051 people &lt; 10 % all population OK
-   respondents answered once OK

### 2.Sample size

-   each scenario has at least 5 counts - OK

Method to be used
-----------------

The Chi-square independence test will be used for this given then categorial data with multiple levels (both the explanatory and the response variable)

Perform inference
-----------------

Hypotheses set - OK Conditions met - OK -&gt; **theoretical can be used**

First, the overall education make up for the population as a whole will be computed:

``` r
table(kx$degree)/nrow(kx)
```

    ## 
    ## Lt High School    High School Junior College       Bachelor       Graduate 
    ##     0.21091506     0.52250629     0.05477155     0.14276284     0.06904426

Basically, if 'nothing is going on', this distribution should be followed closely for every race in the dataset.

``` r
prop.table(table(kx),1)
```

    ##        degree
    ## race    Lt High School High School Junior College   Bachelor   Graduate
    ##   White     0.19148283  0.52912153     0.05306785 0.15264681 0.07368098
    ##   Black     0.30650596  0.51853292     0.06013478 0.07983411 0.03499222
    ##   Other     0.26527625  0.42334431     0.06805708 0.15550677 0.08781559

One can see, in particular, that people belong to "Black" racial category have higher incidence of "Lt High School" and almost half the incidence of "Bachelor", "Graduate"

The "Other" racial group scores higher levels of education than average, pointing to a Model Minority e.g. Indian-Americans, Asian-Americans

Next, the two columns will be transposed , so the Chi-square test could be ran.

``` r
table(kx$degree,kx$race)
```

    ##                 
    ##                  White Black Other
    ##   Lt High School  8732  2365   725
    ##   High School    24129  4001  1157
    ##   Junior College  2420   464   186
    ##   Bachelor        6961   616   425
    ##   Graduate        3360   270   240

Now, running the chi-squared test:

``` r
chisq.test(table(kx$degree,kx$race))
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table(kx$degree, kx$race)
    ## X-squared = 931.06, df = 8, p-value < 2.2e-16

**Given the extremely low p-value &lt; 2.2e-16, we will reject the null hypothesis in favour of the alternative hypothesis.**

Interpret results
-----------------

This data provides convincing evidence that belonging to a race and having a specific level of education are associated.

Meanwhile, we cannot establish causal relationships as this is an observational study.

Conclusion
==========

While the null hypothesis was rejected (so something is going on) and the data provides convincing evidence that race and education level are associated, as previously stated no causal statements can be made.

One should also think about confounding variables - such as income. In fact, for future research one should also study income, home and car ownership (the two latter having the same confounding variable).

References
==========

### Data citation:

Smith, Tom W., Michael Hout, and Peter V. Marsden. General Social Survey, 1972-2012 \[Cumulative File\]. ICPSR34802-v1. Storrs, CT: Roper Center for Public Opinion Research, University of Connecticut /Ann Arbor, MI: Inter-university Consortium for Political and Social Research \[distributors\], 2013-09-11. <doi:10.3886/ICPSR34802.v1>

### Data link:

``` r
load(url("http://bit.ly/dasi_gss_data"))
```

### Other references:

<http://publicdata.norc.org/GSS/DOCUMENTS/BOOK/GSS_Codebook_AppendixB.pdf>

<http://en.wikipedia.org/wiki/Model_minority>

Appendixes
==========

Data Sample
-----------

This will provide first 50 (~ 1 page) of data

``` r
head(kx,50)
```

    ##     race         degree
    ## 1  White       Bachelor
    ## 2  White Lt High School
    ## 3  White    High School
    ## 4  White       Bachelor
    ## 5  White    High School
    ## 6  White    High School
    ## 7  White    High School
    ## 8  White       Bachelor
    ## 9  Black    High School
    ## 10 Black    High School
    ## 11 Black    High School
    ## 12 Black Lt High School
    ## 13 Black Lt High School
    ## 14 Black Lt High School
    ## 15 Black Lt High School
    ## 16 Black    High School
    ## 17 White    High School
    ## 18 White Lt High School
    ## 19 White       Bachelor
    ## 20 White    High School
    ## 21 White    High School
    ## 22 White    High School
    ## 23 White    High School
    ## 24 White    High School
    ## 25 White       Bachelor
    ## 26 White    High School
    ## 27 White    High School
    ## 28 White    High School
    ## 29 White    High School
    ## 30 White Lt High School
    ## 31 White Lt High School
    ## 32 White    High School
    ## 33 White       Bachelor
    ## 34 White Lt High School
    ## 35 White    High School
    ## 36 White    High School
    ## 37 White    High School
    ## 38 White Lt High School
    ## 39 White Lt High School
    ## 40 White    High School
    ## 41 White Lt High School
    ## 42 White    High School
    ## 43 White Lt High School
    ## 44 White Lt High School
    ## 45 White Lt High School
    ## 46 White    High School
    ## 47 White    High School
    ## 48 White    High School
    ## 49 White Lt High School
    ## 50 White Lt High School
