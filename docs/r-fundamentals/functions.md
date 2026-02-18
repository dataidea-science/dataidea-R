# Functions

Functions are reusable blocks of code that perform specific tasks. They help organize code and avoid repetition.

## Built-in Functions

R comes with many built-in functions:

```r
# Mathematical functions
sqrt(16)
abs(-5)
round(3.14159, 2)
ceiling(3.2)
floor(3.8)

# Statistical functions
mean(c(1, 2, 3, 4, 5))
median(c(1, 2, 3, 4, 5))
sd(c(1, 2, 3, 4, 5))

# String functions
paste("Hello", "World")
paste0("Hello", "World")  # No separator
nchar("Hello")
toupper("hello")
tolower("HELLO")
```

## Creating Your Own Functions

### Basic Function Syntax
```r
# Simple function
greet <- function() {
  print("Hello, World!")
}

greet()
```

### Functions with Parameters
```r
# Function with one parameter
square <- function(x) {
  return(x^2)
}

square(5)  # 25

# Function with multiple parameters
add <- function(a, b) {
  return(a + b)
}

add(3, 5)  # 8
```

### Default Parameters
```r
# Function with default values
power <- function(x, exponent = 2) {
  return(x^exponent)
}

power(5)        # 25 (uses default exponent = 2)
power(5, 3)     # 125 (explicit exponent = 3)
```

### Named Arguments
```r
# Function with named parameters
create_person <- function(name, age, city = "Unknown") {
  return(list(name = name, age = age, city = city))
}

create_person("Alice", 30)
create_person(name = "Bob", age = 25, city = "NYC")
create_person(age = 28, name = "Charlie")  # Order doesn't matter
```

## Return Values

```r
# Explicit return
calculate <- function(x, y) {
  sum_result <- x + y
  product_result <- x * y
  return(list(sum = sum_result, product = product_result))
}

# Implicit return (last expression)
multiply <- function(x, y) {
  x * y  # Automatically returned
}

# Multiple return values
stats <- function(x) {
  c(mean = mean(x), median = median(x), sd = sd(x))
}
```

## Variable Scope

```r
# Global vs local variables
global_var <- 10

my_function <- function() {
  local_var <- 5
  print(global_var)  # Can access global
  print(local_var)   # Can access local
}

my_function()
# print(local_var)  # Error: local_var not found outside function
```

## Advanced Function Features

### Variable Arguments (...)
```r
# Function with variable number of arguments
sum_all <- function(...) {
  args <- list(...)
  return(sum(unlist(args)))
}

sum_all(1, 2, 3)
sum_all(1, 2, 3, 4, 5)
```

### Anonymous Functions
```r
# Functions without names
(function(x) x^2)(5)  # 25

# Useful with apply functions
sapply(1:5, function(x) x^2)
```

### Function as Arguments
```r
# Pass function as argument
apply_operation <- function(x, func) {
  return(func(x))
}

apply_operation(5, sqrt)
apply_operation(5, function(x) x^3)
```

## Practical Examples

### Statistical Function
```r
# Calculate z-scores
z_score <- function(x) {
  mean_x <- mean(x, na.rm = TRUE)
  sd_x <- sd(x, na.rm = TRUE)
  return((x - mean_x) / sd_x)
}

data <- c(10, 20, 30, 40, 50)
z_scores <- z_score(data)
```

### Data Processing Function
```r
# Clean and summarize data
summarize_data <- function(df, column) {
  if (!column %in% names(df)) {
    stop("Column not found")
  }
  
  data <- df[[column]]
  return(list(
    mean = mean(data, na.rm = TRUE),
    median = median(data, na.rm = TRUE),
    min = min(data, na.rm = TRUE),
    max = max(data, na.rm = TRUE),
    missing = sum(is.na(data))
  ))
}

df <- data.frame(x = 1:10, y = rnorm(10))
summarize_data(df, "x")
```

### Recursive Functions
```r
# Factorial function
factorial_recursive <- function(n) {
  if (n <= 1) {
    return(1)
  } else {
    return(n * factorial_recursive(n - 1))
  }
}

factorial_recursive(5)  # 120
```

## Error Handling

```r
# Function with error checking
safe_divide <- function(a, b) {
  if (b == 0) {
    stop("Division by zero is not allowed")
  }
  return(a / b)
}

safe_divide(10, 2)   # 5
safe_divide(10, 0)   # Error
```

## Documentation

```r
# Add comments to document functions
calculate_bmi <- function(weight_kg, height_m) {
  # Calculate Body Mass Index
  # Args:
  #   weight_kg: weight in kilograms
  #   height_m: height in meters
  # Returns:
  #   BMI value
  bmi <- weight_kg / (height_m^2)
  return(bmi)
}
```

## Best Practices

1. **Use descriptive names**: `calculate_mean` is better than `cm`
2. **Keep functions focused**: One function should do one thing
3. **Document your functions**: Add comments explaining purpose and parameters
4. **Check inputs**: Validate function arguments
5. **Handle errors**: Use appropriate error handling
6. **Return consistent types**: Make return values predictable
7. **Avoid global variables**: Pass values as parameters

## Next Steps

Learn about [Working with Packages](packages.md) to extend R's functionality.
