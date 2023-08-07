
<!-- README.md is generated from README.Rmd. Please edit that file -->

# RJDemetra3

RJDemetra3 offers several functions to interact with JDemetra+ v3.0
workspaces. Seasonal adjustment with X-12ARIMA can be done with the
package [rjd3x13](https://github.com/rjdemetra/rjd3x13) and with
TRAMO-SEATS with the package
[rjd3tramoseats](https://github.com/rjdemetra/rjd3tramoseats).

## Installation

RJDemetra3 relies on the
[rJava](https://CRAN.R-project.org/package=rJava) package and Java SE 17
or later version is required.

``` r
# Install development version from GitHub
# install.packages("remotes")
remotes::install_github("rjdemetra/rjd3toolkit")
remotes::install_github("rjdemetra/rjd3x13")
remotes::install_github("rjdemetra/rjd3tramoseats")
remotes::install_github("rjdemetra/rjdemetra3")
```

## Usage

RJDemetra3 relies on the
[rJava](https://CRAN.R-project.org/package=rJava) package and Java SE 17
or later version is required.

``` r
library(rjdemetra3)
dir <- tempdir()
y <- rjd3toolkit::ABS$X0.2.09.10.M
jws <- .jws_new()
jmp1 <- .jws_multiprocessing_new(jws, "sa1")
add_sa_item(jmp1, name = "x13", x = rjd3x13::x13(y))
add_sa_item(jmp1, name = "tramo", x = rjd3tramoseats::tramoseats(y))
save_workspace(jws, file.path(dir, "wk.xml"))
#> [1] TRUE

ws <- .jws_open(file = file.path(dir, "wk.xml"))
.jws_compute(ws) # to compute the models
jmp1 <- .jws_multiprocessing(ws, idx = 1) # first multiprocessing
jsa1 <- .jmp_sa(jmp1, idx = 1) # first SaItem
.jsa_name(jsa1)
#> [1] "x13"
mod1 <- .jsa_read(jsa1)
```
