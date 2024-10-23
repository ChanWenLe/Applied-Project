
><p style="font-size: 12px; color: brown;">"<em> A most gruesome and nerve-racking assignment to finish as my last module with SUSS in exchange for the master's certificate </em>"  </p>


This repository contains the visualization of data that I have done for ANL501 Data Visualisation and Storytelling course offered by the Master of Analytics and Visualisation (MAVI) programme at SUSS.

In this course, we explore the use of R programming to construct data visualizations. The course follows <a href="https://socviz.co/"> Data Visualization: A Practical Introduction </a> by Kieran Healy and the course introduction can be found here [<a href="https://nicholas-sim.github.io/ANL501-Data-Visualisation-and-Storytelling/seminar_1/"> Seminar 1 </a>].


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

## Getting Started

### Prerequisites

Make sure you have the following R packages installed:

```r
install.packages(c("skimr", "stringr", "zoo", "tidyverse", "patchwork", "leaflet", "ggplot2", "dplyr", "sf"))

