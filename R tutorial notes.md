
R Studio basics : 
ctrl + enter on line to run it
select then ctrl+enter to run a block


# Packages
Two types of packages : 
base : provided by default
community : to be downloaded for use

popular packages : 
dplyr : for manipulating data frames
tidyr : for cleaning up information 
stringr : for working with string data 
lubridate : for manipulating date information
httr : for working with website data
ggvis : gg-grammar of graphics, interactive visualizations
ggplot2 : most common package for data visualization 
shiny : create interactive applications that you can install on websites
rio : r input and output (importing and exporting data)
rmarkdown : create notebooks or rich documents 
pacman : package manager

standard installation comand : 
`install.packages("package name")`
Ex : 
```r
install.packages("pacman")
```


Then load the package by using either of the following:

```r
require(pacman)  # Gives a confirmation message.
library(pacman)  # No message.
```

```r
# Or, by using "pacman::p_load" you can use the p_load
# function from pacman without actually loading pacman.
# These are packages I load every time.
pacman::p_load(pacman, dplyr, GGally, ggplot2, ggthemes, 
  ggvis, httr, lubridate, plotly, rio, rmarkdown, shiny, 
  stringr, tidyr) 

library(datasets)  # Load/unload base packages manually

# CLEAN UP #################################################

# Clear packages
p_unload(dplyr, tidyr, stringr) # Clear specific packages
p_unload(all)  # Easier: clears all add-ons
detach("package:datasets", unload = TRUE)  # For base
```

Some commands : 
```r
head(iris) # displays the first  six rows of the iris dataset
<- # the assign operator
```

# plot()
- Basic X-Y plotting 
- for data visualization
- R automatically determines what type of visualization to use based on the data 

```r
?plot  # Help for plot()
```

```r
plot(iris$Species)  # Categorical variable shows inbar graph
plot(iris$Petal.Length) # Quantitative Variable shows in scatter plot

plot(iris$Species, iris$Petal.Width) # Categorical x quantitative
# displays in a scatter grph with a box plot basically combining both

plot(iris$Petal.Length, iris$Petal.Width) # Quantitative pair
# displaysin a scatter plot 

plot(iris) # Entire data frame with each species 
```

It is also possible to customize how the data loks

```r
# Plot with options
plot(iris$Petal.Length, iris$Petal.Width,
  col = "#cc0000",  # Hex code for datalab.cc red
  pch = 19,         # Use solid circles for points
  main = "Iris: Petal Length vs. Petal Width",
  xlab = "Petal Length",
  ylab = "Petal Width")
```
- col : color
- pch : point character (19->solid circle)
- main : title
- xlab : label on x axis
- ylab : label on y axis


### Plot formulas with plot()
Note : Plot method can do more than just display data 
We can feed it some formulas

`plot(cos, 0, 2* pi)` // displays a cosine curve 
- `cos` : cosine
- start from `0` till `2*p`i for the graph

```r
plot(exp, 1, 5)
plot(dnorm, -3, +3)
```

### Plot Formula with options 
```r
# Formula plot with options
plot(dnorm, -3, +3,
  col = "#cc0000",
  lwd = 5,
  main = "Standard Normal Distribution",
  xlab = "z-scores",
  ylab = "Density")
```

lwd : line width

## Bar Charts
Most basic graphic for the most basic data

```r
barplot(mtcars$cyl) # doesn't work it just shows all the values in graph 
```

we need to reform the data a little bit 
we need to create a summary table 

`cylinders <- table(mtcars$cyl)`
mtcars -> dataset
cyl -> variable 
cylinders -> object 
create a table of variable cyl and store it in cylinders object (or data container)

```r
barplot(cylinders) # Now it gives number of cars in the particular value for the variable with each bar
```

we can also use plot(cylinders) # for a line plot 

## Histogram
For data that is quantitative, scaled, measured, interval or ratio level
You can see what you have 
- Shape of the distribution 
- Gaps in the distribution 
- Outliers 
- Symmetry

```r
hist(dataset$column-name) # syntax
hist(iris$Sepal.Width) # example
```

change graphic parameter : makes it easier for comparing different values at a time
`par(mfrow = c(3,1))`
par -> parameter
mfrow -> multi frame row wise 
c- > concatenate (treat these numbers as one unit)
3- > number of rows
1 -> number of columns


