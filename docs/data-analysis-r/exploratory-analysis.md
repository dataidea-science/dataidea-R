# Exploratory Data Analysis

Exploratory Data Analysis (EDA) is the process of investigating data to understand its main characteristics, discover patterns, spot anomalies, and check assumptions. It's typically done before formal modeling.

## Goals of EDA

1. **Understand the data**: Structure, types, distributions
2. **Detect patterns**: Trends, relationships, clusters
3. **Identify anomalies**: Outliers, missing data, inconsistencies
4. **Generate hypotheses**: Form ideas for further analysis
5. **Check assumptions**: Validate assumptions for statistical tests

## Univariate Analysis

### Categorical Variables

```r
library(dplyr)
library(ggplot2)

# Frequency table
table(data$category)
prop.table(table(data$category))

# Using dplyr
data %>%
  count(category) %>%
  mutate(proportion = n / sum(n))

# Bar plot
ggplot(data, aes(x = category)) +
  geom_bar() +
  labs(title = "Distribution of Categories")

# Pie chart (less recommended)
ggplot(data, aes(x = "", fill = category)) +
  geom_bar(width = 1) +
  coord_polar("y")
```

### Numerical Variables

```r
# Summary statistics
summary(data$numeric_column)
mean(data$numeric_column, na.rm = TRUE)
median(data$numeric_column, na.rm = TRUE)
sd(data$numeric_column, na.rm = TRUE)
quantile(data$numeric_column, c(0.25, 0.5, 0.75))

# Histogram
ggplot(data, aes(x = numeric_column)) +
  geom_histogram(bins = 30, fill = "steelblue") +
  labs(title = "Distribution of Numeric Variable")

# Density plot
ggplot(data, aes(x = numeric_column)) +
  geom_density(fill = "steelblue", alpha = 0.7) +
  labs(title = "Density Plot")

# Box plot
ggplot(data, aes(y = numeric_column)) +
  geom_boxplot() +
  labs(title = "Box Plot")

# Violin plot
ggplot(data, aes(x = category, y = numeric_column)) +
  geom_violin() +
  labs(title = "Distribution by Category")
```

## Bivariate Analysis

### Categorical vs Categorical

```r
# Contingency table
table(data$category1, data$category2)
prop.table(table(data$category1, data$category2), margin = 1)

# Stacked bar chart
ggplot(data, aes(x = category1, fill = category2)) +
  geom_bar(position = "stack") +
  labs(title = "Stacked Bar Chart")

# Grouped bar chart
ggplot(data, aes(x = category1, fill = category2)) +
  geom_bar(position = "dodge") +
  labs(title = "Grouped Bar Chart")
```

### Numerical vs Numerical

```r
# Scatter plot
ggplot(data, aes(x = var1, y = var2)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm", se = TRUE) +
  labs(title = "Scatter Plot with Trend Line")

# Correlation
cor(data$var1, data$var2, use = "complete.obs")
cor.test(data$var1, data$var2)

# Correlation matrix
cor_matrix <- cor(data[, c("var1", "var2", "var3")], use = "complete.obs")
library(corrplot)
corrplot(cor_matrix, method = "circle")
```

### Categorical vs Numerical

```r
# Box plots by category
ggplot(data, aes(x = category, y = numeric_var)) +
  geom_boxplot() +
  labs(title = "Numeric Variable by Category")

# Violin plots
ggplot(data, aes(x = category, y = numeric_var)) +
  geom_violin() +
  labs(title = "Distribution by Category")

# Grouped statistics
data %>%
  group_by(category) %>%
  summarize(
    mean = mean(numeric_var, na.rm = TRUE),
    median = median(numeric_var, na.rm = TRUE),
    sd = sd(numeric_var, na.rm = TRUE)
  )
```

## Multivariate Analysis

### Pairwise Relationships

```r
# Pair plot
library(GGally)
ggpairs(data[, c("var1", "var2", "var3")])

# Correlation heatmap
library(corrplot)
cor_matrix <- cor(data[, sapply(data, is.numeric)], use = "complete.obs")
corrplot(cor_matrix, method = "color", type = "upper")
```

