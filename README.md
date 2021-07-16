
<!-- README.md is generated from README.Rmd. Please edit that file -->

# modelops

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/modelops)](https://CRAN.R-project.org/package=modelops)
<!-- badges: end -->

The goal of modelops is to provide fluent tooling to version, share,
deploy, and monitor a trained model. Functions handle both recording and
checking the model’s input data prototype, and predicting from a remote
API endpoint. The modelops package is extensible, with generics that can
be supported by many kinds of models. For an example of how to use and
extend the modelops package, see
[deploytidymodels](https://github.com/tidymodels/deploytidymodels).

## Installation

~~You can install the released version of modelops from
[CRAN](https://CRAN.R-project.org) with:~~

``` r
install.packages("modelops") ## not yet
```

And the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("rstudio/modelops")
```

## Example

You can **version** and **share** your model by
[pinning](https://pins.rstudio.com/dev/) it, to a local folder, RStudio
Connect, Amazon S3, and more.

``` r
library(modelops)
library(pins)
model_board <- board_temp()

cars_lm <- lm(mpg ~ ., data = mtcars)

model_board %>% pin_model(cars_lm, "cars")
#> Creating new version '20210712T231612Z-adfa2'
model_board
#> Pin board <pins_board_folder>
#> Path: '/var/folders/hv/hzsmmyk9393_m7q3nscx1slc0000gn/T/RtmptsJbku/pins-bc1d4b6c31ca'
#> Cache size: 0
#> Pins [1]: 'cars'
```

You can **deploy** your pinned model via a [Plumber
API](https://www.rplumber.io/), which can be [hosted in a variety of
ways](https://www.rplumber.io/articles/hosting.html).

``` r
library(plumber)
pr() %>%
    pr_model(model_board, "cars") %>%
    pr_run(port = 8088)
```

Make predictions with your deployed model at its endpoint and new data.

``` r
endpoint <- model_endpoint("http://127.0.0.1:8088/predict")
predict(endpoint, mtcars[4:7, -1])
#> # A tibble: 4 x 1
#>   .pred
#>   <dbl>
#> 1  21.2
#> 2  17.7
#> 3  20.4
#> 4  14.4
```

## Contributing

This project is released with a [Contributor Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.

-   For questions and discussions about modeling packages, modeling, and
    machine learning, please [post on RStudio
    Community](https://community.rstudio.com/new-topic?category_id=15&tags=tidymodels,question).

-   If you think you have encountered a bug, please [submit an
    issue](https://github.com/rstudio/modelops/issues).

-   Either way, learn how to create and share a
    [reprex](https://reprex.tidyverse.org/articles/articles/learn-reprex.html)
    (a minimal, reproducible example), to clearly communicate about your
    code.