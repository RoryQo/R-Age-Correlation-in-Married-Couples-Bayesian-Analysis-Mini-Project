# <div align="center">**Age Correlation in Married Couples**</div>

## Table of Contents
1. [Overview](#overview)
2. [Methodology](#methodology)
3. [Data](#data)
4. [Results](#results)

## Overview
This project analyzes the relationship between the ages of husbands and wives using data from 100 sampled U.S. married couples. The goal is to hypothesize a semiconjugate prior, generate a predictive dataset to confirm this hypothesis and utilize Markov Chain Monte Carlo (MCMC) approximation to estimate the mean ages and the correlation coefficient.

## Methodology
1. **Hypothesis Formation**: After visualizing the relationship between the ages of husbands and wives, we hypothesize a semiconjugate prior. Specifically, we model the variation using an inverse Wishart distribution, while the mean ages of spouses follow a multivariate normal distribution.

2. **Predictive Data Generation**: We generate predictive datasets by taking random samples from the established distributions. These datasets help us confirm the validity of our priors by comparing simulated scatter plots to the actual data. Both follow a strong positive correlation

3. **MCMC Approximation**: Once the distributions are confirmed, we apply MCMC methods to estimate the mean correlation between the ages of husbands and wives. This allows us to create new confidence intervals for average ages and correlations.
      - ```
         # Generate predictive data set (size 100) from
average ages (theta) and cov matrix (sigma)

n = 100

s = 10

# Mean of ages

mu0 <- c(42,42)

L0 <- matrix(c(441, 330.75, 330.75, 441), nrow = 2, ncol = 2)

# Mean ages follow multivariate norm

theta <- mvrnorm(s, mu0, L0)



# sample sigmas following inverse wishart distribution

sigmas <- list()

for(i in 1:s){
 sigma <- solve(rWishart(1, 4, solve(L0))[,,1])

 sigmas[[i]] <- sigma
}


# Sample age from multivariate normal distribution

data <- data.frame(h_age= c(), w_age = c(), dataset= c())

for(i in 1:s){
 y <- mvrnorm(100, mu0, L0)

 new <- data.frame(h_age = y[,1], w_age = y[,2], dataset =i)

 data <- rbind(data, new)

}
```

## Data
The dataset comprises the ages of spouses from 100 sampled U.S. married couples:
- **ageh**: Age of the husband
- **agew**: Age of the wife

## Results
The limited sample size and lack of prior information about the variables led to a relatively wide confidence interval using standard frequentist approaches. In contrast, the Bayesian approach reduced the confidence interval for the average ages of husbands and wives by over a year. The confidence interval for the correlation coefficient was also reduced by 2% compared to the frequentist method (at the same alpha level).

These findings highlight the advantages of Bayesian methods over traditional frequentist approaches, particularly in providing narrower confidence intervals and better estimations of correlations. The ability to leverage prior information significantly enhances the reliability of our analysis regarding age dynamics in marriages.
