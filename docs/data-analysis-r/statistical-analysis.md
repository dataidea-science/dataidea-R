# Statistical Analysis

Statistical analysis involves applying statistical methods to data to draw conclusions, make inferences, and test hypotheses. This tutorial covers common statistical techniques in R.

## Descriptive Statistics

### Measures of Central Tendency

```r
# Mean
mean(data$variable, na.rm = TRUE)

# Median
median(data$variable, na.rm = TRUE)

# Mode (custom function)
get_mode <- function(x) {
  ux <- unique(x)
  ux[which.max(tabulate(match(x, ux)))]
}
get_mode(data$variable)

# Weighted mean
weighted.mean(data$value, data$weight)
```

### Measures of Dispersion

```r
# Standard deviation
sd(data$variable, na.rm = TRUE)

# Variance
var(data$variable, na.rm = TRUE)

# Range
range(data$variable, na.rm = TRUE)
diff(range(data$variable, na.rm = TRUE))

# Interquartile range
IQR(data$variable, na.rm = TRUE)

# Coefficient of variation
cv <- function(x) sd(x, na.rm = TRUE) / mean(x, na.rm = TRUE)
cv(data$variable)
```

### Summary Statistics

```r
# Complete summary
summary(data$variable)

# Custom summary function
library(dplyr)
data %>%
  summarize(
    mean = mean(variable, na.rm = TRUE),
    median = median(variable, na.rm = TRUE),
    sd = sd(variable, na.rm = TRUE),
    min = min(variable, na.rm = TRUE),
    max = max(variable, na.rm = TRUE),
    q25 = quantile(variable, 0.25, na.rm = TRUE),
    q75 = quantile(variable, 0.75, na.rm = TRUE)
  )
```

## Hypothesis Testing

### One-Sample T-Test

```r
# Test if mean equals a specific value
t.test(data$variable, mu = 100)

# One-sided test
t.test(data$variable, mu = 100, alternative = "greater")
t.test(data$variable, mu = 100, alternative = "less")
```

### Two-Sample T-Test

```r
# Independent samples
t.test(variable ~ group, data = data)
t.test(data$group1, data$group2)

# Paired samples
t.test(data$before, data$after, paired = TRUE)

# Equal variances assumption
t.test(variable ~ group, data = data, var.equal = TRUE)
```

### Chi-Square Test

```r
# Test of independence
chisq.test(table(data$var1, data$var2))

# Goodness of fit
observed <- c(10, 20, 30)
expected <- c(15, 20, 25)
chisq.test(observed, p = expected / sum(expected))
```

### ANOVA

```r
# One-way ANOVA
aov_result <- aov(variable ~ factor, data = data)
summary(aov_result)

# Post-hoc tests
TukeyHSD(aov_result)

# Two-way ANOVA
aov_result2 <- aov(variable ~ factor1 * factor2, data = data)
summary(aov_result2)
```

### Non-Parametric Tests

```r
# Wilcoxon rank-sum test (Mann-Whitney)
wilcox.test(variable ~ group, data = data)

# Wilcoxon signed-rank test
wilcox.test(data$before, data$after, paired = TRUE)

# Kruskal-Wallis test
kruskal.test(variable ~ factor, data = data)

# Friedman test
friedman.test(variable ~ factor | block, data = data)
```

## Correlation Analysis

### Pearson Correlation

```r
# Correlation coefficient
cor(data$var1, data$var2, use = "complete.obs")

# Correlation test
cor.test(data$var1, data$var2)

# Correlation matrix
cor(data[, c("var1", "var2", "var3")], use = "complete.obs")
```

### Spearman Correlation

```r
# Spearman rank correlation
cor(data$var1, data$var2, method = "spearman")
cor.test(data$var1, data$var2, method = "spearman")
```

### Partial Correlation

```r
library(ppcor)

# Partial correlation (controlling for other variables)
pcor.test(data$var1, data$var2, data$control_var)
```

## Regression Analysis

### Simple Linear Regression

