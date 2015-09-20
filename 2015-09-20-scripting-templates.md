# Scripting templates
To make the work for research assistants within the lab I have provided a number of templates for standard tasks. All of this will be based on Hadley Wickham's Style Guide (http://adv-r.had.co.nz/Style.html).

## Project Structure
Each project needs to have the following folders:

* R (for storing functions being used more than once)
* analysis (for all analysis scripts)
* data (to store all data)
* output (to store output in the form of knitted `.md` and `.docx` files)
* figures (for `.tex` or `R` files generating figures)

The files in each folder are tied together by an `execute.R` script in the root folder which calls all other subscripts through `source()`.

## Relative paths
All working directories and specification of paths should be set using relative paths. This means that `setwd()` should always be set to the root directory in the git repository, and then other files should be accessed using a path relative to that root directory, i.e. `source("R/functions.R", chdir= TRUE)`. The reason for doing this is to ensure replicability across different machines.

## Loading/installing packages in a unitary way
The packages are loaded either at the top of the script or in the main `execute.R` script. In order to make sure that all necessary packages are installed, use the following code to load and install packages:

    required_packages <- c('TraMineR','TraMineRextras','magrittr', 'dplyr', 'rmarkdown', 'stringr', 'cluster', 'RColorBrewer', 'WeightedCluster', 'xlsx')
    new_packages <- required_packages[!(required_packages %in% installed.packages()[,'Package'])]
    if(length(new_packages)) install.packages(new_packages)
    lapply(required_packages, library, character.only = TRUE)

## Storing credentials in a safe way
Credentials (e.g OAUTH 2.0) are stored in the root folder in a script called `credentials.R` This script assigns strings to necessary variables such as `username` and `password`, like so:

    username <- "aronlindberg"
    password <- "a43294032bc832aed223"

In the `execute.R` script you can then call `source("credentials.R")`. To make sure that the credentials themselves are not shared the `.gitignore` needs to specify `credentials.R` so that the credentials themselves are never committed to the git repository.

## Knitr templates
All output should be compiled to a `.docx` document using the `knitr` package and `pandoc` (for converting `.md` files to `.docx` files). An example of how to do this can be seen below:

    knit("analysis/report.Rmd", output = "/output/report.md")
    system(paste0("pandoc -o ", "report", ".docx ", "report", ".md"))
    unlink("/output/report.md")

## Todo
* Visualization templates (ggplot2/ggvis)
* Figure templates (TikZ)
