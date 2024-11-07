
><p style="font-size: 12px; color: brown;">"<em> A most gruesome and nerve-racking assignment to finish as my last module with SUSS in exchange for the master's certificate </em>"  </p>


This repository contains the visualization of data that I have done for ANL501 Data Visualisation and Storytelling course offered by the Master of Analytics and Visualisation (MAVI) programme at SUSS.

In this course, we explore the use of R programming to construct data visualizations. The course follows <a href="https://socviz.co/"> Data Visualization: A Practical Introduction </a> by Kieran Healy. The data can be found <a href="https://data.gov.sg/datasets?query=resale+flat+price+based+on+approval&page=1&resultId=189"> Resale Flat Price </a>.


# Singapore HDB Resale Market Trend From 1990 to the Present

**Author**: Sally Marcellina Yeo  

## Overview

This project analyzes the Singapore HDB resale market trends from 1990 to the present. The analysis uses various data visualization techniques to provide insights into key factors influencing the market, such as:

- Flat types
- Location
- Remaining lease years
- Regulatory measures
- Economic factors (e.g., interest rates)

The report also examines how significant economic events, such as the 1998 Asian Financial Crisis and the 2007 Global Financial Crisis, along with government interventions, impacted HDB resale prices.

## Key Features

- **Line Plots**: Visualize the time-series data for average prices per square foot (psf) across different periods.
- **Bar Charts**: Show transaction volumes over time to reflect market activity.
- **Scatter Plots**: Analyze the correlation between average psf prices and factors such as remaining lease years, flat types, and location.
- **Map Plots**: Present the average psf prices geographically across different towns in Singapore.

## Insights

- **Economic Impact**: Regulatory measures and economic factors like interest rates significantly influence the HDB resale market.
- **Price Determinants**: Flat type, age, and location have a strong impact on resale prices.
- **Government Measures**: Government interventions during financial crises helped stabilize the market.

## Project Structure

- **RMarkdown File**: The analysis is contained in the file `ANL501_ECA_Sallyyeo001_SallyMarcellinaYeo.Rmd`, which can be used to generate a Word document report.
- **Data Visualizations**: Includes a variety of plots (line plots, bar charts, scatter plots, map plots) to explore and understand market trends.

## The R Code
Here is the steps for importing the data in R Studio.

Setting the working directory
```{r echo=F}
# set your working directory here
setwd("/Users/sallyyeo/Desktop/501ECA")
```

```{r import.data, echo=F}
df.resale.approved.1990to1999 = read.csv("ResaleFlatPricesBasedonApprovalDate19901999.csv")
df.resale.approved.2000to2012 = read.csv("ResaleFlatPricesBasedonApprovalDate2000Feb2012.csv")
df.resale.registered.2012to2014 = read.csv("ResaleFlatPricesBasedonRegistrationDateFromMar2012toDec2014.csv")
df.resale.registered.2015to2016 = read.csv("ResaleFlatPricesBasedonRegistrationDateFromJan2015toDec2016.csv")
df.resale.registered.2017topresent = read.csv("ResaleflatpricesbasedonregistrationdatefromJan2017onwards.csv")
```

Next, I check the data types and skim through the data
```{r check.data.type, include=F }
str(df.resale.approved.1990to1999)
str(df.resale.approved.2000to2012)
str(df.resale.registered.2012to2014)
str(df.resale.registered.2015to2016)
str(df.resale.registered.2017topresent)
```

```{r skim.data, include=F }
#install.packages("skimr")
library(skimr)

skim(df.resale.approved.1990to1999)
skim(df.resale.approved.2000to2012)
skim(df.resale.registered.2012to2014)
skim(df.resale.registered.2015to2016)
skim(df.resale.registered.2017topresent)
```

Following are the list of data issues found using str() and skim():

  * Data type of attribute "resale_price" are different between the data frames.
  * Different number of variables in the data frames.
  * Data type of the month is not date but character.
  * Different upper and lowercase values of attribute "flat_model".
  * Spelling difference for one of the attribute "flat_type" value.
  
Following are the data cleaning steps in order:

  * Convert the data type of "resale_price" to numeric to match with other data frames.
  * Extract the year value from the "remaining_lease" of 2017 to present data set using str_extract() from "stringr" library, and convert it from character to integer.
  * Merge all sorted data frames using bind_rows() from "dplyr" library.
  * Convert "month" variable to Date type.
  * Calculate the value of "remaining_lease" for all combined rows.
  * Convert all numeric data to integer since there is no decimal places needed.
  * Convert the values of "flat_model" variable to uppercase.
  * Replace "MULTI GENERATION" with "MULTI-GENERATION" in the "flat_type" column.
  * Create a new variable "floor_area_psf" to calculate the floor area in square foot.
  * Create a new variable "dollar_psf" to calculate the price per square foot (psf) of a flat. Analyzing the price in psf will be better the gauge the value of the house.

# Data Cleaning
Convert variables to the correct data types.
```{r data.cleaning, include=F}
#install.packages("stringr")
library(stringr)  # to use str_extract()

# convert variables to correct types
df.resale.approved.1990to1999$resale_price = as.numeric(df.resale.approved.1990to1999$resale_price)
df.resale.registered.2017topresent$remaining_lease <- as.integer(str_extract(df.resale.registered.2017topresent$remaining_lease, "\\d{2}"))
```

