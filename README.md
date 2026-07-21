# DSA 406: Exploratory Data Analysis and Statistical Modeling in R

North Carolina State University, Spring 2026

This repository contains coursework completed for DSA 406, a course covering exploratory data analysis, statistical distribution diagnostics, correlation analysis, and linear regression modeling in R. All reports were authored in R Markdown or Quarto and rendered to HTML, using RStudio on Posit Cloud.

Each `.html` file is a fully rendered, self contained report and can be opened directly in a browser without needing to re-run any code.

## Project 1 and 2: YouTube Bounding Boxes (YT-BB) Dataset Analysis

**Research question:** What are the most commonly annotated object classes in the YT-BB dataset, and how do bounding box dimensions vary across classes?

**Dataset:** YouTube-BoundingBoxes (YT-BB), 100,000 observations across 10 variables, describing annotated object bounding boxes in video frames.

### Part 1, Foundational EDA

1. Loaded and cleaned the dataset using `read_csv()`, `clean_names()`, and `complete.cases()`, identifying variable types and handling missing values.
2. Performed data wrangling with `mutate()` and `across()` to correct data types and engineer new bounding box dimension variables (width, height, area).
3. Computed descriptive statistics using `summary()` and `skim()` to characterize central tendency and spread.
4. Built five visualizations, each with a written interpretation:
   - Bar chart of the top ten most common object classes
   - Histogram of bounding box area distribution
   - Boxplot of bounding box width by the top five classes
   - Scatterplot of bounding box width against height for the top five classes
   - Object presence versus absence chart

**Findings:** Bounding box area is right skewed. Class annotations are imbalanced, dominated by a small number of frequent classes. Different object classes show distinct spatial signatures in their bounding box dimensions, consistent with the physical shapes of the underlying object types.

### Part 2, Advanced EDA, Correlation, and Regression

**Hypothesis from Part 1:** Different object classes will show distinctly different distributions of bounding box dimensions, reflecting natural variation in real world object sizes.

**New hypothesis for Part 2:** Bounding box width and height will be positively correlated, meaning wider objects also tend to be taller, such that bounding box area can be reasonably predicted from a linear combination of width and height.

1. Applied grouped descriptive statistics using `describeBy()` across object classes.
2. Used `skimr::skim()` and `summarytools::dfSummary()` for distribution diagnostics, and applied a log transformation to reduce right skew and improve normality.
3. Built a Pearson correlation matrix and heatmap across numeric variables, with significance testing via `corr.test()`.
4. Fit a linear regression model predicting `bb_area` from `bb_width` and `bb_height`, and validated the model against standard linear regression assumptions: linearity, normality of residuals, multicollinearity, and homoscedasticity.

**Findings:** The hypothesis was confirmed. Bounding box width and height are moderately to strongly positively correlated, and the regression model achieved a high R-squared, with the log transformed version showing improved residual normality. A notable limitation is class imbalance, since the dataset is dominated by the "person" class, which may bias the overall correlation and regression results. Future work could fit separate models per object class.

## Assignment 3: NHL 2022-2023 Season Analysis

**Research question:** What factors are most strongly associated with team wins in the NHL 2022-2023 regular season?

**Dataset:** NHL 2022-2023 regular season team statistics, 32 teams across 18 variables.

1. Conducted exploratory data analysis and built a Pearson correlation matrix and heatmap across key performance variables.
2. Fit a linear regression model, `W ~ GF + GA`, predicting regulation wins from goals for and goals against.
3. Validated the model against standard assumptions: linearity, normality of residuals, multicollinearity (via variance inflation factor, VIF), and homoscedasticity.

**Findings:**

| Predictor | Correlation with W | Interpretation |
|---|---|---|
| DIFF (goal difference) | 0.969 | Best single summary of team dominance |
| GA (goals against) | -0.887 | Defensive quality is strongly tied to wins |
| GF (goals for) | 0.797 | Offense matters, but slightly less than defense |

- The model `W ~ GF + GA` explains 94% of the variance in regulation wins (R-squared = 0.94).
- Both GF (+0.153 per goal) and GA (-0.164 per goal) are highly statistically significant (p less than 0.001).
- The GA coefficient is slightly larger in magnitude than the GF coefficient, suggesting defensive performance is marginally more predictive of winning than offensive performance.
- Shootout statistics (SOW, SOL, shootout win percentage) show near zero correlation with wins, consistent with shootout outcomes being largely random.

## Tools and Libraries

R, RStudio on Posit Cloud, R Markdown and Quarto for reporting, and the tidyverse ecosystem, including `dplyr`, `readr`, and `janitor` for data wrangling, `ggplot2` for visualization, `skimr` and `summarytools` for distribution diagnostics, `psych` for grouped descriptive statistics and correlation significance testing, and `car` for variance inflation factor calculations.

## Reproducing the Analysis

Each report was originally authored as an R Markdown or Quarto document. To reproduce a report from source:

1. Open RStudio, either locally or on Posit Cloud.
2. Install the required packages: `tidyverse`, `skimr`, `summarytools`, `psych`, `car`, and `knitr`.
3. Load the corresponding dataset (the YT-BB dataset for Project 1 and 2, or the NHL 2022-2023 season statistics for Assignment 3).
4. Knit or render the R Markdown or Quarto source file to reproduce the full report, including all figures, tables, and statistical output.
