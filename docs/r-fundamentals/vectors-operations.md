# Vectors and Basic Operations

## What are Vectors?

Vectors are the most basic data structure in R. A vector is a sequence of elements of the same type.

## Creating Vectors

### Using c() function
```r
# Numeric vector
numbers <- c(1, 2, 3, 4, 5)
ages <- c(25, 30, 35, 40)

# Character vector
names <- c("Alice", "Bob", "Charlie")
colors <- c("red", "green", "blue")

# Logical vector
flags <- c(TRUE, FALSE, TRUE)
```

### Using : operator
```r
# Sequence of integers
1:10
10:1
seq <- 5:15
```

### Using seq() function
```r
# More control over sequences
seq(1, 10, by = 2)      # 1, 3, 5, 7, 9
seq(0, 1, length.out = 5)  # 0.00 0.25 0.50 0.75 1.00
```

### Using rep() function
```r
# Repeat values
rep(5, times = 3)       # 5 5 5
rep(c(1, 2), times = 2)  # 1 2 1 2
rep(c(1, 2), each = 2)  # 1 1 2 2
```

## Vector Operations

### Arithmetic Operations
```r
x <- c(1, 2, 3, 4, 5)
y <- c(10, 20, 30, 40, 50)

# Element-wise operations
x + y      # 11 22 33 44 55
x * y      # 10 40 90 160 250
x ^ 2      # 1 4 9 16 25
sqrt(x)    # 1.00 1.41 1.73 2.00 2.24
```

### Recycling
```r
# R recycles shorter vectors
c(1, 2, 3) + 10        # 11 12 13
c(1, 2, 3) * c(2, 3)   # 2 6 6 (with warning)
```

## Accessing Vector Elements

### By Index
```r
vec <- c(10, 20, 30, 40, 50)

# Single element (1-indexed)
vec[1]      # 10
vec[3]      # 30

# Multiple elements
vec[c(1, 3, 5)]  # 10 30 50
vec[1:3]         # 10 20 30

# Negative indexing (exclude elements)
vec[-1]     # 20 30 40 50
vec[-c(1, 3)]  # 20 40 50
```

### By Condition
```r
vec <- c(10, 20, 30, 40, 50)

# Logical indexing
vec[vec > 25]           # 30 40 50
vec[vec %% 20 == 0]     # 20 40

# Using which()
which(vec > 25)         # 3 4 5
vec[which(vec > 25)]    # 30 40 50
```

## Vector Functions

### Length and Summary
```r
vec <- c(1, 2, 3, 4, 5, NA, 7)

length(vec)      # 7
sum(vec)         # 22 (NA excluded)
sum(vec, na.rm = TRUE)  # 22
mean(vec, na.rm = TRUE) # 3.67
median(vec, na.rm = TRUE)
min(vec, na.rm = TRUE)
max(vec, na.rm = TRUE)
range(vec, na.rm = TRUE)
```

### Statistical Functions
```r
vec <- c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

mean(vec)        # 5.5
median(vec)      # 5.5
sd(vec)          # Standard deviation
var(vec)         # Variance
quantile(vec)    # Quartiles
summary(vec)     # Summary statistics
```

### Sorting and Ordering
```r
vec <- c(5, 2, 8, 1, 9)

sort(vec)                    # 1 2 5 8 9
sort(vec, decreasing = TRUE) # 9 8 5 2 1
order(vec)                   # 4 2 1 3 5 (indices)
vec[order(vec)]             # Same as sort(vec)
```

## Vector Coercion

R automatically converts vectors to a common type:

```r
# Mixed types get coerced
mixed <- c(1, "two", 3)      # All become character
mixed2 <- c(TRUE, 1, 2.5)    # All become numeric
```

## Common Vector Operations

```r
# Concatenation
vec1 <- c(1, 2, 3)
vec2 <- c(4, 5, 6)
combined <- c(vec1, vec2)    # 1 2 3 4 5 6

# Unique values
vec <- c(1, 2, 2, 3, 3, 3)
unique(vec)                  # 1 2 3

# Membership testing
vec <- c(1, 2, 3, 4, 5)
3 %in% vec                   # TRUE
c(3, 6) %in% vec             # TRUE FALSE
```

## Next Steps

Learn about more complex [Data Structures](data-structures.md) like matrices, lists, and data frames.
