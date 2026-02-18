# Data Cleaning and Transformation

Data cleaning is one of the most important steps in data analysis. Real-world data is often messy, incomplete, or inconsistent. This tutorial covers techniques to clean and transform your data.

## Understanding Your Data

### Initial Inspection
```r
library(dplyr)

# Load data
data <- read.csv("data.csv")

# Basic information
dim(data)           # Dimensions
names(data)         # Column names
str(data)           # Structure
summary(data)       # Summary statistics
head(data)          # First few rows
tail(data)          # Last few rows

# Check for missing values
sum(is.na(data))
colSums(is.na(data))
```

## Handling Missing Values

### Identifying Missing Values
```r
# Check for NA
is.na(data)
sum(is.na(data))

# Check by column
colSums(is.na(data))
sapply(data, function(x) sum(is.na(x)))

# Visualize missing data
library(VIM)
aggr(data, col = c('navyblue', 'red'), numbers = TRUE)
```

### Strategies for Missing Values

#### Remove Missing Values
```r
# Remove rows with any missing values
data_clean <- na.omit(data)

# Remove rows where specific columns are missing
data_clean <- data[!is.na(data$important_column), ]

# Using dplyr
data_clean <- data %>%
  filter(!is.na(important_column))
```

#### Impute Missing Values
```r
# Mean imputation
data$column[is.na(data$column)] <- mean(data$column, na.rm = TRUE)

# Median imputation
data$column[is.na(data$column)] <- median(data$column, na.rm = TRUE)

# Mode imputation (for categorical)
mode_value <- names(sort(table(data$column), decreasing = TRUE))[1]
data$column[is.na(data$column)] <- mode_value

# Forward fill (for time series)
library(zoo)
data$column <- na.locf(data$column)
```

## Handling Duplicates

```r
# Check for duplicates
duplicated(data)
sum(duplicated(data))

# Remove duplicates
data_unique <- unique(data)
data_unique <- distinct(data)

# Remove duplicates based on specific columns
data_unique <- data %>%
  distinct(column1, column2, .keep_all = TRUE)

# Keep first occurrence
data_unique <- data[!duplicated(data), ]
```

## Data Type Conversions

```r
# Convert to numeric
data$column <- as.numeric(data$column)

# Convert to character
data$column <- as.character(data$column)

# Convert to factor
data$column <- as.factor(data$column)

# Convert to date
library(lubridate)
data$date <- as.Date(data$date)
data$date <- ymd(data$date)  # Year-month-day
data$date <- mdy(data$date)  # Month-day-year
```

## Handling Outliers

### Detecting Outliers
```r
# Using IQR method
Q1 <- quantile(data$column, 0.25, na.rm = TRUE)
Q3 <- quantile(data$column, 0.75, na.rm = TRUE)
IQR <- Q3 - Q1
lower_bound <- Q1 - 1.5 * IQR
upper_bound <- Q3 + 1.5 * IQR

outliers <- data$column < lower_bound | data$column > upper_bound

# Using z-scores
z_scores <- abs(scale(data$column))
outliers <- z_scores > 3

# Visualize outliers
boxplot(data$column)
```

### Handling Outliers
```r
# Remove outliers
data_clean <- data[!outliers, ]

# Cap outliers
data$column[data$column > upper_bound] <- upper_bound
data$column[data$column < lower_bound] <- lower_bound

# Transform (log transformation)
data$column <- log(data$column + 1)
```

## String Cleaning

```r
library(stringr)

# Remove whitespace
data$column <- str_trim(data$column)
data$column <- str_squish(data$column)  # Remove extra spaces

# Convert case
data$column <- str_to_lower(data$column)
data$column <- str_to_upper(data$column)
data$column <- str_to_title(data$column)

# Remove special characters
data$column <- str_replace_all(data$column, "[^A-Za-z0-9 ]", "")

# Extract numbers
data$numbers <- str_extract(data$column, "\\d+")

# Replace patterns
data$column <- str_replace(data$column, "old", "new")
```

## Data Transformation with dplyr

