---
layout: post
title: "Pixar’s Box Office Formula: Do Great Stories Make Great Sequels?"
date: 2025-03-17 10:00:00 -0700
categories: bioinformatics journey
permalink: /bioinformatics/journey/:year/:month/:day/:title/
---

> "Sequels are not part of our business model. When we have a great story, we'll do a sequel."
> **Ed Catmull (Pixar co-founder)**  

### Introduction  

Earlier this year, I [decided to earn an Applied Bioinformatics certificate](/bioinformatics/journey/2025/03/13/charting-bioinforrmatics/). As I started learning R and the tidyverse, I found myself struggling to follow textbook examples without simply copying and pasting the code or completely zoning out 😵‍💫. I realized I needed more hands-on experience and wanted to actively apply my skills.

To immerse myself in real-world data analysis, I joined the [Data Science Learning Community](https://dslc.io/) on Slack and discovered the #chat-tidytuesday channel. It just so happened to be Tuesday, and a Zoom session was starting in 10 minutes! I listened in as the group explored data from the Long Beach Animal Shelter, and I was fascinated by the many different ways people interpreted the information. Inspired, I decided to join the next TidyTuesday challenge.

In this post, I’ll walk through how I analyzed **Pixar Data**, from obtaining and tidying the data to visualizing the results. If you’re new to **R** and enjoy working with real-world datasets, you’ll find this especially useful!  

---
### The Goal  

The aim of this project was to **fit a linear regression model to the Pixar dataset**, focusing on **understanding interaction effects between variables** and visualizing the results in a meaningful way.  

While the data manipulation itself is relatively straightforward, this project serves as an exercise in applying key concepts from *Chapter 3: Linear Regression*. As I continue learning R and working with linear models, I saw this week's **TidyTuesday** dataset as an opportunity to reinforce my understanding through hands-on analysis.  

For this analysis, I used the `{pixarfilms}` R package by [Eric Leung](https://github.com/erictleung), available through the [TidyTuesday GitHub repository](https://github.com/rfordatascience/tidytuesday/tree/master/data/2025/2025-03-11).  

> The `{pixarfilms}` package provides datasets to explore Pixar films, including details on production, box office performance, and reception. Most of the data is sourced from [Wikipedia](https://en.wikipedia.org/wiki/List_of_Pixar_films).  

My approach involved:  
1. **Data extraction** – Loading the dataset from `{pixarfilms}`  
2. **Data cleaning** – Transforming the data for analysis  
3. **Modeling** – Fitting a linear regression model and examining interactions  
4. **Visualization** – Creating meaningful plots to interpret findings  

---

### Getting Started  

First, I loaded the necessary packages in **R**:  

```r
library(dplyr)
library(readr)
library(lubridate)
library(pixarfilms)
library(ggplot2)
library(scales)
library(tidyr)
library(stringr)
theme_set(theme_light())
```

### Obtaining the Data 

The dataset comes from two sources:  

1. The **TidyTuesday Pixar dataset** ([March 11, 2025](https://github.com/rfordatascience/tidytuesday/tree/main/data/2025/2025-03-11)), which provides information on Pixar films, including box office revenue, production budgets, and audience reception.  
2. The `{pixarfilms}` R package by [Eric Leung](https://github.com/erictleung), which contains additional financial data on Pixar films.  

I imported the data using `readr::read_csv()` for the TidyTuesday dataset and loaded the `{pixarfilms}` package for additional box office data:  

```r
# Read the TidyTuesday datasets
pixar_films_raw <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2025/2025-03-11/pixar_films.csv')

public_response_raw <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/main/data/2025/2025-03-11/public_response.csv')

# Load box office data from pixarfilms package
box_office_raw <- pixarfilms::box_office
```
Let’s start by examining the data. I used View() and the standard head() function to take a quick look at the dataset. Below is a preview of the first few rows of pixar_films_raw:
```r
# A tibble: 6 × 5
   number film           release_date run_time
   <dbl> <chr>          <date>       <dbl>  
1       "Toy Story"      1995-11-22      81  
2       "A Bug's Life"   1998-11-25      95  
3       "Toy Story 2"    1999-11-24      92  
4       "Monsters, Inc." 2001-11-02      92  
5       "Finding Nemo"   2003-05-30     100  
6       "The Incredibles" 2004-11-05    115  

```

#### Data Cleaning and Identifying Sequels  

First, I created a new column to indicate whether a film was a sequel. Since the dataset does not explicitly label sequels, I manually defined a vector of known Pixar sequels and used it to flag films in the dataset:  

```r
# Define a vector of known sequels
sequel_titles <- c("Toy Story 2", "Toy Story 3", "Toy Story 4",
                   "Cars 2", "Cars 3", "Finding Dory", 
                   "Incredibles 2", "Monsters University", "Lightyear")

# Create the is_sequel column
pixar_films <- pixar_films_raw %>%
  select(-number) %>%
  mutate(is_sequel = ifelse(film %in% sequel_titles, 1, 0))
```
In the pixar_films dataframe I picked off the first column, "number" that just contained a bunch of numbers using select(). This new **is_sequel** column allows us to compare sequels and original films in later analyses.

### Converting Review Scores
The public_response dataset includes various critic and audience ratings, such as Rotten Tomatoes scores, Metacritic scores, and CinemaScore letter grades. Since CinemaScore ratings are given as letters (e.g., "A+", "B-"), I converted them into a numeric scale for easier comparison:

```r
# Define a conversion table for CinemaScore grades
score_conversion <- c("A+" = 100, "A" = 95, "A-" = 90, 
                      "B+" = 85, "B" = 80, "B-" = 75, 
                      "C+" = 70, "C" = 65, "C-" = 60, 
                      "D+" = 55, "D" = 50, "D-" = 45, 
                      "F" = 0)

# Convert letter grades to numeric scores and handle missing values
public_response <- public_response_raw %>%
  mutate(cinema_score = score_conversion[cinema_score]) %>%
  mutate(across(c(rotten_tomatoes, metacritic, cinema_score, critics_choice), ~replace_na(.x, 0)))
```
By converting these letter grades, I ensured they could be used in statistical models without issues.

### Creating an Interaction Term
To explore whether a film’s **budget** influences **box office performance** differently for **sequels** versus **original films**, I created an **interaction term** between `budget` and `is_sequel`. This allows me to analyze whether the relationship between **budget and revenue** shifts depending on whether a film is a sequel.  

First, I added an **is_sequel** column, encoding sequels as `1` and original films as `0`. Then, I created the **interaction term** by multiplying `budget` by `is_sequel`. Finally, I combined my datasets using **left_join()** to ensure all relevant information was included for analysis.  

```r
# Add is_sequel column to box office data and create an interaction term
box_office_data <- box_office_raw %>%
  mutate(is_sequel = ifelse(film %in% sequel_titles, 1, 0)) %>%
  relocate(is_sequel, .before = budget) %>%
  mutate(budget_sequel_interaction = budget * is_sequel)

#Join box_office_data with pixar_films and public response
full_pixar_data <- box_office_data %>%
  left_join(pixar_films, by = "film") %>%
  left_join(public_response, by = "film")
```
Let's take a look at how the table looks now, using head(pixar_films):
```r
#A tibble: 6 × 5
    film               release_date       run_time film_rating is_sequel
    <chr>               <date>            <dbl>   <chr>         <dbl>
   "Toy Story"    	1995-11-22	81	G	    0
   "A Bug's Life"	1998-11-25	95	G	    0
   "Toy Story 2"	        1999-11-24	92	G	    1
   "Monsters, Inc."	2001-11-02	92	G	    0
   "Finding Nemo"	2003-05-30	100	G	    0
   "The Incredibles"	2004-11-05	115	PG	    0

```
### Visualizing the Data
The raw dataset was in a wide format, with each year as a separate column. To make it easier to visualize, I transformed it into a long format using **pivot_longer()**, which consolidates the year columns into a single **year** variable:
```r
tidy_data <- data %>%
  pivot_longer(cols = starts_with("19") | starts_with("20"), 
               names_to = "year", 
               values_to = "value") %>%
  mutate(year = as.integer(year))
```
### Creating the Heatmap
With the data in the right format, I used ggplot2 to generate a heatmap. Each tile represents a Pixar movie's rating from a specific critic, with colors indicating the score—**red for lower ratings** and **green for higher ratings**.
```r
# Heatmap for critics score
ggplot(critics_long, aes(x = critic, y = film, fill = score)) +
  geom_tile(color = "white") +
  scale_fill_gradient(low = "red", high = "green") +
  labs(title = "Pixar Movie Critic Score Heatmap",
       x = "Critic", y = "Movie", fill = "Score")
```
# Final Visualization
Here is the resulting heatmap!
![Pixar Movie Critic Score Heatmap](/assets/static/pixar_heatmap.png)

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