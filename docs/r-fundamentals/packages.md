# Working with Packages

Packages extend R's functionality by providing additional functions, data, and tools. Understanding how to work with packages is essential for data analysis.

## What are Packages?

Packages are collections of R functions, data, and compiled code organized in a well-defined structure. They can be:
- **Base packages**: Included with R installation
- **Recommended packages**: Installed by default but not loaded
- **Contributed packages**: Available from CRAN, Bioconductor, GitHub, etc.

## Installing Packages

### From CRAN
```r
# Install a single package
install.packages("ggplot2")

# Install multiple packages
install.packages(c("dplyr", "tidyr", "readr"))

# Install with dependencies
install.packages("ggplot2", dependencies = TRUE)
```

### From Bioconductor
```r
# Install BiocManager first
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Install Bioconductor packages
BiocManager::install("Biobase")
```

### From GitHub
```r
# Install devtools first
install.packages("devtools")

# Install from GitHub
devtools::install_github("username/repository")
```

## Loading Packages

```r
# Load a package
library(ggplot2)

# Load with namespace (recommended)
library(dplyr)

# Check if package is loaded
"ggplot2" %in% loadedNamespaces()
```

## Using Package Functions

### Direct Access
```r
library(dplyr)
filter(mtcars, mpg > 20)
```

### Namespace Access
```r
# Use without loading
dplyr::filter(mtcars, mpg > 20)

# Useful when multiple packages have same function name
stats::filter()  # Different from dplyr::filter()
```

## Essential Packages for Data Analysis

### Data Manipulation
```r
# dplyr - Data manipulation
library(dplyr)
mtcars %>%
  filter(mpg > 20) %>%
  select(mpg, hp, wt) %>%
  arrange(desc(mpg))

# tidyr - Data tidying
library(tidyr)
# pivot_longer(), pivot_wider(), separate(), unite()

# data.table - Fast data manipulation
library(data.table)
```

### Data Import/Export
```r
# readr - Read rectangular data
library(readr)
read_csv("data.csv")
read_tsv("data.tsv")

# readxl - Read Excel files
library(readxl)
read_excel("data.xlsx")

# haven - Read SAS, SPSS, Stata files
library(haven)
```

### Visualization
```r
# ggplot2 - Grammar of graphics
library(ggplot2)
ggplot(mtcars, aes(x = mpg, y = hp)) +
  geom_point()

# plotly - Interactive plots
library(plotly)

# lattice - Trellis graphics
library(lattice)
```

### Statistical Analysis
```r
# broom - Tidy statistical models
library(broom)

# car - Companion to Applied Regression
library(car)
```

## Managing Packages

### List Installed Packages
```r
# All installed packages
installed.packages()

# Check if specific package is installed
"ggplot2" %in% rownames(installed.packages())
```

### Update Packages
```r
# Update all packages
update.packages()

# Update specific package
update.packages("ggplot2")
```

### Remove Packages
```r
# Remove a package
remove.packages("package_name")
```

### Package Information
```r
# Package help
help(package = "ggplot2")
packageDescription("ggplot2")

# List functions in package
ls("package:ggplot2")
```

## Package Conflicts

When multiple packages have functions with the same name:

```r
# Check conflicts
conflicts(detail = TRUE)

# Use namespace to specify
dplyr::filter()  # dplyr's filter
stats::filter()  # stats' filter
```

## Creating Your Own Package

While advanced, you can create your own packages:

```r
# Using devtools
library(devtools)
create_package("my_package")
```

## Best Practices

1. **Load packages at the start**: Put `library()` calls at the beginning of your script
2. **Use namespace notation**: `package::function()` when there might be conflicts
3. **Document dependencies**: List required packages in your scripts
4. **Keep packages updated**: Regularly update packages for bug fixes
5. **Use renv for projects**: Manage package versions per project
6. **Check package versions**: Some functions change between versions

## Example: Complete Workflow

```r
# Load required packages
library(dplyr)
library(ggplot2)
library(readr)

# Read data
data <- read_csv("data.csv")

# Process data
processed <- data %>%
  filter(!is.na(value)) %>%
  group_by(category) %>%
  summarize(mean_value = mean(value))

# Visualize
ggplot(processed, aes(x = category, y = mean_value)) +
  geom_bar(stat = "identity") +
  labs(title = "Mean Values by Category")
```

## Package Repositories

- **CRAN**: Comprehensive R Archive Network (main repository)
- **Bioconductor**: Bioinformatics packages
- **GitHub**: Development versions and custom packages
- **R-Forge**: Development repository

## Next Steps

Now that you understand R fundamentals, proceed to [Data Analysis with R](../data-analysis-r/introduction.md) to apply these concepts to real data analysis tasks.