```r
# Histograms for each species using options
hist(iris$Petal.Width [iris$Species == "setosa"],
  xlim = c(0, 3),
  breaks = 9,
  main = "Petal Width for Setosa",
  xlab = "",
  col = "red")
```

choosen data where species is setosa
xlim -> limit value for x
breaks -> how many bars you need in histogram 

Restore graphic Parameter : 
```r
par(mfrow = c(1,1))
```

whole code for histogram : 
```r
# File:   Histograms.R
# Course: R: An Introduction (with RStudio)

# LOAD PACKAGES ############################################

library(datasets)

# LOAD DATA ################################################

?iris
head(iris)

# BASIC HISTOGRAMS #########################################

hist(iris$Sepal.Length)
hist(iris$Sepal.Width)
hist(iris$Petal.Length)
hist(iris$Petal.Width)

# HISTOGRAM BY GROUP #######################################

# Put graphs in 3 rows and 1 column
par(mfrow = c(3, 1))

# Histograms for each species using options
hist(iris$Petal.Width [iris$Species == "setosa"],
  xlim = c(0, 3),
  breaks = 9,
  main = "Petal Width for Setosa",
  xlab = "",
  col = "red")

hist(iris$Petal.Width [iris$Species == "versicolor"],
  xlim = c(0, 3),
  breaks = 9,
  main = "Petal Width for Versicolor",
  xlab = "",
  col = "purple")

hist(iris$Petal.Width [iris$Species == "virginica"],
  xlim = c(0, 3),
  breaks = 9,
  main = "Petal Width for Virginica",
  xlab = "",
  col = "blue")

# Restore graphic parameter
par(mfrow=c(1, 1))

# CLEAN UP #################################################

# Clear packages
detach("package:datasets", unload = TRUE)  # For base

# Clear plots
dev.off()  # But only if there IS a plot

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

## ScatterPlots
Use scatterplot when you need to visualize the association between two quantitative variables
If you look for 
- if data is linear
- If there is consistent spread across dataset from one end to another
- Outliers
- Correlations between variables

```r
# Good to first check univariate distributions
hist(mtcars$wt)
hist(mtcars$mpg)

# Basic X-Y plot for two quantitative variables
plot(mtcars$wt, mtcars$mpg)

# Add some options
plot(mtcars$wt, mtcars$mpg,
  pch = 19,         # Solid circle
  cex = 1.5,        # Make 150% size
  col = "#cc0000",  # Red
  main = "MPG as a Function of Weight of Cars",
  xlab = "Weight (in 1000 pounds)",
  ylab = "MPG")
```


## Overlaying Plots 
Super-imposing one plot on top of another plot
Increases Information Density
Use views that support and complement each other 
Dont make the visualization overwhelming

To do that : 
`add = TRUE` add this property to the 2nd plot
```r
# Add a normal distribution
curve(dnorm(x, mean = mean(lynx), sd = sd(lynx)),
      col = "thistle4",  # Color of curve
      lwd = 2,           # Line width of 2 pixels
      add = TRUE)        # Superimpose on previous graph

```

##### kernel density estimators : 
- KDE is a non parametric way to estimate the probability density function (PDF) of a continuous random variable. 
- It provides a smooth curve that represents the distribution of the data, unlike histograms which are discrete.

```r
# Add two kernel density estimators
lines(density(lynx), col = "blue", lwd = 2)
lines(density(lynx, adjust = 3), col = "purple", lwd = 2)

```
density -> what to follow
adjust -> average across 

Rug plot : 
(provides more data idk how )
`rug(lynx, lwd = 2, col = "grey")`

whole code :
```r
# File:   OverlayingPlots.R
# Course: R: An Introduction (with RStudio)

# INSTALL AND LOAD PACKAGES ################################

library(datasets)  # Load/unload base packages manually

# LOAD DATA ################################################

# Annual Canadian Lynx trappings 1821-1934
?lynx
head(lynx)

# HISTOGRAM ################################################

# Default
hist(lynx)

# Add some options
hist(lynx,
     breaks = 14,          # "Suggests" 14 bins
     freq   = FALSE,       # Axis shows density, not freq.
     col    = "thistle1",  # Color for histogram
     main   = paste("Histogram of Annual Canadian Lynx",
                    "Trappings, 1821-1934"),
     xlab   = "Number of Lynx Trapped")

# Add a normal distribution
curve(dnorm(x, mean = mean(lynx), sd = sd(lynx)),
      col = "thistle4",  # Color of curve
      lwd = 2,           # Line width of 2 pixels
      add = TRUE)        # Superimpose on previous graph

