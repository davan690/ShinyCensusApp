## Output of Application is similar like this link: http://shiny.rstudio.com/tutorial/lesson5/

ShinyCensusApp
==============
counties.rds
counties.rds is a dataset of demographic data for each county in the United States, collected with the UScensus2010 R package. You can download it here.

Once you have the file,

Create a new folder named data in your census-app directory.
Move counties.rds into the data folder.

The dataset in counties.rds contains

the name of each county in the United States
the total population of the county
the percent of residents in the county who are white, black, hispanic, or asian
counties <- readRDS("census-app/data/counties.rds")
head(counties)

helpers.R
==========
helpers.R is an R script that can help you make choropleth maps, like the ones pictured above. A choropleth map is a map that uses color to display the regional variation of a variable. In our case, helpers.R will create percent_map, a function designed to map the data in counties.rds. You can download helpers.R here.

helpers.R uses the maps and mapproj packages in R. If you’ve never installed these packages before, you’ll need to do so before you make this app. Run

> install.packages(c("maps", "mapproj"))
Save helpers.R inside your census-app directory, 

The percent_map function in helpers.R takes five arguments:

Argument	Input
var	a column vector from the counties.rds dataset
color	any character string you see in the output of colors()
legend.title	A character string to use as the title of the plot’s legend
max	A parameter for controlling shade range (defaults to 100)
min	A parameter for controlling shade range (defaults to 0)
You can use percent_map at the command line to plot the counties data as a choropleth map, like this.

library(maps)
source("census-app/helpers.R")
counties <- readRDS("census-app/data/counties.rds")
percent_map(counties$white, "darkgreen", "% white")

Loading files and file paths
Take a look at the above code. To use percent_map, we first ran helpers.R with the source function, and then loaded counties.rds with the readRDS function. We also ran library(maps).

You will need to ask Shiny to call the same functions before it uses percent_map in your app, but how you write these functions will change. Both source and readRDS require a file path, and file paths do not behave the same way in a Shiny app as they do at the command line.

When Shiny runs the commands in server.R, it will treat all file paths as if they begin in the same directory as server.R. In other words, the directory that you save server.R in will become the working directory of your Shiny app.

Since you saved helpers.R in the same directory as server.R, you can ask Shiny to load it with

source("helpers.R")
Since you saved counties.rds in a sub-directory (named data) of the directory that server.R is in, you can load it with.

counties <- readRDS("data/counties.rds")
You can load the maps package in the normal way with

library(maps)
which does not require a file path.

Execution
Shiny will execute all of these commands if you place them in your server.R script. However, where you place them in server.R will determine how many times they are run (or re-run), which will in turn affect the performance of your app.

Shiny will run some sections of server.R more often than others.

Shiny will run the whole script the first time you call runApp. This causes Shiny to execute shinyServer. shinyServer then gives Shiny the unnamed function in its first argument.
