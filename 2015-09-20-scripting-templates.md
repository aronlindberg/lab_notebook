# Scripting templates
To make the work for research assistants within the lab I have provided a number of templates for standard tasks. All of this will be based on Hadley Wickham's Style Guide (http://adv-r.had.co.nz/Style.html).

## Project Structure
Each project needs to have the following folders:

* R (for storing functions being used more than once)
* analysis (for all analysis scripts)
* data (to store all data)
* output (to store output in the form of knitted `.md` and `.docx` files)
* figures (for `.tex` or `R` files generating figures)

The files in each folder are tied together by an `make.R` script in the root folder which calls all other subscripts through `source()`, along these lines:

    script_list <- list("static_cluster_validation.R", "routine_complexity.R", "temporal_cluster_validation.R",
    "file_graphs_munging_data.R", "social_complexity.R", "compiling_descriptive_stats.R", "correlation_matrix.R",
    "graphics.R", "analyzing_qualitative_coding.R", "statistical_tests.R")

    for (i in 1:length(script_list)){
      try(source(script_list[[i]]))
    }

## Relative paths
All working directories and specification of paths should be set using relative paths. This means that `setwd()` should always be set to the root directory in the git repository, and then other files should be accessed using a path relative to that root directory, i.e. `source("R/functions.R", chdir= TRUE)`. The reason for doing this is to ensure replicability across different machines.

## Loading/installing packages in a unitary way
The packages are loaded either at the top of the script or in the main `make.R` script. In order to make sure that all necessary packages are installed, use the following code to load and install packages:

    required_packages <- c('TraMineR','TraMineRextras','magrittr', 'dplyr', 'rmarkdown', 'stringr',
    'cluster', 'RColorBrewer', 'WeightedCluster', 'xlsx')
    new_packages <- required_packages[!(required_packages %in% installed.packages()[,'Package'])]
    if(length(new_packages)) install.packages(new_packages)
    lapply(required_packages, library, character.only = TRUE)

## Storing credentials in a safe way
Credentials (e.g OAUTH 2.0) are stored in the root folder in a script called `credentials.R` This script assigns strings to necessary variables such as `username` and `password`, like so:

    username <- "aronlindberg"
    password <- "a43294032bc832aed223"

In the `execute.R` script you can then call `source("credentials.R")`. To make sure that the credentials themselves are not shared the `.gitignore` needs to specify `credentials.R` so that the credentials themselves are never committed to the git repository.

## Dependency Management
To ensure that the code is reproducible across machines and time, we need to carefully specify which version of R and each package is used. This is done through a header which is essentially a abbreviated output from `sessionInfo()`, e.g.:

    # R version 3.2.1 (2015-06-18)
    # Platform: x86_64-apple-darwin13.4.0 (64-bit)
    # Running under: OS X 10.10.4 (Yosemite)
    # Packages: gdata_2.17.0, igraph_1.0.1, jsonlite_0.9.16, httpuv_1.3.3

A future version of this template set may use Docker or a similar environment for replicating the environment more fully.

## Knitr templates
All output should be compiled to a `.docx` document using the `knitr` package and `pandoc` (for converting `.md` files to `.docx` files). An example of how to do this can be seen below:

    knit("/analysis/report.Rmd", output = "/output/report.md")
    system(paste0("pandoc -o ", "report", ".docx ", "report", ".md"))
    unlink("/output/report.md")

## Todo
* Update relative path section with .gigitnore practice
* Practices for GitHub issues, pull requests, and commits (https://help.github.com/articles/closing-issues-via-commit-messages/)
* Stargazer templates for regression models and summary tables
* Visualization templates (ggplot2/ggvis)
