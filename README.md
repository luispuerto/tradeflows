---
title: "Forest products trade flows database"
output:
  html_document:
    toc : true
---
This package prepares data for a forest products trade flow database.
It contains functions that automatically grab data from the 
COMTRADE and COMEXT databases, feed it through algorithms to compare 
unit values, to mirror export and import values.
Data will be delivered to an online user-interface.

Examples and demonstration:

* See example use in the doc/www folder.
* look in docs/development/countries for investigations on country specific issues

## Installation
You can install this package using the devtools package.
```
library(devtools)
install_bitbucket("paul4forest/tradeflows")
```

If the devtools package is not installed on your system.
```
install.packages("devtools")
```
You may need to upgrade the libcurl library on your system shell
```
sudo apt-get install libcurl4-gnutls-dev 
```

## Input
The function `loadcomtrade` and `loadcomtrade_bycode`, 
load data from the comtrade API in JSON format 
and convert it to data frames.

[Bug request of the Comtrade API](https://www.surveymonkey.com/r/?sm=2BQpdoHC1wFB3xIKcDja4TErpxm5%2b5nI5Iuz8et35wI%3d)
* submitted the 
jsonlite::fromJSON("http://comtrade.un.org/data/cache/classificationHS.json")
Warning message:
Unexpected Content-Type: application/x-javascript 

### Raw data
Inspired by the way hadley prepares this [flight planes data](https://github.com/hadley/nycflights13/tree/master/data-raw).
The package includes a training dataset:
sawnwood bilateral trade data for European countries.

## Output
Reports and data files.

### Templates
Templates are placed in inst/templates, 
[location inspired by the rapport package](https://github.com/Rapporter/rapport/tree/master/inst/templates).
See also their function [rapport.ls](https://github.com/Rapporter/rapport/blob/7b459b9733a44511b4884b6d35d25d743c7a11e1/R/rp_helpers.R) that lists templates.
And their function 
[rapport.read](https://github.com/Rapporter/rapport/blob/7b459b9733a44511b4884b6d35d25d743c7a11e1/R/template.R) that reads templates from files or form 
package-bundled templates.


## Tools
### This is a package
Created based on [instructions from Hadley](http://r-pkgs.had.co.nz/).
`devtools::load_all()` or __Cmd + Shift + L__, reloads all code in the package.
Add packages to the list of required packages
`devtools::use_package("dplyr")`
`devtools::use_package("ggplot2", "suggests")`
For data I followed his recommendations in r-pkgs/data.rmd
`devtools::use_data(mtcars)`
`devtools::use_data_raw()` # To create a data-raw/ folder and add it to .Rbuildignore

### Tests
Example of testing [for the devtools package](https://github.com/hadley/devtools/blob/master/tests/testthat/test-data.r)

### Data frame manipulation with dplyr
dplyr uses non standard evaluation. See vignette("nse") 
NSE is powered by the lazyeval package
```
# standard evaluation
sawnwood %>% select_(.dots = c("yr", "rtCode" )) %>% head
# is the same as
# lazy evaluation
sawnwood %>% select(yr, rtCode ) %>% head
```

### Error catching with tryCatch
see `demo(error.catching)`.


### Documentation using roxygen2
You should be able to see the documentation of exported functions by placing a 
question mark before the function name at the R command prompt.

inspired by the documentation of roxygenize
https://github.com/yihui/roxygen2/blob/master/R/roxygenize.R
`vignette("namespace", package = "roxygen2")` says:

> If you are using just a few functions from another package, the recommended option is to note the package name in the Imports: field of the DESCRIPTION file and call the function(s) explicitly using ::, e.g., pkg::fun(). Alternatively, though no longer recommended due to its poorer readability, use @importFrom, e.g., @importFrom pgk fun, and call the function(s) without ::.
> If you are using many functions from another package, use @import package to import them all and make available without using ::.

But Hadley says:

> Alternatively, if you are repeatedly using many functions from another package, you can import them in one command with @import package. This is the least recommended solution: it makes your code harder to read (because you can’t tell where a function is coming from), and if you @import many packages, the chance of a conflicting function names increases.

calling packages might have to be changed to follow Hadley's recommendations
on how package namespaces: http://r-pkgs.had.co.nz/namespace.html
see also vignette("namespace", package = "roxygen2")
require(RJSONIO)
require(dplyr)


### Version tracking system with git
The .git repository is backed on bitbucket.
Use devtools::install_bitbucket() to install the package.


### Shiny
A demonstration with time series plot and bar chart will be made
with shiny and the ggplot2 package, based on the diamond example using.


## Ongoing work

* [Oct 2014] Use a client side table sorter, 
such as the [JQuery tablesorter](http://tablesorter.com/docs/)
The plugin requires the thead and tbody tags, which are generated by Rmarkdown. 
Calling the javascript files should be possible within a YAML document, see 
[HTML document format](http://rmarkdown.rstudio.com/html_document_format.html).
    + I linked to the necessary javascript in docs/www/include/in_header.html
    + It basically works, CSS styles need to be added so that we can see little sorting arrows. See sample table in docs/www/index.html

### Work for programmers of the production system
* Automate data load from comtrade
    * Use loadcomtrade_bycode() as a function
    * Replace loadcomtrade_bycode() by a software that loads json files from comtrade, and places the content in a raw comtrade table in a SQL database
* Automate clean mechanism
    * Call R functions in clean.R from the server to clean the raw data
  from the database. Put cleaned data in the database.


### TODO by order of ease / importance
* use package options, inspired by the devtools or knitr package 
  "Devtools uses the following options to configure behaviour:..."
* Time plot of HS by quantity, weight or value data (ggplot gant chart) to 
visualise missing data
* load from EUROSTAT comext at 10 digit level
* Javascript visualisation: [Add googleVis and Rcharts to Markdown documents](http://al2na.github.io/Rmarkdown_JSviz/)

