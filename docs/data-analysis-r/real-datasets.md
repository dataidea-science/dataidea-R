# Working with Real Datasets

This tutorial demonstrates a complete data analysis workflow using real-world datasets. We'll apply all the concepts learned in previous tutorials.

## Built-in Datasets

R comes with many built-in datasets perfect for practice:

```r
# List available datasets
data()

# Load a dataset
data(mtcars)
data(iris)
data(airquality)
data(USArrests)
```

## Case Study 1: Analyzing Car Data (mtcars)

### Step 1: Load and Explore

```r
library(dplyr)
library(ggplot2)

# Load data
data(mtcars)

# Initial exploration
head(mtcars)
str(mtcars)
summary(mtcars)
dim(mtcars)
```

### Step 2: Data Cleaning

```r
# Check for missing values
sum(is.na(mtcars))
colSums(is.na(mtcars))

# Convert row names to column
mtcars <- mtcars %>%
  rownames_to_column(var = "car_name")

# Create categorical variables
mtcars <- mtcars %>%
  mutate(
    cyl_factor = factor(cyl),
    gear_factor = factor(gear),
    am_factor = factor(am, labels = c("Automatic", "Manual"))
  )
```

### Step 3: Exploratory Data Analysis

```r
# Univariate analysis
ggplot(mtcars, aes(x = mpg)) +
  geom_histogram(bins = 20, fill = "steelblue") +
  labs(title = "Distribution of MPG")

# Bivariate analysis
ggplot(mtcars, aes(x = hp, y = mpg)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(title = "Horsepower vs MPG")

# Group comparisons
mtcars %>%
  group_by(am_factor) %>%
  summarize(
    mean_mpg = mean(mpg),
    median_mpg = median(mpg),
    sd_mpg = sd(mpg)
  )

ggplot(mtcars, aes(x = am_factor, y = mpg)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "MPG by Transmission Type")
```

### Step 4: Statistical Analysis

```r
# Correlation analysis
cor(mtcars$mpg, mtcars$hp)
cor.test(mtcars$mpg, mtcars$hp)

# T-test: MPG by transmission type
t.test(mpg ~ am, data = mtcars)

# Regression analysis
model <- lm(mpg ~ hp + wt + cyl, data = mtcars)
summary(model)
```

## Case Study 2: Iris Flower Dataset

### Complete Analysis Workflow

```r
library(dplyr)
library(ggplot2)
library(corrplot)

# Load data
data(iris)

# Explore structure
str(iris)
summary(iris)

# Visualize distributions
ggplot(iris, aes(x = Sepal.Length, fill = Species)) +
  geom_density(alpha = 0.7) +
  labs(title = "Sepal Length Distribution by Species")

# Pair plot
library(GGally)
ggpairs(iris, aes(color = Species))

# Statistical tests
# ANOVA: Sepal length by species
aov_result <- aov(Sepal.Length ~ Species, data = iris)
summary(aov_result)
TukeyHSD(aov_result)

# Correlation matrix
numeric_iris <- iris[, 1:4]
cor_matrix <- cor(numeric_iris)
corrplot(cor_matrix, method = "circle")
```

## Case Study 3: Air Quality Data

### Time Series Analysis

```r
library(dplyr)
library(ggplot2)
library(lubridate)

# Load data
data(airquality)

# Explore
head(airquality)
summary(airquality)

# Handle missing values
airquality_clean <- airquality %>%
  filter(!is.na(Ozone), !is.na(Solar.R))

# Create date variable
airquality_clean <- airquality_clean %>%
  mutate(
    date = ymd(paste("1973", Month, Day, sep = "-"))
  )

# Time series plot
ggplot(airquality_clean, aes(x = date, y = Ozone)) +
  geom_line() +
  geom_point() +
  labs(title = "Ozone Levels Over Time")

# Monthly patterns
airquality_clean %>%
  group_by(Month) %>%
  summarize(mean_ozone = mean(Ozone, na.rm = TRUE)) %>%
  ggplot(aes(x = Month, y = mean_ozone)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  labs(title = "Average Ozone by Month")
```

## Case Study 4: Custom Dataset Analysis

### Complete Workflow Example

