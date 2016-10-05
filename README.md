
[![Build Status](https://travis-ci.org/pat-s/oddsratio.svg?branch=master)](https://travis-ci.org/pat-s/oddsratio) [![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/oddsratio)](http://cran.r-project.org/package=oddsratio)

oddsratio
=========

Convenience functions for odds ratio calculation of Generalized Additive (Mixed) Models and Generalized Linear (Mixed) Models with a binomial response variable (i.e. logistic regression models).

Examples
--------

### GLM

Odds ratio calculation of predictors `gre` & `gpa` of a fitted model `fit.glm` with increment steps of 380 and 5, respectively.
For factor variables (here: `rank`with 4 levels), automatically all odds ratios corresponding to the base level (here: `rank1`) are returned.

``` r
calc.oddsratio.glm(data = dat, model = fit.glm, incr = list(gre = 380, gpa = 5))
```

``` r
Variable:   'gre'
Increment:  '380'
Odds ratio: 2.364

Variable:   'gpa'
Increment:  '5'
Odds ratio: 55.712

Variable:   'rank2'
Increment:  'Non numeric predictor. Refer to basis factor level!'
Odds ratio: 0.509

Variable:   'rank3'
Increment:  'Non numeric predictor. Refer to basis factor level!'
Odds ratio: 0.262

Variable:   'rank4'
Increment:  'Non numeric predictor. Refer to basis factor level!'
Odds ratio: 0.212
```

Resulting in a plain numerical output (odds ratio) without reference to its respective predictor. Mistakes can be made quite easily by specifying the increment steps in a wrong order (referencing to the order of coefficients). Cases in which the coefficients are non-numerical are not covered in this example. However, the package also takes care of these cases. Function `calc.odds.ratio.glm()` provides a nicely formatted output and helps avoiding false references of predictors and increment steps by explicitly specifying the relations in its argument `incr`.

``` r
calc.oddsratio.glm(data = dat, model = fit.glm, incr = list(gre = 380, gpa = 5))
```

### GAM

For GAMs, the calculation of odds ratio is different. Due to its non-linear definition, odds ratios do only apply to specific value pairs and are not constant throughout the whole value range of the predictor as for GLMs. Hence, odds ratios of GAMs can only be computed for one predictor at a time fixing all other predictors to a fixed value while only changing the desired value combination of the specific predictor.

To simplify this behaviour, the `oddsratio` package does this work for you: Function `calc.odds.ratio.gam` takes the predictor name (`pred`) and the two values (`values`) as arguments and saves you time and coding lines.

``` r
calc.oddsratio.gam(data = dat, model = fit.gam, pred = "x2", values = c(0.099, 0.198))
```

If you want to compute multiple odds ratios, the argument `slice = TRUE` provides the opportunity to split the value range in percentage steps (`percentage`) for which odds ratios are calculated. For example, applying a value of *20* will split the value range of the chosen predictor in five parts and computes odds ratios between those (0% - 20%, 20% - 40%, etc.).

``` r
calc.oddsratio.gam(data = dat, model = fit.gam, pred = "x2", percentage = 20, slice = TRUE)
```

Installation
------------

Get the development version from Github:

``` r
devtools::install_github("pat-s/oddsratio")
```

Examples
--------

To see the functions in action, please see the examples in the respective help pages `?calc.oddsratio.gam` and `?calc.oddsratio.gam`.

To Do
-----

-   Write vignette (necessary?) for both functions or include both in one?
-   Add calculation of odds ratio confidence intervals
-   Implement plotting function of calculated odds ratio in GLM/GAM function?
-   Add references for odds ratio calculation
