# Data Visualization

Effective data visualization is crucial for understanding data and communicating insights. R, especially with ggplot2, provides powerful tools for creating publication-quality visualizations.

## Introduction to ggplot2

ggplot2 is based on the Grammar of Graphics, which provides a systematic way to build plots layer by layer.

### Basic Structure

```r
library(ggplot2)

# Basic syntax
ggplot(data = data, aes(x = x_var, y = y_var)) +
  geom_point()
```

## Scatter Plots

```r
# Basic scatter plot
ggplot(data, aes(x = x_var, y = y_var)) +
  geom_point()

# With customization
ggplot(data, aes(x = x_var, y = y_var)) +
  geom_point(
    color = "steelblue",
    size = 3,
    alpha = 0.6
  ) +
  labs(
    title = "Scatter Plot",
    x = "X Variable",
    y = "Y Variable"
  ) +
  theme_minimal()

# With color by category
ggplot(data, aes(x = x_var, y = y_var, color = category)) +
  geom_point() +
  labs(title = "Scatter Plot by Category")

# With trend line
ggplot(data, aes(x = x_var, y = y_var)) +
  geom_point() +
  geom_smooth(method = "lm", se = TRUE) +
  labs(title = "Scatter Plot with Trend Line")
```

## Bar Charts

```r
# Basic bar chart
ggplot(data, aes(x = category)) +
  geom_bar()

# Counts
ggplot(data, aes(x = category)) +
  geom_bar(fill = "steelblue") +
  labs(title = "Bar Chart")

# With values
summary_data <- data %>%
  group_by(category) %>%
  summarize(mean_value = mean(value))

ggplot(summary_data, aes(x = category, y = mean_value)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  labs(title = "Bar Chart with Values")

# Grouped bar chart
ggplot(data, aes(x = category1, fill = category2)) +
  geom_bar(position = "dodge") +
  labs(title = "Grouped Bar Chart")

# Stacked bar chart
ggplot(data, aes(x = category1, fill = category2)) +
  geom_bar(position = "stack") +
  labs(title = "Stacked Bar Chart")
```

## Histograms and Density Plots

```r
# Histogram
ggplot(data, aes(x = numeric_var)) +
  geom_histogram(bins = 30, fill = "steelblue", color = "white") +
  labs(title = "Histogram")

# Density plot
ggplot(data, aes(x = numeric_var)) +
  geom_density(fill = "steelblue", alpha = 0.7) +
  labs(title = "Density Plot")

# Overlapping densities
ggplot(data, aes(x = numeric_var, fill = category)) +
  geom_density(alpha = 0.5) +
  labs(title = "Overlapping Densities")
```

## Box Plots

```r
# Basic box plot
ggplot(data, aes(y = numeric_var)) +
  geom_boxplot() +
  labs(title = "Box Plot")

# By category
ggplot(data, aes(x = category, y = numeric_var)) +
  geom_boxplot(fill = "steelblue") +
  labs(title = "Box Plot by Category")

# Violin plot
ggplot(data, aes(x = category, y = numeric_var)) +
  geom_violin(fill = "steelblue", alpha = 0.7) +
  labs(title = "Violin Plot")
```

## Line Plots

```r
# Basic line plot
ggplot(data, aes(x = time, y = value)) +
  geom_line() +
  labs(title = "Line Plot")

# With points
ggplot(data, aes(x = time, y = value)) +
  geom_line() +
  geom_point() +
  labs(title = "Line Plot with Points")

# Multiple lines
ggplot(data, aes(x = time, y = value, color = series)) +
  geom_line() +
  labs(title = "Multiple Line Plot")
```

## Themes and Styling

```r
# Built-in themes
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  theme_minimal()

ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  theme_bw()

ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  theme_classic()

ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  theme_dark()

# Custom theme
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  theme(
    plot.title = element_text(size = 16, face = "bold"),
    axis.title = element_text(size = 12),
    axis.text = element_text(size = 10),
    legend.position = "bottom"
  )
```