```r
library(dplyr)
library(ggplot2)
library(readr)

# 1. Import data
data <- read_csv("sales_data.csv")

# 2. Initial exploration
dim(data)
str(data)
summary(data)
head(data)

# 3. Data cleaning
clean_data <- data %>%
  # Remove duplicates
  distinct() %>%
  
  # Handle missing values
  mutate(
    sales = ifelse(is.na(sales), median(sales, na.rm = TRUE), sales),
    region = ifelse(is.na(region), "Unknown", region)
  ) %>%
  
  # Remove outliers
  filter(sales > 0 & sales < 1000000) %>%
  
  # Create new variables
  mutate(
    month = month(date),
    year = year(date),
    sales_category = case_when(
      sales < 1000 ~ "Low",
      sales < 5000 ~ "Medium",
      TRUE ~ "High"
    )
  )

# 4. Exploratory analysis
# Summary statistics
clean_data %>%
  group_by(region) %>%
  summarize(
    total_sales = sum(sales),
    mean_sales = mean(sales),
    median_sales = median(sales),
    count = n()
  )

# Visualizations
ggplot(clean_data, aes(x = region, y = sales)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "Sales by Region")

ggplot(clean_data, aes(x = month, y = sales)) +
  geom_line(stat = "summary", fun = "mean") +
  labs(title = "Average Sales by Month")

# 5. Statistical analysis
# Compare regions
aov_result <- aov(sales ~ region, data = clean_data)
summary(aov_result)

# Correlation
cor(clean_data$sales, clean_data$marketing_spend)

# Regression
model <- lm(sales ~ marketing_spend + region, data = clean_data)
summary(model)

# 6. Create final report
# Summary table
summary_table <- clean_data %>%
  group_by(region, sales_category) %>%
  summarize(
    count = n(),
    total_sales = sum(sales),
    mean_sales = mean(sales)
  )

# Key visualizations
p1 <- ggplot(clean_data, aes(x = region, y = sales)) +
  geom_boxplot() +
  labs(title = "Sales Distribution by Region")

p2 <- ggplot(clean_data, aes(x = month, y = sales, color = region)) +
  geom_line(stat = "summary", fun = "mean") +
  labs(title = "Monthly Sales Trends by Region")

# Save outputs
write_csv(summary_table, "summary_table.csv")
ggsave("sales_by_region.png", p1, width = 10, height = 6)
ggsave("monthly_trends.png", p2, width = 10, height = 6)
```

## Best Practices for Real Data Analysis

1. **Start with exploration**: Understand your data before analysis
2. **Document everything**: Keep notes on decisions and findings
3. **Validate assumptions**: Check assumptions for statistical tests
4. **Iterate**: Refine your analysis based on findings
5. **Visualize first**: Use plots to understand patterns
6. **Be skeptical**: Question unexpected results
7. **Reproducibility**: Use scripts, not interactive sessions
8. **Version control**: Track changes to your analysis

## Common Challenges

### Missing Data
```r
# Strategy: Multiple imputation
library(mice)
imputed_data <- mice(data, m = 5, method = "pmm")
complete_data <- complete(imputed_data)
```

### Large Datasets
```r
# Use data.table for speed
library(data.table)
dt <- as.data.table(data)
result <- dt[, .(mean_value = mean(value)), by = category]
```

### Complex Relationships
```r
# Use advanced modeling
library(randomForest)
model <- randomForest(outcome ~ ., data = data)
```

## Creating Reproducible Reports

### Using R Markdown

```r
# Create an R Markdown document
# ---
# title: "Data Analysis Report"
# output: html_document
# ---
# 
# ```{r setup, include=FALSE}
# knitr::opts_chunk$set(echo = TRUE)
# ```
# 
# ## Introduction
# 
# This report analyzes...
# 
# ```{r}
# library(dplyr)
# data <- read_csv("data.csv")
# ```
```

## Next Steps

You've now completed the data analysis tutorials! Continue practicing with:
- Different types of datasets
- More complex analyses
- Advanced visualization techniques
- Machine learning models
- Creating reproducible reports

Remember: The best way to learn is by doing. Find datasets that interest you and practice applying these techniques!