# Add two kernel density estimators
lines(density(lynx), col = "blue", lwd = 2)
lines(density(lynx, adjust = 3), col = "purple", lwd = 2)

# Add a rug plot
rug(lynx, lwd = 2, col = "gray")

# CLEAN UP #################################################

# Clear packages
detach("package:datasets", unload = TRUE)  # For base

# Clear plots
dev.off()  # But only if there IS a plot

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)
```

## Summary()
After the basic visualizations of data 
we need to get some precise statistical info
Ex : 
- Counts for Categories
- Quartiles and mean for quantitative variables
- Etc...

summary(dataset)
- gives count of the each category for categorical variable
- gives quantitative values : min, 1st quartile, median, mean, 3rd quartile, max value
- for entire data frame : find all quantitative values of all quantitative values and counts for all categorical variables
```r
summary(iris$Species) # Categorical Variable 
summary(iris$Sepal.Length) # Quantitative Variable
summary(iris) # Entire data frame
```

### describe()
- Get more detail
- comes from psych package

you get **n, mean, standard deviation SD, median, 10% trimmed mean, Median Absolute Deviation MAD, min/max, range, skewness, kurtosis and Standard Error SD**

- This still comes after Graphical Summaries

Note : p_something is always from pacman

> [!attention]
> In R the function describe() is only used for quantitative variables only and not categorical variables.

Example : 
```r
describe(iris$Sepal.Length) # One Quantitative variable
describe(iris) # Entire Data set
```

full code : 
```r
# File:   Describe.R
# Course: R: An Introduction (with RStudio)

# INSTALL AND LOAD PACKAGES ################################

library(datasets)  # Load base packages manually

# Installs pacman ("package manager") if needed
if (!require("pacman")) install.packages("pacman")

# Use pacman to load add-on packages as desired
pacman::p_load(pacman, psych) 

# LOAD DATA ################################################

head(iris)

# PSYCH PACKAGE ############################################

# Get info on package
p_help(psych)           # Opens package PDF in browser
p_help(psych, web = F)  # Opens help in R Viewer

# DESCRIBE() ###############################################

# For quantitative variables only.

describe(iris$Sepal.Length)  # One quantitative variable
describe(iris)               # Entire data frame

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear packages
p_unload(all)  # Remove all add-ons
detach("package:datasets", unload = TRUE)   # For base

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

### Selecting Cases 
- Allows you to focus your analysis
- choose particular cases and look at them more closely
- You can select by category or by value or both.

Selecting using square brackets with conditions

We want to select cases by their category 
we will do this three times (for each species )
```r
# Versicolor
hist(iris$Petal.Length[iris$Species == "versicolor"],
	main = "Petal Length : Versicolor")

# Select when the species is versicolor and
# put title "petal length : versicolor"
```

```r
# Virginica
hist(iris$Petal.Length[iris$Species == "verginica"],
	main = "Petal Length : Verginica")

# Setosa
hist(iris$Petal.Length[iris$Species == "setosa"],
	main = "Petal Length : Setosa")
```

Selecting By Value 

```r
# short petals only 
hist(iris$Petal.Length[iris$Petal.Length < 2],
	main = "Petal Length less than 2")
```

```r
# Multiple Selectors
hist(iris$Petal.Length[iris$Species == "virginica" & 
						iris$Petal.Length < 5.5],
	main = "Petal Length : Short Virginica")
```

Creating Subsample
- If you know you need to use the sub sample multiple times 
- you can create a new data set with the sub sample
Format : `data[rows, columns]`
Leave rows or columns blank to select all
```r
i.setosa  <- iris[iris$Speciaes == "setosa", ] # setosa rows and all columns 
```

now we can explore the subsample : 
```r
head(i.setosa)
summary(i.setosa$Petal.Length)
hist(i.setosa$Petal.Length)
```


### Data Formats
- Important for Accessing Data 
- Data Types
- Data Structures

**Data types :** 
Numeric : integer, single & double
character
logical
complex
raw

**Data Structures :** 
vector : 1+ numbers in a 1d array, same data type, R's basic data object
Matrix/array :  two dimensions, same length columns, same data class, columns not named, array can have 3 or more dimensions
Data Frame  : Can have vectors of multiple types , all same length, closest R analogue to spreadsheet, R has special functions to work with data frames
List : Most flexible, Ordered collection of elements, Any class, length or structure, can include lists inside it, however somewhat hard to work with 