### Faceting

```r
# Multiple plots by category
ggplot(data, aes(x = var1, y = var2)) +
  geom_point() +
  facet_wrap(~ category) +
  labs(title = "Scatter Plots by Category")

# Grid of plots
ggplot(data, aes(x = var1, y = var2)) +
  geom_point() +
  facet_grid(category1 ~ category2)
```

## Advanced Visualizations

### Time Series

```r
library(lubridate)

# Time series plot
data$date <- ymd(data$date_string)
ggplot(data, aes(x = date, y = value)) +
  geom_line() +
  geom_point() +
  labs(title = "Time Series Plot") +
  theme_minimal()
```

### Distribution Comparisons

```r
# Overlapping distributions
ggplot(data, aes(x = value, fill = category)) +
  geom_density(alpha = 0.5) +
  labs(title = "Overlapping Distributions")

# Q-Q plots
qqnorm(data$numeric_var)
qqline(data$numeric_var)
```

## Statistical Tests

### Normality Tests

```r
# Shapiro-Wilk test
shapiro.test(data$numeric_var)

# Kolmogorov-Smirnov test
ks.test(data$numeric_var, "pnorm", mean = mean(data$numeric_var), sd = sd(data$numeric_var))
```

### Association Tests

```r
# Chi-square test
chisq.test(table(data$cat1, data$cat2))

# T-test
t.test(numeric_var ~ category, data = data)

# ANOVA
aov_result <- aov(numeric_var ~ category, data = data)
summary(aov_result)
```

## EDA Workflow Example

```r
library(dplyr)
library(ggplot2)
library(corrplot)

# Load data
data <- read.csv("data.csv")

# 1. Initial overview
dim(data)
str(data)
summary(data)
head(data)

# 2. Check missing values
colSums(is.na(data))

# 3. Univariate analysis
# Numeric variables
numeric_vars <- sapply(data, is.numeric)
summary(data[, numeric_vars])

# Histograms for all numeric variables
data[, numeric_vars] %>%
  gather(key = "variable", value = "value") %>%
  ggplot(aes(x = value)) +
  geom_histogram(bins = 30) +
  facet_wrap(~ variable, scales = "free")

# Categorical variables
categorical_vars <- sapply(data, is.factor) | sapply(data, is.character)
for (var in names(data)[categorical_vars]) {
  print(table(data[[var]]))
  print(ggplot(data, aes_string(x = var)) + geom_bar())
}

# 4. Bivariate analysis
# Correlation matrix
cor_matrix <- cor(data[, numeric_vars], use = "complete.obs")
corrplot(cor_matrix, method = "circle")

# Scatter plots for highly correlated variables
high_cor <- which(abs(cor_matrix) > 0.7 & abs(cor_matrix) < 1, arr.ind = TRUE)
# Plot relationships

# 5. Group comparisons
data %>%
  group_by(category) %>%
  summarize(
    count = n(),
    mean_value = mean(numeric_var, na.rm = TRUE),
    sd_value = sd(numeric_var, na.rm = TRUE)
  )

# 6. Identify outliers
boxplot(data[, numeric_vars])
```

## Best Practices

1. **Start simple**: Begin with basic summaries and plots
2. **Look for patterns**: Trends, clusters, relationships
3. **Check assumptions**: Normality, linearity, independence
4. **Document findings**: Keep notes on what you discover
5. **Iterate**: EDA is an iterative process
6. **Use multiple views**: Different visualizations reveal different insights
7. **Question everything**: Don't take data at face value

## Common EDA Questions

- What is the distribution of each variable?
- Are there any outliers or anomalies?
- What are the relationships between variables?
- Are there any patterns or trends?
- What assumptions can we make?
- What further analysis is needed?

## Next Steps

After exploring your data, proceed to [Statistical Analysis](statistical-analysis.md) for more formal analysis techniques.
