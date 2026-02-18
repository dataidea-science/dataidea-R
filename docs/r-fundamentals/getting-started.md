# Getting Started with R

## What is R?

R is a programming language and environment specifically designed for statistical computing and graphics. It's widely used by data scientists, statisticians, and researchers for data analysis, visualization, and statistical modeling.

## Installing R

### Windows
1. Download R from [CRAN](https://cran.r-project.org/bin/windows/base/)
2. Run the installer and follow the setup wizard
3. Optionally install RStudio for a better development experience

### macOS
1. Download R from [CRAN](https://cran.r-project.org/bin/macosx/)
2. Open the downloaded `.pkg` file and follow the installation instructions
3. Install RStudio from [rstudio.com](https://www.rstudio.com/products/rstudio/download/)

### Linux
```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install r-base

# Fedora
sudo dnf install R

# CentOS/RHEL
sudo yum install R
```

## Your First R Session

Open R or RStudio and try these commands:

```r
# Print "Hello, World!"
print("Hello, World!")

# Simple arithmetic
2 + 2
10 * 5
sqrt(16)

# Get help
help(print)
?print
```

## RStudio Overview

RStudio is an integrated development environment (IDE) for R that provides:
- **Console**: Where you type and execute R commands
- **Script Editor**: For writing and saving R scripts
- **Environment**: Shows objects in your current session
- **Plots**: Displays graphs and visualizations
- **Files/Help**: File browser and help documentation

## Basic R Syntax

```r
# Comments start with #
# R is case-sensitive
# Variable assignment uses <- or =
x <- 10
y = 20

# Print values
x
print(x)

# Combine values
c(x, y)
```

## Next Steps

Now that you have R installed and understand the basics, proceed to [Variables and Data Types](variables-data-types.md).
