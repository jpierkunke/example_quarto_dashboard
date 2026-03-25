# Making a Quarto dashboard

This demo shows how to adapt a script (`01_water_quality_viz.R`) into a Quarto dashboard (`index.qmd`).

- A simpler version of the dashboard, `02_initial_dashboard.qmd`, is included to demonstrate how one might start turning the script into a dashboard, while `index.qmd` is the result of more modifications and additions.
- The Quarto dashboard file is called `index.qmd` so that the rendered document is `index.html`, which GitHub Pages will use to make a webpage. More details below.

This demo is based on the Quarto documentation for [dashboard layout](https://quarto.org/docs/dashboards/layout.html).

# Table of Contents
* [The script](#script)
* [Making the script into a dashboard: First pass](#first-pass)

<a name="script"/>

## The script

To start, look at the initial script, `01_water_quality_viz.R`.

- First, the script reads in a comma-separated values (csv) file of water quality data, reads in stream gauge data from USGS using the `dataRetrieval` package, and renames the variables.
- After that, the script is broken into three sections with section headings:
    - monthly summary statistics
        - compute some summary statistics about when temperature, dissolved oxygen (DO) and pH exceed certain threshholds
        - make a table
    - plot each variable in its own subfigure
        - join the two data sets (data from the water quality csv file and data from dataRetrieval)
        - pivot the combined dataset for plotting
        - make the parameter column a factor variable
        - rename the levels (parameter names) for the plots
        - plot each variable in its own facet/panel
    - plot each variable, stacked so that dates align
        - make the plot
        - save it to file



<a name="first-pass"/>

## Making the script into a dashboard: First pass {#first-pass}

Let's start by trying to make a dashboard with the two plots side by side, and the table of summary statistics below.

We can start by making a new Quarto document as we would do for any other kind of Quarto document: html, pdf, Word, presentation, etc.

Click the menu that looks like a blank page with a green plus sign, in the upper-left corner of the R Studio window.

Select "Quarto Document..." and in the window that pops up, click "Create Empty Document".

Let's make a simple header to start:

```
---
title: "Visualizing stream gauge and water quality data"
author: "Jess Kunke and Angie Reed"
format: dashboard:
---
```

After the header and before we make the dashboard content of the document, we make a code chunk that does the setup: loading necessary packages, reading in the data, etc. The `message: false` option ensures that any messages from loading packages and running these functions will not be displayed in the dashboard when we render it.

```{r}
#| message: false

library(knitr)
library(plotly)
library(tidyverse)
library(dataRetrieval)
library(lubridate)

# read in data
water.quality = read_csv("water_quality_data.csv")
# change values of -1 x 10^6 to NA
water.quality = water.quality %>%
  mutate(across(where(is.numeric), ~ na_if(., -1e6))) %>%
  rename(Time = DateTime)

siteNo = "01018035" # Lowery bridge
# For a full list of pCodes:
# https://help.waterdata.usgs.gov/code/parameter_cd_query?fmt=rdb&inline=true&group_cd=%
# - 00060 is discharge
# - 00010 is stream temperature in Celcius
pCodes = c("00060","00010")
start.date = "2022-01-01"
end.date = "2022-12-31"
# load the instantaneous measurements
streamflow = readNWISuv(siteNumbers = siteNo,
                         parameterCd =  pCodes,
                         startDate = start.date,
                         endDate = end.date)

# rename some columns
streamflow = streamflow %>%
  renameNWISColumns() %>%
  rename(Time = dateTime) %>%
  select(Time, Flow_Inst)
```




     
