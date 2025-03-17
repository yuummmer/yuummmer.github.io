---
layout: post
title: "Pixar at the Box Office: Do Sequels Make More Money?"
date: 2025-03-17
categories: [statistics, data-analysis]
permalink: /statistics/box-office/:year/:month/:day/:title/
---

> "Inspirational quote related to the topic."  
> **Author Name, Source, Year**  

### Introduction  

Earlier this year, I explored **[topic]**, inspired by [book/person/event]. My interest in [subject] has grown through my journey into **R and the tidyverse**, and I wanted to put my skills into practice.  

In this post, I’ll walk through how I analyzed **[dataset/topic]**, from obtaining and tidying the data to visualizing the results. If you’re new to **R** and enjoy working with real-world datasets, you’ll find this especially useful!  

---

### The Goal  

The goal of this project was to **[describe objective]**. I was particularly interested in exploring **[specific aspect]** and visualizing it in a meaningful way.  

To do this, I used the **[dataset name]**, which includes information on **[key variables]**. My approach involved:  
1. **Data extraction** – Obtaining the dataset from [source]  
2. **Data cleaning** – Transforming the data into a usable format  
3. **Visualization** – Creating meaningful plots to illustrate findings  

---

### Getting Started  

First, I loaded the necessary packages in **R**:  

```r
library(tidyverse)  # Core data wrangling & visualization
library(readxl)      # Reading Excel files
library(gganimate)   # Creating animated plots
library(scales)      # Formatting axis labels

### Obtaining the Data 

The dataset comes from [source], which provides [type of data]. I used the read_excel function to import the relevant data:

data <- read_excel("data/filename.xlsx", sheet = "ESTIMATES", range = "C17:R258")

Let's take a quick look at the structure of the dataset:

glimpse(data)
head(data)

### Tidying the Data
The raw data was in wide format, with each year as a column. To make it more useful for visualization, I transformed it into long format using pivot_longer():

tidy_data <- data %>%
  pivot_longer(cols = starts_with("19") | starts_with("20"), 
               names_to = "year", 
               values_to = "value") %>%
  mutate(year = as.integer(year))

### Visualizing the Data
With the cleaned dataset, I created a ggplot visualization:

ggplot(tidy_data, aes(x = year, y = value, color = category)) +
  geom_line() +
  labs(title = "Data Trend Over Time",
       x = "Year", 
       y = "Value") +
  theme_minimal()

To take it further, I used gganimate to show changes over time:

animated_plot <- ggplot(tidy_data, aes(x = year, y = value, color = category)) +
  geom_point() +
  transition_reveal(year)

animate(animated_plot, fps = 10, duration = 5)

Key Takeaways
[Main finding 1] – Description
[Main finding 2] – Description
[Main finding 3] – Description
This project reinforced my understanding of data wrangling, visualization, and animation in R. If you're interested in trying a similar analysis, I highly recommend TidyTuesday as a great place to start!

Let me know what you think, and feel free to share your own visualizations! 🚀