### Selecting Columns
```r
library(dplyr)

# Select specific columns
data <- data %>%
  select(column1, column2, column3)

# Select columns by pattern
data <- data %>%
  select(starts_with("prefix"))
  select(ends_with("suffix"))
  select(contains("pattern"))
```

### Filtering Rows
```r
# Filter by condition
data <- data %>%
  filter(column > 100)
  filter(column %in% c("A", "B", "C"))
  filter(column > 50 & column2 < 200)
  filter(is.na(column) | column != "")
```

### Creating New Variables
```r
# Create new column
data <- data %>%
  mutate(
    new_column = column1 + column2,
    ratio = column1 / column2,
    category = ifelse(column > 100, "High", "Low")
  )

# Conditional transformation
data <- data %>%
  mutate(
    status = case_when(
      score >= 90 ~ "Excellent",
      score >= 80 ~ "Good",
      score >= 70 ~ "Fair",
      TRUE ~ "Poor"
    )
  )
```

### Grouping and Summarizing
```r
# Group by and summarize
summary <- data %>%
  group_by(category) %>%
  summarize(
    count = n(),
    mean_value = mean(value, na.rm = TRUE),
    median_value = median(value, na.rm = TRUE),
    sd_value = sd(value, na.rm = TRUE)
  )
```

## Reshaping Data with tidyr

```r
library(tidyr)

# Wide to long
data_long <- data %>%
  pivot_longer(
    cols = starts_with("value"),
    names_to = "variable",
    values_to = "value"
  )

# Long to wide
data_wide <- data_long %>%
  pivot_wider(
    names_from = variable,
    values_from = value
  )

# Separate columns
data <- data %>%
  separate(
    combined_column,
    into = c("col1", "col2"),
    sep = "-"
  )

# Unite columns
data <- data %>%
  unite(
    new_column,
    col1, col2,
    sep = "-"
  )
```

## Date and Time Manipulation

```r
library(lubridate)

# Parse dates
data$date <- ymd(data$date_string)
data$datetime <- ymd_hms(data$datetime_string)

# Extract components
data$year <- year(data$date)
data$month <- month(data$date)
data$day <- day(data$date)
data$weekday <- wday(data$date, label = TRUE)

# Date arithmetic
data$days_since <- as.numeric(Sys.Date() - data$date)
data$next_month <- data$date + months(1)
```

## Example: Complete Cleaning Workflow

```r
library(dplyr)
library(stringr)
library(lubridate)

# Load data
data <- read.csv("messy_data.csv")

# Clean data
clean_data <- data %>%
  # Remove duplicates
  distinct() %>%
  
  # Clean strings
  mutate(
    name = str_trim(str_to_title(name)),
    email = str_to_lower(email)
  ) %>%
  
  # Handle missing values
  mutate(
    age = ifelse(is.na(age), median(age, na.rm = TRUE), age),
    salary = ifelse(is.na(salary), mean(salary, na.rm = TRUE), salary)
  ) %>%
  
  # Remove outliers
  filter(age >= 18 & age <= 100) %>%
  filter(salary > 0 & salary < 1000000) %>%
  
  # Create new variables
  mutate(
    age_group = case_when(
      age < 30 ~ "Young",
      age < 50 ~ "Middle",
      TRUE ~ "Senior"
    ),
    salary_category = ifelse(salary > median(salary), "High", "Low")
  ) %>%
  
  # Convert dates
  mutate(
    start_date = mdy(start_date),
    years_employed = as.numeric(Sys.Date() - start_date) / 365
  )

# Save cleaned data
write.csv(clean_data, "clean_data.csv", row.names = FALSE)
```

## Best Practices

1. **Don't modify original data**: Work with copies
2. **Document transformations**: Keep track of what you changed
3. **Check after each step**: Verify transformations worked
4. **Handle edge cases**: Consider what happens with empty data
5. **Preserve data lineage**: Know how data was transformed
6. **Test on sample**: Try transformations on small subset first

## Next Steps

Learn about [Exploratory Data Analysis](exploratory-analysis.md) to understand your cleaned data better.
