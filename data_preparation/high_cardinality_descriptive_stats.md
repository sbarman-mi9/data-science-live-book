# High Cardinality Variable in Descriptive Stats

  
## What is this about?
  
A **high cardinality** variable is one in which it can take _many_ different values. For example country. 

This chapter will cover cardinality reduction based on Pareto rule, using the `freq` function which gives a quick view about where the most of values are concentrated and variable distribution.



 
 
<br> 

## High Cardinality in Descriptive Statistics

The following example contains a survey of 910 cases, with 3 columns: `person`, `country` and `has_flu`, which indicates having such illness in the last month.


```r
library(funModeling) 
```

`data_country` data comes inside `funModeling` package (please update to release 1.6).

Quick `data_country` profiling (first 10 rows)


```r
# plotting first 10 rows
head(data_country, 10)
```

```
##     person     country has_flu
## 478    478      France      no
## 990    990      Brazil      no
## 606    606      France      no
## 575    575 Philippines      no
## 806    806      France      no
## 232    232      France      no
## 422    422      Poland      no
## 347    347     Romania      no
## 858    858     Finland      no
## 704    704      France      no
```

```r
# exploring data, displaying only first 10 rows
head(freq(data_country, "country"), 10)
```

<img src="figure/data_preparation_nominal_variable-1.png" title="plot of chunk data_preparation_nominal_variable" alt="plot of chunk data_preparation_nominal_variable" width="400px" />

```
##           country frequency percentage cumulative_perc
## 1          France       288      31.65           31.65
## 2          Turkey        67       7.36           39.01
## 3           China        65       7.14           46.15
## 4         Uruguay        63       6.92           53.07
## 5  United Kingdom        45       4.95           58.02
## 6       Australia        41       4.51           62.53
## 7         Germany        30       3.30           65.83
## 8          Canada        19       2.09           67.92
## 9     Netherlands        19       2.09           70.01
## 10          Japan        18       1.98           71.99
```


```r
# exploring data
freq(data_country, "has_flu")
```

<img src="figure/data_preparation_nominal_variable_2-1.png" title="plot of chunk data_preparation_nominal_variable_2" alt="plot of chunk data_preparation_nominal_variable_2" width="400px" />

```
##   has_flu frequency percentage cumulative_perc
## 1      no       827      90.88           90.88
## 2     yes        83       9.12          100.00
```

<br>

The last table shows there are **70 different countries**, and ~9% of people who had flu -`has_flu="yes"`.

But many of them have almost no participation in the data. This is the _long tail_, so one technique to reduce cardinality is to keep those categories that are present the a high percentahge of data share, for example 70, 80 or 90%, the Pareto principle.


```r
# 'freq' function, from 'funModeling' package, retrieves the cumulative_percentage that will help to do the cut. 
country_freq=freq(data_country, 'country', plot = F)

# Since 'country_freq' is an ordered table by frequency, let's inspect the first 10 rows with the most share.
country_freq[1:10,]
```

```
##           country frequency percentage cumulative_perc
## 1          France       288      31.65           31.65
## 2          Turkey        67       7.36           39.01
## 3           China        65       7.14           46.15
## 4         Uruguay        63       6.92           53.07
## 5  United Kingdom        45       4.95           58.02
## 6       Australia        41       4.51           62.53
## 7         Germany        30       3.30           65.83
## 8          Canada        19       2.09           67.92
## 9     Netherlands        19       2.09           70.01
## 10          Japan        18       1.98           71.99
```

<br>

So 10 countries represent more the 70% of cases. We can assign the category `other` to the remaining cases and plot:


```r
data_country$country_2=ifelse(data_country$country %in% country_freq[1:10,'country'], data_country$country, 'other')
freq(data_country, 'country_2')
```

<img src="figure/data_preparation_profiling_nominal_variable_2-1.png" title="plot of chunk data_preparation_profiling_nominal_variable_2" alt="plot of chunk data_preparation_profiling_nominal_variable_2" width="400px" />

```
##         country_2 frequency percentage cumulative_perc
## 1          France       288      31.65           31.65
## 2           other       255      28.02           59.67
## 3          Turkey        67       7.36           67.03
## 4           China        65       7.14           74.17
## 5         Uruguay        63       6.92           81.09
## 6  United Kingdom        45       4.95           86.04
## 7       Australia        41       4.51           90.55
## 8         Germany        30       3.30           93.85
## 9          Canada        19       2.09           95.94
## 10    Netherlands        19       2.09           98.03
## 11          Japan        18       1.98          100.00
```

<br> 

## Final comments

Low representative categories are sometimes errors in data, such as having: `Egypt`, `Eggypt.`, and may give some evidence in bad habbits collecting data and/or possible errors when collecting from the source.

There is no general rule to shrink data, it depends on each case.

<br>

**Next recommended chapter: <a href="http://livebook.datascienceheroes.com/data_preparation/high_cardinality_descriptive_stats.html">High Cardinality Variable in Predictive Modeling</a>**




