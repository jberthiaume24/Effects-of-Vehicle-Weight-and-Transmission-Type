# Linear Regression Analysis of Gas Mileage

## Overview

This analysis explores how vehicle weight and transmission type affect gasoline mileage using linear regression. It includes:
- Building and evaluating linear regression models.
- Testing the significance of transmission type.
- Investigating the interaction between vehicle weight and transmission type.
- Testing the interaction effect.

## Requirements

To run this analysis, you need:
- **R**: A programming language for statistical computing.
- **RStudio**: An integrated development environment (IDE) for R.
- **R Markdown**: A framework for creating dynamic documents with R.

## Setup Instructions

### Installing R

1. **Download R** from the Comprehensive R Archive Network (CRAN): [CRAN R Project](https://cran.r-project.org/)
2. **Install R** by following the instructions for your operating system.

### Installing RStudio

1. **Download RStudio** from the RStudio website: [RStudio Downloads](https://www.rstudio.com/products/rstudio/download/)
2. **Install RStudio** by following the setup instructions.

### Installing Required R Packages

In RStudio, install necessary packages using the following commands:

```r
install.packages("knitr")
install.packages("ggplot2")  # If you need plotting capabilities
```

### Setting Up the R Markdown File

1. **Create a new R Markdown document** in RStudio by selecting File > New File > R Markdown.
2. **Copy and paste** the following content into the R Markdown (.Rmd) file:

```markdown
---
title: "Linear Regression Analysis of Gas Mileage: Effects of Vehicle Weight and Transmission Type"
author: "Joshua Berthiaume"
date: "2024-08-24"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

# Introduction

This analysis examines the factors affecting gasoline mileage in vehicles using two key predictors: vehicle weight and type of transmission. We will first build a linear regression model to understand the relationship between mileage, weight, and transmission type. Subsequently, we will explore whether including an interaction term between vehicle weight and transmission type enhances the model. Finally, we will test the significance of this interaction to determine if it meaningfully affects mileage.

# 1.0 Linear Regression Model Relating Gas Mileage to Vehicle Weight and Transmission Type

We start by fitting a linear regression model where gasoline mileage (y) is predicted by vehicle weight (x10) and type of transmission (x11).

```{r}
# Load the dataset
auto = read.csv("data2.csv")

# Display the first few rows of the dataset
head(auto)

# Fit the linear regression model
model = lm(y ~ x10 + x11, data = auto)

# Display the summary of the model
summary(model)
```

The fitted model is given by the equation: ŷ = 38.62 - 0.0044x10 - 3.55x11. This equation indicates how gasoline mileage is influenced by vehicle weight and transmission type.

# 2.0 Assessing the Significance of Transmission Type

```{r}
# Fit the reduced model without the transmission variable
model.2 = lm(y ~ x10, data = auto)

# Perform ANOVA to compare the full and reduced models
anova(model.2, model)

# Calculate the critical F-value for hypothesis testing
alpha = 0.05
critical_f = qf(1 - alpha, 1, 25)
critical_f
```

Hypothesis test:

    H0: β2=0 (i.e., transmission type does not affect mileage)  
    Ha: β2≠0

Test Statistic:

    Fobs = (27.158/1) / (254.88/25) = 2.66,  
    
Rejection region:

    F > 4.24
    
Conclusion:

    Since the observed F-statistic (Fobs = 2.66) is less than the critical F-value (4.24), we do not reject H0. 
    Therefore, the type of transmission does not have a significant effect on gasoline mileage in this model.

# 3.0 Modifying the Model to Include an Interaction Term

To investigate potential interactions between vehicle weight and transmission type, we extend the model to include an interaction term.

```{r}
model.1 = lm(y~x10 + x11 + x10*x11, data = auto)
summary(model.1)

model.2 = lm(y ~ x10, data = auto)
anova(model.2,model.1)

alpha = .05
qf(1-alpha,2,24)
```

Hypothesis test:

    H0: β2=β3=0  (i.e. no interaction effect)
    Ha: not H0
    
Test statistics:

    Fobs = (122.55/2) / (159.48/24) = 9.22,  
    
Rejection region:

    F > 3.40

Conclusion:

    Since the observed F-statistic (Fobs = 9.22) is greater than the critical F-value (3.40), we reject H0. 
    This suggests that the type of transmission does significantly affect mileage performance when considering the interaction with vehicle weight.

# 4.0 Testing the Interaction Effect

We further isolate the significance of the interaction effect by comparing models with and without the interaction term.

```{r}
# Fit the model with the interaction term
model.1 = lm(y ~ x10 + x11 + x10*x11, data = auto)

# Fit the model without the interaction term
model.2 = lm(y ~ x10 + x11, data = auto)

# Perform ANOVA to test the interaction effect
anova(model.2, model.1)

# Calculate the critical F-value for hypothesis testing
alpha = 0.05
critical_f = qf(1 - alpha, 1, 24)
critical_f
```

Hypothesis test:

    H0: β3=0 (i.e., no interaction effect) 
    Ha: β3≠0
    
Test statistics:

    Fobs = (95.396/1) / (159.48/24) = 14.356,  
    
Rejection region:

    F > 4.26
    
Conclusion:

    Since the observed F-statistic (Fobs = 14.356) is greater than the critical F-value (4.26), we reject H0. This indicates a significant interaction effect between vehicle weight and type of transmission on gasoline mileage. The presence of an interaction suggests that the impact of transmission type on mileage depends on vehicle weight, leading to different slopes for different transmission types.

# Conclusion

In this analysis, we developed and evaluated models to understand how vehicle weight and transmission type affect gasoline mileage. Initially, we found that transmission type alone does not significantly affect mileage. However, upon including an interaction term, we observed a significant interaction effect, indicating that the relationship between vehicle weight and mileage varies depending on the type of transmission. This interaction highlights the complexity of the factors influencing gasoline mileage and underscores the importance of considering such interactions in regression models.

## Data
Ensure that the dataset `data2.csv` is available in your working directory. The dataset should contain columns:

```
- y: Gasoline mileage
- x10: Vehicle weight
- x11: Transmission type
```
## Running the Analysis

1. **Open RStudio** and **open** the R Markdown file (`.Rmd`).
2. **Click** the "Knit" button to generate the HTML report. This will execute the R code chunks and produce a detailed report of the analysis.