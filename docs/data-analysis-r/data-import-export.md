# Data Import and Export

Importing and exporting data is the first and last step in most data analysis workflows. R provides many ways to read and write data in various formats.

## Reading CSV Files

### Using read.csv() (base R)
```r
# Basic usage
data <- read.csv("data.csv")

# With options
data <- read.csv(
  "data.csv",
  header = TRUE,           # First row contains column names
  sep = ",",               # Separator
  quote = "\"",            # Quote character
  stringsAsFactors = FALSE # Don't convert strings to factors
)

# Specify column types
data <- read.csv(
  "data.csv",
  colClasses = c("character", "numeric", "Date", "logical")
)
```

### Using readr Package
```r
library(readr)

# Faster and more consistent
data <- read_csv("data.csv")

# Specify column types
data <- read_csv(
  "data.csv",
  col_types = cols(
    name = col_character(),
    age = col_integer(),
    salary = col_double(),
    date = col_date()
  )
)

# Read with locale settings
data <- read_csv("data.csv", locale = locale(decimal_mark = ","))
```

## Reading Excel Files

```r
library(readxl)

# Read first sheet
data <- read_excel("data.xlsx")

# Read specific sheet
data <- read_excel("data.xlsx", sheet = "Sheet2")
data <- read_excel("data.xlsx", sheet = 2)

# Read specific range
data <- read_excel("data.xlsx", range = "A1:D100")

# List all sheets
excel_sheets("data.xlsx")
```

## Reading JSON Files

```r
library(jsonlite)

# Read JSON file
data <- fromJSON("data.json")

# Read JSON from URL
data <- fromJSON("https://api.example.com/data")
```

## Reading from Databases

### SQLite
```r
library(DBI)
library(RSQLite)

# Connect to database
con <- dbConnect(RSQLite::SQLite(), "database.db")

# Read table
data <- dbReadTable(con, "table_name")

# Execute query
data <- dbGetQuery(con, "SELECT * FROM table WHERE condition")

# Close connection
dbDisconnect(con)
```

### PostgreSQL/MySQL
```r
library(DBI)
library(RPostgreSQL)  # or RMySQL

con <- dbConnect(
  PostgreSQL(),
  host = "localhost",
  dbname = "mydb",
  user = "username",
  password = "password"
)

data <- dbGetQuery(con, "SELECT * FROM table")
dbDisconnect(con)
```

## Reading Web Data

```r
# Read CSV from URL
data <- read.csv("https://example.com/data.csv")

# Using readr
library(readr)
data <- read_csv("https://example.com/data.csv")

# Using httr for APIs
library(httr)
response <- GET("https://api.example.com/data")
data <- content(response, "parsed")
```

## Reading Other Formats

### SPSS, SAS, Stata
```r
library(haven)

# SPSS
data <- read_sav("data.sav")

# SAS
data <- read_sas("data.sas7bdat")

# Stata
data <- read_dta("data.dta")
```

### Fixed Width Files
```r
# Base R
data <- read.fwf("data.txt", widths = c(5, 10, 8))

# Using readr
library(readr)
data <- read_fwf("data.txt", fwf_widths(c(5, 10, 8)))
```

## Writing Data

### CSV Files
```r
# Base R
write.csv(data, "output.csv", row.names = FALSE)

# Using readr (faster)
library(readr)
write_csv(data, "output.csv")
```

### Excel Files
```r
library(writexl)

# Write to Excel
write_xlsx(data, "output.xlsx")

# Write multiple sheets
write_xlsx(
  list(Sheet1 = data1, Sheet2 = data2),
  "output.xlsx"
)
```

### JSON Files
```r
library(jsonlite)

# Write JSON
write_json(data, "output.json", pretty = TRUE)
```

### Other Formats
```r
library(haven)

# Write SPSS
write_sav(data, "output.sav")

# Write Stata
write_dta(data, "output.dta")
```

## Handling Common Issues

### Missing Values
```r
# Check for missing values
is.na(data)
sum(is.na(data))
colSums(is.na(data))

# Read with NA strings
data <- read.csv("data.csv", na.strings = c("", "NA", "NULL", "-"))
```

### Encoding Issues
```r
# Specify encoding
data <- read.csv("data.csv", fileEncoding = "UTF-8")
data <- read_csv("data.csv", locale = locale(encoding = "UTF-8"))
```

### Large Files
```r
# Read in chunks
library(readr)

# Read first 1000 rows
data <- read_csv("large_file.csv", n_max = 1000)

# Using data.table for large files
library(data.table)
data <- fread("large_file.csv")
```

## Best Practices

1. **Check data after import**: Use `head()`, `str()`, `summary()`
2. **Specify column types**: Prevents type inference errors
3. **Handle encoding**: Be aware of character encoding issues
4. **Save processed data**: Don't re-import raw data repeatedly
5. **Use relative paths**: Make your code portable
6. **Document data sources**: Keep track of where data came from

## Example: Complete Import Workflow

```r
library(readr)
library(dplyr)

# Read data
raw_data <- read_csv(
  "data/sales.csv",
  col_types = cols(
    date = col_date(format = "%Y-%m-%d"),
    product_id = col_character(),
    quantity = col_integer(),
    price = col_double()
  ),
  na = c("", "NA", "NULL")
)

# Inspect data
head(raw_data)
str(raw_data)
summary(raw_data)

# Check for issues
sum(is.na(raw_data))
any(duplicated(raw_data))

# Save processed data
write_csv(raw_data, "data/processed/sales_clean.csv")
```

## Next Steps

Learn about [Data Cleaning and Transformation](data-cleaning.md) to prepare your data for analysis.