```r
# Fit model
model <- lm(y ~ x, data = data)
summary(model)

# Model diagnostics
plot(model)

# Predictions
predict(model, newdata = data.frame(x = c(10, 20, 30)))

# Confidence intervals
confint(model)
predict(model, newdata = data.frame(x = 10), interval = "confidence")
predict(model, newdata = data.frame(x = 10), interval = "prediction")
```

### Multiple Linear Regression

```r
# Multiple predictors
model <- lm(y ~ x1 + x2 + x3, data = data)
summary(model)

# Interaction terms
model <- lm(y ~ x1 * x2, data = data)

# Polynomial terms
model <- lm(y ~ x + I(x^2), data = data)
```

### Logistic Regression

```r
# Binary outcome
model <- glm(outcome ~ predictor, data = data, family = binomial)
summary(model)

# Odds ratios
exp(coef(model))
exp(confint(model))
```

### Model Selection

```r
# Stepwise selection
null_model <- lm(y ~ 1, data = data)
full_model <- lm(y ~ ., data = data)
step_model <- step(null_model, scope = list(upper = full_model), direction = "both")

# AIC comparison
AIC(model1, model2)
```

## Time Series Analysis

```r
library(forecast)

# Create time series object
ts_data <- ts(data$value, start = c(2020, 1), frequency = 12)

# Decompose
decompose(ts_data)

# Autocorrelation
acf(ts_data)
pacf(ts_data)

# ARIMA model
arima_model <- auto.arima(ts_data)
forecast(arima_model, h = 12)
```

## Survival Analysis

```r
library(survival)

# Kaplan-Meier estimator
surv_obj <- Surv(time = data$time, event = data$event)
km_fit <- survfit(surv_obj ~ group, data = data)
plot(km_fit)

# Log-rank test
survdiff(surv_obj ~ group, data = data)

# Cox proportional hazards
cox_model <- coxph(surv_obj ~ predictor, data = data)
summary(cox_model)
```

## Power Analysis

```r
library(pwr)

# T-test power
pwr.t.test(d = 0.5, power = 0.8, sig.level = 0.05)

# ANOVA power
pwr.anova.test(k = 3, f = 0.25, power = 0.8, sig.level = 0.05)

# Correlation power
pwr.r.test(r = 0.3, power = 0.8, sig.level = 0.05)
```

## Effect Sizes

```r
library(effectsize)

# Cohen's d
cohens_d(data$group1, data$group2)

# Eta squared
eta_squared(aov_result)

# Cramér's V
cramers_v(table(data$var1, data$var2))
```

## Example: Complete Statistical Analysis

```r
library(dplyr)
library(broom)

# Load data
data <- read.csv("data.csv")

# 1. Descriptive statistics
summary_stats <- data %>%
  group_by(group) %>%
  summarize(
    n = n(),
    mean = mean(value, na.rm = TRUE),
    sd = sd(value, na.rm = TRUE),
    median = median(value, na.rm = TRUE),
    q25 = quantile(value, 0.25, na.rm = TRUE),
    q75 = quantile(value, 0.75, na.rm = TRUE)
  )

# 2. Normality test
shapiro.test(data$value)

# 3. Compare groups
# If normal: t-test
t_test <- t.test(value ~ group, data = data)
tidy(t_test)

# If not normal: Wilcoxon test
wilcox_test <- wilcox.test(value ~ group, data = data)
tidy(wilcox_test)

# 4. Correlation
cor_test <- cor.test(data$var1, data$var2)
tidy(cor_test)

# 5. Regression
model <- lm(value ~ predictor1 + predictor2, data = data)
summary(model)
tidy(model)
glance(model)

# 6. Model diagnostics
plot(model)
```

## Best Practices

1. **Check assumptions**: Verify assumptions before tests
2. **Choose appropriate tests**: Consider data distribution and type
3. **Report effect sizes**: Don't just report p-values
4. **Multiple comparisons**: Adjust for multiple testing
5. **Interpret results**: Understand what results mean
6. **Document methods**: Keep track of analyses performed

## Next Steps

Learn about [Data Visualization](visualization.md) to effectively communicate your statistical findings.
