# Data-Visualization-Project

# importing libraries -----------------------------------------------------

library(ggplot2)
library(dplyr)
library(ggthemes)


# Importing data ----------------------------------------------------------



df <- read.csv(file.choose())
head(df)

df <- select(df, c(Country, HDI.Rank, HDI, CPI, Region))
head(df)


# Visualization -----------------------------------------------------------

graph1 <- ggplot(df, aes(x = CPI, y = HDI, color = Region)) + geom_jitter()


# Changing the shape of points --------------------------------------------



graph1 <- ggplot(df, aes(x = CPI, y = HDI, color = Region)) + geom_jitter(shape = 1)

graph1


# giving it a trend line --------------------------------------------------


graph1 + geom_smooth(aes(group = 1))


# Upgrading a trend line --------------------------------------------------



## We want to further edit this trend line. Add the following arguments to geom_smooth (outside of aes):

##### method = 'lm'
##### formula = y ~ log(x)
##### se = FALSE
##### color = 'red'



graph2 <- graph1 + geom_smooth(aes(group = 1), method = 'lm', formula = y ~ log(x), se = FALSE, color = 'Red')



# Adding labels to the country --------------------------------------------

graph2 + geom_text(aes(label = Country))

##### as there is too much clutter in the graph, we will manually subset labels--------



points_to_label <- c("Russia", "Venezuela", "Iraq", "Myanmar", "Sudan",
                     "Afghanistan", "Congo", "Greece", "Argentina", "Brazil",
                     "India", "Italy", "China", "South Africa", "Spane",
                     "Botswana", "Cape Verde", "Bhutan", "Rwanda", "France",
                     "United States", "Germany", "Britain", "Barbados", "Norway", "Japan",
                     "New Zealand", "Singapore")

graph3 <- graph2 + geom_text(aes(label = Country), color = "gray20",
                             data = subset(df, Country %in% points_to_label),
                             check_overlap = TRUE)

graph3



# Adding theme to the graph -----------------------------------------------

graph4 <- graph3 + theme_bw()
graph4


# Editing X and Y axis labels ---------------------------------------------


graph5 <- graph4 + scale_y_continuous(name = "Human Development Index, 2011 (1 = Best)", 
                                      limits = c(0.2, 1.0))

graph6 <- graph5 + scale_x_continuous(name = "Corruption Perceptions Index, 2011 (10=least corrupt)", 
                                      limits = c(0.9, 10.5), breaks = 1:10)

graph6


# Adding 'Economist' theme ------------------------------------------------

graph6 <- graph6 + theme_economist_white()

graph6
