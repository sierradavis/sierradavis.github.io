---
layout: post
title: "Query a De-Identified EMR using R"
date: 2019-10-10
---

<p>In this post, I will be using the DBI and dbplyr to work with the data warehouse tables as a local data frame. </p>

<p>A de-identified data warehouse from multiple health systems can be an excellent resource for biomedical research projects. 
One of the key benefits is the vast number of patients at various facilities. Those facilities can be considered primarily adult 
organizations or pediatric organizations; however, due to the de-identified data warehouse, those indications are not readily available. 
For our research question, we may want to analyze pediatric patients at pediatric organizations to determine if there are variabilities as 
it relates to the treatment of care. This post will help us identify pediatric organizations by querying our patient population and facility data and using descriptive statistics to classify those organizations. </p>


```
library(DBI)         
library(odbc)        
suppressPackageStartupMessages( library(dplyr) )  
suppressPackageStartupMessages( library(dbplyr) )  
```

<p>For this example, I referenced:  </p>

[Best practices for working with databases](https://resources.rstudio.com/webinars/best-practices-for-working-with-databases-march-2018-edgar-ruiz)

```


SERVER   <- "servername"
VERSION  <- 2016    
DATABASE <- paste0("DBNAME", VERSION)
CAPTION  <- paste("EMR Data", VERSION)
                   

EMR <- dbConnect(odbc(),
                         Driver="SQL Server",
                         Server=SERVER,
                         Database=DATABASE,
                         UID=Sys.getenv("EMR_UID"),
                         PWD=Sys.getenv("EMR_PWD"))

encounter         <- tbl(EMR, in_schema("dbo", "ENCOUNTER"))
facility          <- tbl(EMR, in_schema("dbo", "FACILITY"))
```

<p>Now that I have the tables I want to use, I used dplyr to filter and summarize the data by Facility.</p>
```
facilityStats <-
  inner_join(encounter, facility, by="FACILITY_ID")   %>%
  select(System = SYSTEM_ID,
         Facility     = FACILITY_ID,
         AgeYears     = AGE_YEARS,
         AgeDays      = AGE_DAYS)                    %>%
  filter(AgeYears >= 0)                                 %>% 
  
  group_by(System, Facility)                      %>%  
  summarize(Encounters    = n(),
            AgeYearsMean  = mean(AgeYears, na.rm=TRUE),
            AgeYearsSD    = sd(AgeYears,   na.rm=TRUE),
            AgeDaysMean   = mean(AgeDays,  na.rm=TRUE),
            AgeDaysSD     = sd(AgeDays,    na.rm=TRUE)) %>%  
  ungroup()                                             %>%
  
  arrange(AgeYearsMean)                                 %>%
  collect()



pedStats <- facilityStats %>% filter(AgeYearsMean < 20)

```

<p>My final step is to create a temp table in the database for future use.</p>

```
copy_to(EMR, pedStats, "PediatricFacilities")

```