## Color Scales

```r
# Continuous color scale
ggplot(data, aes(x = x, y = y, color = value)) +
  geom_point() +
  scale_color_gradient(low = "blue", high = "red")

# Discrete color scale
ggplot(data, aes(x = x, y = y, color = category)) +
  geom_point() +
  scale_color_brewer(palette = "Set1")

# Manual colors
ggplot(data, aes(x = x, y = y, color = category)) +
  geom_point() +
  scale_color_manual(values = c("A" = "red", "B" = "blue", "C" = "green"))
```

## Faceting

```r
# Facet wrap
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  facet_wrap(~ category)

# Facet grid
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  facet_grid(category1 ~ category2)

# With scales
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  facet_wrap(~ category, scales = "free")
```

## Advanced Visualizations

### Heatmaps

```r
# Correlation heatmap
library(corrplot)
cor_matrix <- cor(data[, sapply(data, is.numeric)])
corrplot(cor_matrix, method = "color")

# Using ggplot2
library(reshape2)
cor_melted <- melt(cor_matrix)
ggplot(cor_melted, aes(x = Var1, y = Var2, fill = value)) +
  geom_tile() +
  scale_fill_gradient2(low = "blue", high = "red", mid = "white")
```

### Pair Plots

```r
library(GGally)
ggpairs(data[, c("var1", "var2", "var3")])
```

### Maps

```r
library(maps)
library(mapdata)

# World map
world_map <- map_data("world")
ggplot(world_map, aes(x = long, y = lat, group = group)) +
  geom_polygon(fill = "lightblue", color = "black")
```

## Saving Plots

```r
# Save as PNG
ggsave("plot.png", width = 10, height = 6, dpi = 300)

# Save as PDF
ggsave("plot.pdf", width = 10, height = 6)

# Save as SVG
ggsave("plot.svg", width = 10, height = 6)

# High resolution
ggsave("plot.png", width = 10, height = 6, dpi = 300)
```

## Interactive Visualizations

```r
library(plotly)

# Convert ggplot to plotly
p <- ggplot(data, aes(x = x, y = y)) +
  geom_point()
ggplotly(p)

# Direct plotly
plot_ly(data, x = ~x, y = ~y, type = "scatter", mode = "markers")
```

## Best Practices

1. **Choose appropriate chart types**: Match visualization to data type
2. **Keep it simple**: Avoid unnecessary decorations
3. **Use color effectively**: Consider colorblind-friendly palettes
4. **Label clearly**: Always include titles and axis labels
5. **Maintain consistency**: Use consistent styling across plots
6. **Consider audience**: Adjust complexity for your audience
7. **Tell a story**: Visualizations should communicate insights

## Example: Publication-Ready Plot

```r
library(ggplot2)
library(dplyr)

# Prepare data
summary_data <- data %>%
  group_by(category, year) %>%
  summarize(mean_value = mean(value, na.rm = TRUE))

# Create plot
ggplot(summary_data, aes(x = year, y = mean_value, color = category)) +
  geom_line(size = 1.2) +
  geom_point(size = 3) +
  scale_color_brewer(palette = "Set1", name = "Category") +
  labs(
    title = "Trends Over Time by Category",
    subtitle = "Mean values from 2020-2024",
    x = "Year",
    y = "Mean Value",
    caption = "Data source: Internal Database"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 16, face = "bold"),
    plot.subtitle = element_text(size = 12, color = "gray50"),
    axis.title = element_text(size = 12),
    axis.text = element_text(size = 10),
    legend.position = "bottom",
    panel.grid.minor = element_blank()
  )

# Save
ggsave("publication_plot.png", width = 10, height = 6, dpi = 300)
```

## Next Steps

Apply your visualization skills with [Working with Real Datasets](real-datasets.md) to practice on actual data.