Coercion is good in Data Science
- Changing a data object from one type to another

```r
# File:   DataFormats.R
# Course: R: An Introduction (with RStudio)

# DATA TYPES ###############################################

# Numeric

n1 <- 15  # Double precision by default
n1 # by default stored in an array of size 1 [1] 15
typeof(n1) # "double"

n2 <- 1.5
n2 # [1] 1.5
typeof(n2) # "double"

# Character (single quote or double quotes)

c1 <- "c"
c1
typeof(c1)

c2 <- "a string of text"
c2 # size 1 array
typeof(c2) # "character" 

# Logical

l1 <- TRUE
l1
typeof(l1)

l2 <- F
l2
typeof(l2)

# DATA STRUCTURES ##########################################

## Vector ##################################################

v1 <- c(1, 2, 3, 4, 5) # c -> concatenate
v1
is.vector(v1)

v2 <- c("a", "b", "c") # vector of characters
v2
is.vector(v2)

v3 <- c(TRUE, TRUE, FALSE, FALSE, TRUE) # vector of logical values 
v3
is.vector(v3)

## Matrix ##################################################

m1 <- matrix(c(T, T, F, F, T, F), nrow = 2) # makes 2 rows with 3 automatic cols
m1

m2 <- matrix(c("a", "b", 
               "c", "d"), 
               nrow = 2,
               byrow = T)
m2

## Array ###################################################

# Give data, then dimemensions (rows, columns, tables)
# colon operator (:) -> give me numbers from 1 to 24
# c means concatenate 
a1 <- array(c( 1:24), c(4, 3, 2)) 
a1 

## Data frame ##############################################

# Can combine vectors of the same length

vNumeric   <- c(1, 2, 3) # vector of numeric values
vCharacter <- c("a", "b", "c") # vector of characters
vLogical   <- c(T, F, T) # vector of logical values

dfa <- cbind(vNumeric, vCharacter, vLogical) # cbind -> column bind
dfa  # Matrix of one data type
# just using cbind it changed all data to the most generic type char here
# to fix that use data frame

df <- as.data.frame(cbind(vNumeric, vCharacter, vLogical)) # specify data frame
df  # Makes a data frame with three different data types

## List ####################################################

o1 <- c(1, 2, 3) # object 1 or integer type
o2 <- c("a", "b", "c", "d")
o3 <- c(T, F, T, T, F)

list1 <- list(o1, o2, o3) # convert all objects into a list using list()
list1

list2 <- list(o1, o2, o3, list1)  # Lists within lists! 
list2

# COERCING TYPES ###########################################
# to automajtically convert a data value from one data type to another
## Automatic coercion ######################################

# Goes to "least restrictive" data type

(coerce1 <- c(1, "b", TRUE))
# coerce1  # Parenthese around command above make this moot
typeof(coerce1)

## Coerce numeric to integer ###############################

(coerce2 <- 5)
typeof(coerce2)

(coerce3 <- as.integer(5))
typeof(coerce3)

## Coerce character to numeric #############################

(coerce4 <- c("1", "2", "3"))
typeof(coerce4)

(coerce5 <- as.numeric(c("1", "2", "3")))
typeof(coerce5)

## Coerce matrix to data frame #############################

(coerce6 <- matrix(1:9, nrow= 3))
is.matrix(coerce6)

(coerce7 <- as.data.frame(matrix(1:9, nrow= 3)))
is.data.frame(coerce7)

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```


### Factors
- Categories and names of the categories 
- A factor is an attribute of a vector that specifies the possible values and their order.

