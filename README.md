
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rjdemetra3

**rjdemetra3** offers several functions to create, read and modify JDemetra+ v3.0
workspaces, a specific data structure, indispensable to use the Graphical User Interface (GUI).
A workspace can be created in GUI or direclty in R.

## Installation

**rjdemetra3** relies on
[**rJava**](https://CRAN.R-project.org/package=rJava) package and Java
SE 17 or later version is required.

To get the current stable version (from the latest release):

``` r
# install.packages
remotes::install_github("rjdemetra/rjd3toolkit@*release")
remotes::install_github("rjdemetra/rjd3tramoseats@*release")
remotes::install_github("rjdemetra/rjd3x13@*release")
remotes::install_github("rjdemetra/rjd3providers@*release")
remotes::install_github("rjdemetra/rjdemetra3@*release")
```

To get the current development version from GitHub:

``` r
# install.packages("remotes")
remotes::install_github("rjdemetra/rjdemetra3")
```

## Usage

**rjdemetra3** relies on the
[**rJava**](https://CRAN.R-project.org/package=rJava) package and Java
SE 17 or later version is required.

``` r
library("rjdemetra3")

dir <- tempdir()

y <- rjd3toolkit::ABS$X0.2.09.10.M
jws <- .jws_new()
jsap1 <- .jws_sap_new(jws, "sa1")
add_sa_item(jsap1, name = "x13", x = rjd3x13::x13(y))
add_sa_item(jsap1, name = "tramo", x = rjd3tramoseats::tramoseats(y))
save_workspace(jws, file.path(dir, "ws.xml"))

jws <- .jws_load(file = file.path(dir, "ws.xml"))
.jws_compute(jws) # to compute the models
jsap1 <- .jws_sap(jws, idx = 1) # first SAProcessing
jsa1 <- .jsap_sa(jsap1, idx = 1) # first SAItem
.jsa_name(jsa1)
#> [1] "x13"
mod1 <- .jsa_read(jsa1)
```

### Create SA-item with path

``` r
# install.packages("remotes")
# remotes::install_github("rjdemetra/rjd3providers")

dir <- tempdir()

xlsx_file <- paste0(system.file("examples", package="rjd3providers"), "/Insee.xlsx")
ts1 <- rjd3providers::spreadsheet_series(file = xlsx_file, sheet = 1L, series = 3L)

y <- ts1$data
jws <- .jws_new()
jsap1 <- .jws_sap_new(jws, "sa1")
add_sa_item(jsap1, name = "x13", x = rjd3x13::x13(y))
rjdemetra3::set_ts(jsap = jsap1, idx = 1L, y = ts1)
add_sa_item(jsap1, name = "tramo", x = rjd3tramoseats::tramoseats(y))
rjdemetra3::set_ts(jsap = jsap1, idx = 2L, y = ts1)

save_workspace(jws, file.path(dir, "ws.xml"))

jws <- .jws_load(file = file.path(dir, "ws.xml"))
.jws_compute(jws) # to compute the models
jsap1 <- .jws_sap(jws, idx = 1) # first SAProcessing
jsa1 <- .jsap_sa(jsap1, idx = 1) # first SAItem
.jsa_name(jsa1)
#> [1] "x13"
mod1 <- .jsa_read(jsa1)
```


## Contributing

Any contribution is welcome and should be done through pull requests and/or issues.

## Licensing

The code of this project is licensed under the [European Union Public Licence (EUPL)](https://joinup.ec.europa.eu/page/eupl-text-11-12).
