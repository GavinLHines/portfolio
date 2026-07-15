# Mangrove Tree Data Analysis (Notes)

> **Context:** College coursework, NOT internship work. Final project for CIS 351 B (Data Analysis), Flagler College, Spring 2026. Team of three: Ava Mariano, Jazmine Major, Gavin Hines.

## What it is

A full exploratory data analysis in R of lab chemistry results from mangrove tree samples, produced for the CIS 351 B final project. The dataset came from a Flagler senior capstone that sampled mangrove trees at seven sites and had the material analyzed at the University of Hawaii at Hilo's analytical lab. We analyzed organic matter (OM%), carbon (%C), and nitrogen (%N) against species (Red vs. Black Mangrove) and sampling site.

## Dataset

- 126 plant samples across seven sites (SUN, MON, TAN, LOB, TOY, STR, FRI)
- Two species: Red Mangrove (60), Black Mangrove (66)
- Numerical variables: om_percent, c_percent, n_percent
- Categorical variables: species, site
- Source file: `source_data.csv`

## Data cleaning

- Standardized column names to lower snake_case with `janitor::clean_names()`, then renamed (e.g. `percent_c` to `c_percent`)
- Dropped NA rows
- Fixed typos, spacing, and stray author comments in `weighboat_code` using `str_replace_all` and `str_remove` (regex to strip everything after a hyphen)
- Derived `species` and `site` categorical variables from `weighboat_code` using `case_when`, `str_starts`, and `str_detect`
- Caught and corrected a unit-conversion error: the original capstone students computed %C and %N from OM% using formulas but mixed up percent vs. decimal inputs, producing impossible values (e.g. N% over 700%). We rescaled the formulas to bring results into realistic ranges for mangroves.

## Analysis

- Descriptive statistics for all numerical and categorical variables
- Visualizations (ggplot2 / ggpubr): stacked bar chart of species by site, histogram of OM% (right-skewed), boxplot of OM% by species with error bars, scatter plot of mean OM% vs. mean N% by site with regression line, percent-stacked bar chart of nutrient potential by site
- Statistical tests:
  - Kruskal-Wallis on OM% by site: chi-squared = 70.634, df = 6, p = 3.03e-13. Reject null; OM% differs significantly across sites.
  - Wilcoxon rank-sum on OM% by species: W = 1598, p = 0.2301. Fail to reject; no significant difference between species.

## Findings

Organic matter content of mangrove samples differs across environments (sites) but not between the two species. Sites MON and LOB showed the highest nutrient potential. Higher OM% implies higher carbon and nitrogen, which signals a healthier environment for the trees.

## Limitations

%C and %N were formula-derived from OM% rather than lab-measured, forcing a linear relationship (R = 1) that made those two variables uninteresting to compare directly. The data was not normally distributed, so we used non-parametric tests (Kruskal-Wallis, Wilcoxon) rather than ANOVA or t-tests.

## Skills

R, tidyverse (dplyr, stringr), ggplot2, ggpubr, janitor, data cleaning, regex, exploratory data analysis, non-parametric hypothesis testing.

## Files

- `Final_Project_Submission.html`: the rendered R Markdown report with all code, output, and figures
- `source_data.csv`: the raw dataset
- `assignment_rubric.html`: the CIS 351 B assignment brief, for context on requirements
