# Control Flow

Control flow statements allow you to control the execution of your R code based on conditions and loops.

## Conditional Statements

### if Statement
```r
x <- 10

if (x > 5) {
  print("x is greater than 5")
}
```

### if-else Statement
```r
x <- 3

if (x > 5) {
  print("x is greater than 5")
} else {
  print("x is not greater than 5")
}
```

### if-else if-else Statement
```r
score <- 85

if (score >= 90) {
  grade <- "A"
} else if (score >= 80) {
  grade <- "B"
} else if (score >= 70) {
  grade <- "C"
} else {
  grade <- "F"
}

print(grade)
```

### ifelse() Function
```r
# Vectorized conditional
x <- c(1, 5, 10, 15, 20)
result <- ifelse(x > 10, "High", "Low")
# "Low" "Low" "Low" "High" "High"
```

### switch() Statement
```r
choice <- "b"

result <- switch(choice,
  "a" = "Apple",
  "b" = "Banana",
  "c" = "Cherry",
  "Unknown"
)
# "Banana"
```

## Loops

### for Loop
```r
# Basic for loop
for (i in 1:5) {
  print(i)
}

# Loop over vector
fruits <- c("apple", "banana", "cherry")
for (fruit in fruits) {
  print(fruit)
}

# Loop with index
for (i in seq_along(fruits)) {
  print(paste(i, fruits[i]))
}
```

### while Loop
```r
# Basic while loop
count <- 1
while (count <= 5) {
  print(count)
  count <- count + 1
}

# Conditional while loop
x <- 10
while (x > 0) {
  print(x)
  x <- x - 2
}
```

### repeat Loop
```r
# Infinite loop with break
count <- 1
repeat {
  print(count)
  count <- count + 1
  if (count > 5) {
    break
  }
}
```

## Loop Control Statements

### break Statement
```r
# Exit loop early
for (i in 1:10) {
  if (i == 5) {
    break
  }
  print(i)
}
# Prints 1, 2, 3, 4
```

### next Statement
```r
# Skip current iteration
for (i in 1:10) {
  if (i %% 2 == 0) {
    next  # Skip even numbers
  }
  print(i)
}
# Prints 1, 3, 5, 7, 9
```

## Vectorized Operations vs Loops

R is optimized for vectorized operations. Avoid loops when possible:

```r
# Slow: Using loop
result <- numeric(1000)
for (i in 1:1000) {
  result[i] <- i^2
}

# Fast: Vectorized
result <- (1:1000)^2

# Another example
x <- 1:1000
# Slow
sum_squared <- 0
for (val in x) {
  sum_squared <- sum_squared + val^2
}

# Fast
sum_squared <- sum(x^2)
```

## apply() Family Functions

These functions apply operations to data structures without explicit loops:

### apply() - for matrices/arrays
```r
mat <- matrix(1:12, nrow = 3, ncol = 4)

apply(mat, 1, sum)  # Sum of each row
apply(mat, 2, mean) # Mean of each column
```

### lapply() - for lists
```r
my_list <- list(a = 1:5, b = 6:10, c = 11:15)
lapply(my_list, sum)  # Returns a list
```

### sapply() - simplified lapply
```r
sapply(my_list, sum)  # Returns a vector
```

### vapply() - apply with type checking
```r
vapply(my_list, sum, numeric(1))  # Ensures numeric output
```

### tapply() - apply by group
```r
scores <- c(85, 90, 88, 92, 87)
groups <- c("A", "B", "A", "B", "A")
tapply(scores, groups, mean)
```

## Best Practices

1. **Prefer vectorization**: Use vectorized operations instead of loops when possible
2. **Use apply() family**: More readable and often faster than loops
3. **Avoid modifying objects in loops**: Create new objects instead
4. **Use meaningful variable names**: `i`, `j` are fine for simple loops, but use descriptive names when helpful
5. **Break complex conditions**: Make conditions readable

## Example: Complete Control Flow

```r
# Process student grades
students <- data.frame(
  name = c("Alice", "Bob", "Charlie", "Diana"),
  score = c(85, 92, 78, 95)
)

# Add grade column using ifelse
students$grade <- ifelse(students$score >= 90, "A",
                        ifelse(students$score >= 80, "B",
                              ifelse(students$score >= 70, "C", "F")))

# Process with loop
for (i in 1:nrow(students)) {
  if (students$score[i] >= 90) {
    students$status[i] <- "Excellent"
  } else if (students$score[i] >= 80) {
    students$status[i] <- "Good"
  } else {
    students$status[i] <- "Needs Improvement"
  }
}
```

## Next Steps

Learn about [Functions](functions.md) to create reusable code blocks.