```r
# File:   Factors.R
# Course: R: An Introduction (with RStudio)

# CREATE DATA ##############################################

(x1 <- 1:3)
(y  <- 1:9)

# Combine variables
(df1 <- cbind.data.frame(x1, y))
# since x1 has only 3 values it will be repeated 3 times to match y size in the data frame
typeof(df1$x1)
str(df1) # str() -> display internal structure of an object  

# AS.FACTOR ################################################

(x2  <- as.factor(c(1:3)))
(df2 <- cbind.data.frame(x2, y))
# at this point this still looks the same as above 
typeof(df2$x2) 
str(df2) # it shows instead of a simple integer x2 is a factor with 3 levels 

# DEFINE EXISTING VARIABLE AS FACTOR #######################

x3  <- c(1:3)
df3 <- cbind.data.frame(x3, y)
(df3$x3 <- factor(df3$x3,
  levels = c(1, 2, 3))) # define the levels of the factor manually
typeof(df3$x3)
str(df3)

# LABELS FOR FACTOR ########################################

x4  <- c(1:3)
df4 <- cbind.data.frame(x4, y)
df4$x4 <- factor(df4$x4,
  levels = c(1, 2, 3),
  labels = c("macOS", "Windows", "Linux"))
df4
typeof(df4$x4)
str(df4)

# ORDERED FACTORS AND LABELS ###############################

x5  <- c(1:3)
df5 <- cbind.data.frame(x5, y)
(df5$x5 <- ordered(df5$x5,
  levels = c(3, 1, 2),
  labels = c("No", "Maybe", "Yes")))
df5
typeof(df5$x5)
str(df5)

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

### Entering Data 
- Methods available for entering data  : colon, seq (sequence), c (concatenate), scan, rep

<- : used to assign values to a variable. 
This reads as  "gets" and is similar to equal to sign from other languages

```r
# File:   EnteringData.R
# Course: R: An Introduction (with RStudio)

# COLON OPERATOR ###########################################

# Assigns number 0 through 10 to x1
x1 <- 0:10 
x1

# Descending order
x2 <- 10:0
x2

# SEQ ######################################################

?seq  # R help on seq

# Ascending values (duplicates 1:10)
(x3 <- seq(10))

# Specify change in values
(x4 <- seq(30, 0, by = -3)) # by negative 3 is the step 

# ENTER MULTIPLE VALUES WITH C #############################

# c = concatenate (or combine or collect)
?c  # R help on c

x5 <- c(5, 4, 1, 6, 7, 2, 2, 3, 2, 8)
x5

# SCAN #####################################################

?scan  # R help on scan

x6 <- scan()  # After running this command, go to console
# Hit return after each number
# Hit return twice to stop
x6

# REP ######################################################
# to replicate or repeat elements 
?rep  # R help on rep
x7 <- rep(TRUE, 5) # repeat true 5 times
x7

# Repeats set
x8 <- rep(c(TRUE, FALSE), 5)
x8

# Repeats items in set
x9 <- rep(c(TRUE, FALSE), each = 5)
x9

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

### Importing Data 
- Getting data for analysis
- Get large amounts of data 
- get the data quickly 
- in order to process them 

Types of files : csv, txt, xlsx, json

- R has built in functions for importing data in many formats
- Or just use one : rio.
rio -> r input and output
- rio combines all of R's import functions into one simple utility

We do not need to specify the file for the function manually, it automatically recognizes it
file <- import(file address)

View(dataset object ) to view the data  
```r
# File:   ImportingData.R
# Course: R: An Introduction (with RStudio)

# INSTALL AND LOAD PACKAGES ################################

library(datasets)  # Load base packages manually

# Installs pacman ("package manager") if needed
if (!require("pacman")) install.packages("pacman")

# Use pacman to load add-on packages as desired
pacman::p_load(pacman, rio) 

# ABOUT EXCEL FILES ########################################

# From the official R documentation
browseURL("http://j.mp/2aFZUrJ")

# You have been warned: ಠ_ಠ

# IMPORTING WITH RIO #######################################

# CSV
rio_csv <- import("ImportingData_Datasets/mbb.csv")
head(rio_csv)

# TXT
rio_txt <- import("ImportingData_Datasets/mbb.txt")
head(rio_txt)

# Excel XLSX
rio_xlsx <- import("ImportingData_Datasets/mbb.xlsx")
head(rio_xlsx)

# DATA VIEWER ##############################################

?View
View(rio_csv)

# READ.TABLE FOR TXT FILES #################################

# R's built-in function for text files (used by rio)

# TEXT FILES

# Load a spreadsheet that has been saved as tab-delimited
# text file. Need to give complete address to file. This
# command gives an error on missing data but works on
# complete data.
r_txt1 <- read.table("ImportingData_Datasets/mbb.txt", header = TRUE)

# This works with missing data by specifying the separator: 
# \t is for tabs, sep = "," for commas. R converts missing
# to "NA"
r_txt2 <- read.table("ImportingData_Datasets/mbb.txt", 
  header = TRUE, 
  sep = "\t") # specify the separator manually

# READ.CSV FOR CSV FILES ###################################

# R's built-in function for csv files (also used by rio)

# CSV FILES
# Don't have to specify delimiters for missing data
# because CSV means "comma separated values"
trends.csv <- read.csv("ImportingData_Datasets/mbb.csv", header = TRUE)

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear packages
p_unload(all)  # Remove all add-ons

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

### Modeling Data  
Overview Only
Hierarchical Clustering -> Like with Like 
- But it depends on your criteria :
- hierarchical vs Set k
- Measures of Distance
- Divicive vs Agglomerative

Here we will do the most simple clustering : Euclidean distance, Hierarchical  clustering and Divisive Method

Pipes : 
allows us to take output of one step and make it the input for another step
"%>%" is the pipe operator

- **`rect.hclust(hc, ...)`**: This function highlights clusters in a hierarchical clustering dendrogram created using `hclust()`. It draws rectangles around the identified clusters.
- k= 2 : specifies the number of clusters to highlight

```r
# File:   HierarchicalClustering.R
# Course: R: An Introduction (with RStudio)

