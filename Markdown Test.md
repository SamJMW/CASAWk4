# Week 4 Homework Task

**The task this week is to:**

-   Read in global gender inequality data Join the global gender inequality index to spatial data of the World, creating a new column of difference in inequality between 2010 and 2019.

-   Note this download has become temperamental but the .geojson should work. Andy will provide the data if not. Share it with the World on GitHub Add you repository URL to the circulated spreadsheet

-   *Tip the countrycode R package will be helpful!*

-   *Tip the gender inequality has changed in the last year, you will find what you need in the “All composite indices and components time series (1990-2021)” dataset, the metadata file beneath it will explain what the columns are.*

```{r}
library(dplyr)
library(tidyverse)
library(here)
library(janitor)
library(sf)

GIData <- read.csv("HDR23-24_Composite_indices_complete_time_series.csv")

GIData <- GIData %>%
  mutate(GIData, GI_diff_10_19 = GIData$gii_2010 - GIData$gii_2019)

GIData <- subset(GIData, select = c("iso3", "country", "hdicode", "region", "GI_diff_10_19"))

Top_10 <- GIData %>%
  arrange(desc(GI_diff_10_19)) %>%
  slice_head(n = 10)

Bottom_10 <- GIData %>%
  arrange(GI_diff_10_19) %>%
  slice_head(n=10)

World <- st_read("World_Countries_Generalized.shp")

GIData <- clean_names(GIData)
World <- clean_names(World)

join_data <- left_join(World, GIData, by= c("countryaff" = "country"))

```