Combine all the data frames as one data frame.
```{r, include=F}
#install.packages("dplyr")
library(dplyr)  # to use bind_rows

# combine all data frames
data <- bind_rows(df.resale.approved.1990to1999, df.resale.approved.2000to2012, df.resale.registered.2012to2014, df.resale.registered.2015to2016, df.resale.registered.2017topresent)
```

Convert month to date type.
```{r, include=F}
#install.packages("zoo")
library(zoo)  # to use as.yearmon()

# convert variables to correct types
data$month <- as.Date(as.yearmon(data$month)) # using zoo package
```

Calculate the value of remaining lease for all combined rows and convert numeric to integer.
```{r, include=F}
data %>%
  mutate(remaining_lease = 99-(as.numeric(format(month,'%Y'))-lease_commence_date)) -> 
  data

data %>% 
  mutate_if(is.numeric, as.integer) -> data
```

Checking unique values.
```{r, include=F}
# check for unique variable values
unique(data$flat_type)
unique(data$town)
unique(data$flat_model)

# convert to uppercase
data$flat_model <- toupper(data$flat_model)

# replace all the "MULTI GENERATION" to "MULTI-GENERATION"
data$flat_type <- gsub("MULTI GENERATION", "MULTI-GENERATION", data$flat_type)
```

Calculate the floor area in sqf and the dollar per sqf.
```{r, include=F}
# calculate floor area in sqf
data$floor_area_sqf <- round(data$floor_area_sqm * 10.764)

# create new variable "dollar_psf"
data %>%
  mutate(dollar_psf = resale_price/floor_area_sqf) -> data
```

Overview of the clean data:
``` {r head.ten, echo=F}
str(data)
```
Output:
```{plaintext}
'data.frame':	920241 obs. of  11 variables:
 $ month              : chr  "1990-01" "1990-01" "1990-01" "1990-01" ...
 $ town               : chr  "ANG MO KIO" "ANG MO KIO" "ANG MO KIO" "ANG MO KIO" ...
 $ flat_type          : chr  "1 ROOM" "1 ROOM" "1 ROOM" "1 ROOM" ...
 $ block              : chr  "309" "309" "309" "309" ...
 $ street_name        : chr  "ANG MO KIO AVE 1" "ANG MO KIO AVE 1" "ANG MO KIO AVE 1" "ANG MO KIO AVE 1" ...
 $ storey_range       : chr  "10 TO 12" "04 TO 06" "10 TO 12" "07 TO 09" ...
 $ floor_area_sqm     : num  31 31 31 31 73 67 67 67 67 67 ...
 $ flat_model         : chr  "IMPROVED" "IMPROVED" "IMPROVED" "IMPROVED" ...
 $ lease_commence_date: int  1977 1977 1977 1977 1976 1977 1977 1977 1977 1977 ...
 $ resale_price       : num  9000 6000 8000 6000 47200 46000 42000 38000 40000 47000 ...
 $ remaining_lease    : int  NA NA NA NA NA NA NA NA NA NA ...
```

# Data Analysis
Aggregating data and assigned to new data frame
``` {r data.processing.general.line.plot, echo=F}
# create a new data frame and aggregrate on the date to get the average psf
psf_df <- data %>%
  group_by(month) %>%
  summarise(average_dollar_psf = mean(dollar_psf))
```

Line plot showing average price psf annually
``` {r general.line.plot.psf, echo=F}
##### line plot average price psf per year #####
#install.packages("ggplot2")
library(ggplot2)

# plot using average_dollar_psf with ribbon
ggplot(psf_df, aes(x = month, 
                 y = average_dollar_psf)) +
  geom_line() +
  geom_ribbon(aes(xmin = as.Date("1997-07-01"), 
                  xmax = as.Date("2007-09-30"), 
                  y = average_dollar_psf), 
              fill="red", 
              alpha=0.5, 
              inherit.aes = F) +
  geom_ribbon(aes(xmin = as.Date("2009-01-01"), 
                xmax = as.Date("2014-12-31"), 
                y = average_dollar_psf), 
            fill="red", 
            alpha=0.5, 
            inherit.aes = F) +
  geom_ribbon(aes(xmin = as.Date("2020-04-01"), 
                  xmax = as.Date("2024-02-01"), 
                  y = average_dollar_psf), 
              fill="red", 
              alpha=0.5, 
              inherit.aes = F) +
  scale_y_continuous(labels = scales::dollar_format(prefix = "$"), 
                     breaks = seq(0, 
                                  max(psf_df$average_dollar_psf), 
                                  by = 50)) +
  scale_x_date(breaks = seq(as.Date("1990-01-01"), 
                            as.Date("2024-02-01"), 
                            by = "1 years"), 
               date_labels = "%Y") +
  labs(title = "Average Price psf of Resale HDB\nfrom 1990 to 2024",
       x = "Year",
       y = "Average Price psf (SGD)") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, 
                                   hjust = 1),
        plot.title = element_text(size = 10)) +
  ylim(0, 650)
```