# INSTALL AND LOAD PACKAGES ################################

library(datasets)  # Load base packages manually

# Installs pacman ("package manager") if needed
if (!require("pacman")) install.packages("pacman")

# Use pacman to load add-on packages as desired
pacman::p_load(pacman, tidyverse) 

# LOAD DATA ################################################

?mtcars
head(mtcars)
cars <- mtcars[, c(1:4, 6:7, 9:11)]  # Select variables
head(cars)

# COMPUTE AND PLOT CLUSTERS ################################

# Save hierarchical clustering to "hc." This codes uses
# pipes from dplyr.
hc <- cars   %>%  # Get cars data
      dist   %>%  # Compute distance/dissimilarity matrix
      hclust      # Computer hierarchical clusters
  
plot(hc)          # Plot dendrogram

# ADD BOXES TO PLOT ########################################

rect.hclust(hc, k = 2, border = "gray")
rect.hclust(hc, k = 3, border = "blue")
rect.hclust(hc, k = 4, border = "green4")
rect.hclust(hc, k = 5, border = "darkred")

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear packages
p_unload(all)  # Remove all add-ons
detach("package:datasets", unload = TRUE)  # For base

# Clear plots
dev.off()  # But only if there IS a plot

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```

### Principal Components 
"Less Noise and fewer unhelpful variables in data = more meaning"
- This is also called dimensionality reduction 

PCA : Principle Component Analysis
Example : 
- We have a 2 variables graph and plot the points
- Apply regression and obtain the regression line
- Find perpendicular distance of each point from the regression line
- Collapse the points onto the regression line 
- Rotate the line to make it 1 dimensional 

We went from 2d to 1d data but maintained the most important information in the dataset.

```r
# File:    R01_6_2_PrincipalComponents.R
# Course:  R01: R: An introduction
# Chapter: 6: Modeling data
# Section: 2: Principal components
# Author:  Barton Poulson, datalab.cc, @bartonpoulson
# Date:    2016-08-04

# INSTALL AND LOAD PACKAGES ################################

# Packages I load every time; uses "pacman"
pacman::p_load(pacman, dplyr, GGally, ggplot2, ggthemes, 
  ggvis, httr, lubridate, plotly, rio, rmarkdown, shiny, 
  stringr, tidyr) 

library(datasets)  # Load base packages manually

# LOAD DATA ################################################

head(mtcars)
cars <- mtcars[, c(1:4, 6:7, 9:11)]  # Select variables
head(cars)

# COMPUTE PCA ##############################################

# For entire data frame ####################################
pc <- prcomp(cars,
        center = TRUE,  # Centers means to 0 (optional)
        scale = TRUE)   # Sets unit variance (helpful)

# To specify variables #####################################

pc <- prcomp(~ mpg + cyl + disp + hp + wt + qsec + am +
        gear + carb, 
        data = mtcars, 
        center = TRUE,
        scale = TRUE)

# EXAMINE RESULTS ##########################################

# Get summary stats
summary(pc)

# Screeplot for number of components
plot(pc)

# Get standard deviations and rotation
pc

# See how cases load on PCs
predict(pc) %>% round(2)

# Biplot of first two components
biplot(pc)

# CLEAN UP #################################################

# Clear environment
rm(list = ls()) 

# Clear packages
p_unload(all)  # Remove all add-ons
detach("package:datasets", unload = TRUE)  # For base

# Clear plots
dev.off()  # But only if there IS a plot

# Clear console
cat("\014")  # ctrl+L

# Clear mind :)

```