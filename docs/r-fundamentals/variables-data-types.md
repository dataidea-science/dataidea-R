# Variables and Data Types

## Variables in R

Variables store data values. In R, you can assign values using `<-` or `=`:

```r
# Using <-
x <- 10
name <- "John"

# Using =
y = 20
city = "New York"
```

**Note**: The `<-` operator is preferred in R as it's more idiomatic.

## Basic Data Types

R has several fundamental data types:

### Numeric
```r
# Integer and floating-point numbers
age <- 25
height <- 5.9
temperature <- -10.5

# Check type
class(age)
typeof(age)
```

### Character (Strings)
```r
# Text data
name <- "Alice"
greeting <- 'Hello'
sentence <- "R is great for data analysis"

# Check type
class(name)
```

### Logical (Boolean)
```r
# TRUE or FALSE
is_student <- TRUE
is_employed <- FALSE

# Logical operations
result <- 5 > 3  # TRUE
result2 <- 2 == 3  # FALSE

# Check type
class(is_student)
```

### Complex
```r
# Complex numbers
z <- 3 + 4i
class(z)
```

### Raw
```r
# Raw bytes
raw_data <- charToRaw("Hello")
class(raw_data)
```

## Type Checking and Conversion

```r
# Check if a variable is of a specific type
is.numeric(age)
is.character(name)
is.logical(is_student)

# Convert between types
x <- "123"
x_numeric <- as.numeric(x)
x_integer <- as.integer(x_numeric)
x_character <- as.character(x_numeric)
```

## Special Values

R has special values that represent missing or undefined data:

```r
# NA - Not Available (missing value)
missing_value <- NA
is.na(missing_value)

# NULL - Empty object
empty <- NULL
is.null(empty)

# NaN - Not a Number
not_a_number <- 0/0
is.nan(not_a_number)

# Inf - Infinity
infinity <- 1/0
is.infinite(infinity)
```

## Variable Naming Rules

- Must start with a letter or dot
- Can contain letters, numbers, dots, and underscores
- Case-sensitive
- Avoid reserved words (if, else, for, while, etc.)

```r
# Valid names
my_variable <- 10
myVariable <- 20
my.variable <- 30
.variable <- 40

# Invalid names
# 2variable <- 10  # Error: starts with number
# my-variable <- 10  # Error: contains hyphen
```

## Best Practices

1. Use descriptive names: `student_age` instead of `x`
2. Be consistent with naming conventions
3. Use `<-` for assignment (R convention)
4. Check data types when importing data
5. Handle missing values appropriately

## Next Steps

Learn about [Vectors and Basic Operations](vectors-operations.md) to work with collections of data.
