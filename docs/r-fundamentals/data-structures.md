# Data Structures

R provides several data structures beyond vectors. Understanding these is crucial for effective data manipulation.

## Matrices

A matrix is a two-dimensional array of elements of the same type.

### Creating Matrices
```r
# From a vector
matrix(1:12, nrow = 3, ncol = 4)
matrix(1:12, nrow = 3, ncol = 4, byrow = TRUE)

# Direct creation
mat <- matrix(c(1, 2, 3, 4, 5, 6), nrow = 2, ncol = 3)

# Using cbind() and rbind()
col1 <- c(1, 2, 3)
col2 <- c(4, 5, 6)
mat <- cbind(col1, col2)  # Combine as columns
mat <- rbind(col1, col2)  # Combine as rows
```

### Accessing Matrix Elements
```r
mat <- matrix(1:12, nrow = 3, ncol = 4)

mat[1, 2]        # Element at row 1, column 2
mat[1, ]         # First row
mat[, 2]         # Second column
mat[1:2, 2:3]    # Submatrix
```

### Matrix Operations
```r
mat1 <- matrix(1:4, nrow = 2)
mat2 <- matrix(5:8, nrow = 2)

mat1 + mat2      # Element-wise addition
mat1 * mat2      # Element-wise multiplication
mat1 %*% mat2    # Matrix multiplication
t(mat1)          # Transpose
```

## Arrays

Arrays are multi-dimensional generalizations of matrices.

```r
# 3D array
arr <- array(1:24, dim = c(2, 3, 4))
arr[1, 2, 3]     # Access element
dim(arr)         # Dimensions
```

## Lists

Lists can contain elements of different types, including other lists.

### Creating Lists
```r
# Basic list
my_list <- list(1, "hello", TRUE, c(1, 2, 3))

# Named list
person <- list(
  name = "Alice",
  age = 30,
  scores = c(85, 90, 88)
)
```

### Accessing List Elements
```r
person <- list(name = "Alice", age = 30, scores = c(85, 90, 88))

# By index
person[[1]]           # "Alice"
person[["name"]]      # "Alice"
person$name           # "Alice"

# Multiple elements (returns a list)
person[c("name", "age")]
```

### Modifying Lists
```r
person$city <- "New York"
person[["email"]] <- "alice@example.com"
person[[4]] <- "New element"
```

## Data Frames

Data frames are the most important data structure for data analysis - they're like tables with rows and columns.

### Creating Data Frames
```r
# From vectors
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(25, 30, 35),
  city = c("NYC", "LA", "Chicago")
)

# From existing data
df2 <- data.frame(
  x = 1:5,
  y = letters[1:5],
  z = c(TRUE, FALSE, TRUE, FALSE, TRUE)
)
```

### Accessing Data Frame Elements
```r
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(25, 30, 35),
  city = c("NYC", "LA", "Chicago")
)

# By column name
df$name
df[["age"]]
df[, "city"]

# By position
df[1, ]           # First row
df[, 2]           # Second column
df[1, 2]          # Element at row 1, col 2

# Subsetting
df[df$age > 27, ]
df[, c("name", "age")]
```

### Data Frame Functions
```r
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(25, 30, 35),
  city = c("NYC", "LA", "Chicago")
)

nrow(df)          # Number of rows
ncol(df)          # Number of columns
dim(df)           # Dimensions
names(df)         # Column names
str(df)           # Structure
summary(df)       # Summary statistics
head(df)          # First few rows
tail(df)          # Last few rows
```

### Modifying Data Frames
```r
# Add new column
df$salary <- c(50000, 60000, 70000)

# Add new row
df <- rbind(df, data.frame(
  name = "David",
  age = 28,
  city = "Boston",
  salary = 55000
))

# Rename columns
names(df)[1] <- "full_name"
colnames(df) <- c("full_name", "age", "city", "salary")
```

## Factors

Factors are used to represent categorical data.

```r
# Creating factors
gender <- factor(c("M", "F", "M", "F", "M"))
levels(gender)    # "F" "M"

# Ordered factors
size <- factor(c("S", "M", "L", "M", "S"),
               levels = c("S", "M", "L"),
               ordered = TRUE)

# Converting to/from factors
as.factor(c(1, 2, 3))
as.numeric(factor(c("A", "B", "C")))
```

## Checking Data Structures

```r
# Type checking
is.vector(x)
is.matrix(x)
is.array(x)
is.list(x)
is.data.frame(x)
is.factor(x)

# Class information
class(x)
typeof(x)
str(x)
```

## Next Steps

Learn about [Control Flow](control-flow.md) to add logic and loops to your R programs.
