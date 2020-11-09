---
layout: post
title: "Using R and Python to advance pediatric research"
date: 2019-12-17
---


<p> My previous post discussed team work in data science. This post will be a brief overview of how I use R and Python to work with our clinical team to identify valuable insights. </p>


<p>The focus of our project is to develop cohorts of pediatric patients with cancer. The workflow consists of:</p>

* Identifying patients and querying their entire EHR record.
* Converting the encounter data into a patient centric temporal dataset.
* Clinical team reviews patient data and identifies filters needed or new information that should be obtained.
* Data scientist applies filters and other indicators to assess key insights identified by the clinical team.
* Clinical team reevaluates patient data.
* Data scientist visualize and analyze dataset for significance.

![Plot](/assets/images/reticulate.PNG)


In order to streamline my workflow, I used the [Reticulate package](https://github.com/rstudio/reticulate). This package integrates R and Python in RStudio. I use R to securely connect and query our EHR database, and tidy data for researchers. I then use Python to develop temporal files for researchers to identify valuable insights.



<p>There shouldn't be a tug of war with what language to use in data science. I find both open source communities to be helpful and always engaging. It is always important to understand the end goal of your data science project. I believe R and Python can used together in a meaningful way to further research.</p>

