---
layout: post
title: "My Workflow using R Markdown Notebooks"
date: 2019-12-2
---


<p>I use data science methods to answer challenging pediatric research questions. I've used R Markdown Notebooks in my workflow to answer those questions. R Markdown Notebooks allow me to analyze and collaborate with other data scientists. This blog post will provide a summary of the steps I take to complete tasks using R Markdown.</p>

# Setup
<p>When setting up, I update the title to be more specific and add libraries I need in the first chunk. I also add sections defining the chunks. Additionally, I can add information such as text or images that will enhance the notebook.</p>


```

library(dplyr)  # select, group_by, ...
library(dbi) # sql
library(kableExtra)  # kable_styling

```


# Import Data
<p>My next step is importing data. For my analysis, I use a de-identified EHR database installed in a SQL cloud environment. I can use the DBI package to execute SQL queries and assign those results to a data frame.</p>


```
{sql, connection=EMR,output.var="example_output"}
SELECT DISTINCT 
 PATIENT_NUM            AS PatientNum,
  ADMITTEDTM      AS AdmittedTime,
  DISCHARGEDTM      AS DischargeTime,
  ACCOUNT_ID         AS AccountID,
  AGEYEARS        AS AgeYears,
  PATIENTSEX                AS Gender,
  PATIENTRACE                   AS Race,
  DIAGNOSISTYPE      AS DiagnosisType,
  DIAGNOSISCODE      AS DiagnosisCode,
  DIAGNOSISDESC      AS DiagnosisDescription,
  INSURANCE         AS Insurance
FROM
  DIAGNOSIS     AS Diagnosis_Table

  .
  .
  .
  .
  .
  .


WHERE
  AGEYEARS <= 18
  AND 

  (
    (DIAGNOSISTYPE = 'ICD10-CM' AND
     DIAGNOSISCODE IN ('G03.9', 'G03.8')
    )
    OR
    (DIAGNOSISTYPE = 'ICD9' AND
     DIAGNOSISCODE IN ('322.9', '03.6')
    )
  )

```

# Tidy Data
<p>I tidy any data into a data frame so that I can explore it.</p>


```
stats <- example_output %>% 
filter(AgeYears > 3)                                 %>% 
  arrange(PatientNum)                                 
  ```

# Explore Data
<p>I can summarize my data with descriptive statistics to identify interesting data points. To assess this, I use kable. I can also visualize my data using ggplot.</p>

```
stats %>% 
  
  group_by(Gender)                      %>%  
  summarize(Encounters    = n(),
            AgeYearsMean  = mean(AgeYears, na.rm=TRUE),
            AgeYearsSD    = sd(AgeYears,   na.rm=TRUE),
            AgeDaysMean   = mean(AgeDays,  na.rm=TRUE),
            AgeDaysSD     = sd(AgeDays,    na.rm=TRUE)) %>%  
  ungroup()                                             %>%
  
  arrange(AgeYearsMean)  %>% 
  kable("html", format.args=list(big.mark=","),
      caption ="Age Descriptive Statistics by Gender")    %>%
kable_styling(bootstrap_options=c("striped", "bordered", "condensed"),
              full_width=TRUE, 
              position="left",
              font_size=11)                           %>%
column_spec(1, bold=TRUE)                              
  ```

<p>This is a brief summary of how I use R Markdown Notebooks to answer research questions.</p>

