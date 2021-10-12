---
categories:
- ""
- ""
date: "2017-10-31T21:28:43-05:00"
description: ""
draft: false
image: 
keywords: ""
slug: ipsum
title: "Preprogram R Project"
output:
 html_document:
   theme: flatly
   highlight: zenburn
   toc: yes
   toc_float: yes
 pdf_document:
   toc: yes
---



```{r load-libraries, warning=FALSE, message=FALSE, echo=FALSE}
library(tidyverse)  # Load ggplot2, dplyr, and all the other tidyverse packages
library(gapminder)  # gapminder dataset
library(here)
library(janitor)
```

The goal is to test your software installation, to demonstrate competency in Markdown, and in the basics of `ggplot`.

Please delete all the intro text I wrote from line 22 to line 69 and start writing your short biography after this blockquote.


# MY BIOGRAPHY

My name is Come de Germay and I am coming from France

![](Capture d’écran 2020-07-14 à 13.48.24.png)

I have been studying software engineering in the UK and in France for 4 years at Imperial College and Ecole Polytechnique (Paris, France).

After various internships in:

* Industry,
* Tech,
* Consulting,  
* Banking,
* and Private Equity

I have cofounded a startup [HOMNES Invest](http://homnesinvest.com) which aims at **democratizing real estate investment using blockchain**.

I am now joining *LBS' MFA* to improve my knowlegde in Finance. 



# Task 2: `gapminder` country comparison

You have seen the `gapminder` dataset that has data on life expectancy, population, and GDP per capita for 142 countries from 1952 to 2007. To get a glimpse of the dataframe, namely to see the variable names, variable types, etc., we use the `glimpse` function. We also want to have a look at the first 20 rows of data.

```{r}
glimpse(gapminder)

head(gapminder, 20) # look at the first 20 rows of the dataframe

```

Your task is to produce two graphs of how life expectancy has changed over the years for the `country` and the `continent` you come from.

I have created the `country_data` and `continent_data` with the code below.

```{r}
country_data <- gapminder %>% 
           filter(country == "France") # just choosing Greece, as this is where I come from

continent_data <- gapminder %>% 
           filter(continent == "Europe")
```

First, create a plot of life expectancy over time for the single country you chose. Map `year` on the x-axis, and `lifeExp` on the y-axis. You should also use `geom_point()` to see the actual data points and `geom_smooth(se = FALSE)` to plot the underlying trendlines. You need to remove the comments **\#** from the lines below for your code to run.

```{r, lifeExp_one_country}
plot1 <- ggplot(data = country_data, mapping = aes(x = year, y = lifeExp))+
  geom_point() +
  geom_smooth(se = FALSE)+
  NULL 
plot1
```

Next we need to add a title. Create a new plot, or extend plot1, using the `labs()` function to add an informative title to the plot.

```{r, lifeExp_one_country_with_label}
plot1<- plot1 +
  labs(title = "Life expectancy in France since 1950 ",
      x = "Year ",
      y = "Life expectancy ") +
  NULL


plot1
```

Secondly, produce a plot for all countries in the *continent* you come from. (Hint: map the `country` variable to the colour aesthetic. You also want to map `country` to the `group` aesthetic, so all points for each country are grouped together).

```{r lifeExp_one_continent}
ggplot(data=continent_data, mapping = aes(x = year, y = lifeExp , colour= country, group =country))+
  geom_point() + 
  geom_smooth(se = FALSE) +
  NULL
```

Finally, using the original `gapminder` data, produce a life expectancy over time graph, grouped (or faceted) by continent. We will remove all legends, adding the `theme(legend.position="none")` in the end of our ggplot.

```{r lifeExp_facet_by_continent}
ggplot(data = gapminder , mapping = aes(x = year, y = lifeExp , colour= continent))+
  geom_point() + 
  geom_smooth(se = FALSE) +
  facet_wrap(~continent) +
  theme(legend.position="none") + #remove all legends
  NULL
```

Given these trends, what can you say about life expectancy since 1952? Again, don't just say what's happening in the graph. Tell some sort of story and speculate about the differences in the patterns.

Type your answer after this blockquote.

The above graph display a common trend: the life expency has been increasing ever since 1952, this is coherent with literrature and past studies. However, we can witness differences between each continent growth rate and each continent starting and ending points. I would assume it is linked to their economic situation.

# Task 3: Brexit vote analysis

We will have a look at the results of the 2016 Brexit vote in the UK. First we read the data using `read_csv()` and have a quick glimpse at the data

```{r load_brexit_data, warning=FALSE, message=FALSE}
brexit_results <- read_csv(here::here("data","brexit_results.csv"))


glimpse(brexit_results)
```

The data comes from [Elliott Morris](https://www.thecrosstab.com/), who cleaned it and made it available through his [DataCamp class on analysing election and polling data in R](https://www.datacamp.com/courses/analyzing-election-and-polling-data-in-r).

Our main outcome variable (or y) is `leave_share`, which is the percent of votes cast in favour of Brexit, or leaving the EU. Each row is a UK [parliament constituency](https://en.wikipedia.org/wiki/United_Kingdom_Parliament_constituencies).

To get a sense of the spread, or distribution, of the data, we can plot a histogram, a density plot, and the empirical cumulative distribution function of the leave % in all constituencies.

```{r brexit_histogram, warning=FALSE, message=FALSE}

# histogram
plot2<- ggplot(brexit_results, aes(x = leave_share)) +
 geom_histogram(binwidth = 2.5)

plot2<- plot2 +
  labs(title = "Histogram of the people leaving the UK ",
      x = "Leaving percentage ",
      y = "Count") +
  NULL
plot2

# density plot-- think smoothed histogram
plot3<- ggplot(brexit_results, aes(x = leave_share)) +
 geom_density()

plot3<- plot3 +
  labs(title = "Density plot of the people leaving the UK ",
      x = "Leaving percentage ",
      y = "Density") +
  NULL
plot3

# The empirical cumulative distribution function (ECDF) 
plot4<- ggplot(brexit_results, aes(x = leave_share)) +
 stat_ecdf(geom = "step", pad = FALSE) +
 scale_y_continuous(labels = scales::percent)

plot4<- plot4 +
  labs(title = "Cummulative distribution plot of the people leaving the UK ",
      x = "Leaving percentage ",
      y = "Cummulative %") +
  NULL
plot4


```

One common explanation for the Brexit outcome was fear of immigration and opposition to the EU's more open border policy. We can check the relationship (or correlation) between the proportion of native born residents (`born_in_uk`) in a constituency and its `leave_share`. To do this, let us get the correlation between the two variables

```{r brexit_immigration_correlation}
brexit_results %>% 
 select(leave_share, born_in_uk) %>% 
 cor()
```

The correlation is almost 0.5, which shows that the two variables are positively correlated.

We can also create a scatterplot between these two variables using `geom_point`. We also add the best fit line, using `geom_smooth(method = "lm")`.

```{r brexit_immigration_plot}
plot5<- ggplot(brexit_results, aes(x = born_in_uk, y = leave_share)) +
 geom_point(alpha=0.3) +

 # add a smoothing line, and use method="lm" to get the best straight-line
 geom_smooth(method = "lm") + 

 # use a white background and frame the plot with a black box
 theme_bw() +
 NULL

plot5<- plot5 +
  labs(title = "Scatter plot between being born in the uk and wanting to leave the EU",
      x = "Born in the UK percentage ",
      y = "Leaving percentage ") +
  NULL
plot5
```

You have the code for the plots, I would like you to revisit all of them and use the `labs()` function to add an informative title, subtitle, and axes titles to all plots.

What can you say about the relationship shown above? Again, don't just say what's happening in the graph. Tell some sort of story and speculate about the differences in the patterns.

Type your answer after, and outside, this blockquote.

From the graph we can witness that there is a strong correlation between born in the UK % leaving %. This is an common behavior that is observable in most countries when discussing nationalism. Howver, although the trend tends to increase the spread at 100% born in the UK is high, indicating that this belief is not shared by all UK native citizens. 

# Task 4: Animal rescue incidents attended by the London Fire Brigade

[The London Fire Brigade](https://data.london.gov.uk/dataset/animal-rescue-incidents-attended-by-lfb) attends a range of non-fire incidents (which we call 'special services'). These 'special services' include assistance to animals that may be trapped or in distress. The data is provided from January 2009 and is updated monthly. A range of information is supplied for each incident including some location information (postcode, borough, ward), as well as the data/time of the incidents. We do not routinely record data about animal deaths or injuries.

Please note that any cost included is a notional cost calculated based on the length of time rounded up to the nearest hour spent by Pump, Aerial and FRU appliances at the incident and charged at the current Brigade hourly rate.

```{r load_animal_rescue_data, warning=FALSE, message=FALSE}

url <- "https://data.london.gov.uk/download/animal-rescue-incidents-attended-by-lfb/8a7d91c2-9aec-4bde-937a-3998f4717cd8/Animal%20Rescue%20incidents%20attended%20by%20LFB%20from%20Jan%202009.csv"

animal_rescue <- read_csv(url,
                         locale = locale(encoding = "CP1252")) %>% 
 janitor::clean_names()


glimpse(animal_rescue)
```
One of the more useful things one can do with any data set is quick counts, namely to see how many observations fall within one category. For instance, if we wanted to count the number of incidents by year, we would either use `group_by()... summarise()` or, simply [`count()`](https://dplyr.tidyverse.org/reference/count.html)

```{r, instances_by_calendar_year}

animal_rescue %>% 
 dplyr::group_by(cal_year) %>% 
 summarise(count=n())

animal_rescue %>% 
 count(cal_year, name="count")

```

Let us try to see how many incidents we have by animal group. Again, we can do this either using group_by() and summarise(), or by using count()

```{r, animal_group_percentages}
animal_rescue %>% 
 group_by(animal_group_parent) %>% 

 #group_by and summarise will produce a new column with the count in each animal group
 summarise(count = n()) %>% 

 # mutate adds a new column; here we calculate the percentage
 mutate(percent = round(100*count/sum(count),2)) %>% 

 # arrange() sorts the data by percent. Since the default sorting is min to max and we would like to see it sorted
 # in descending order (max to min), we use arrange(desc()) 
 arrange(desc(percent))


animal_rescue %>% 

 #count does the same thing as group_by and summarise
 # name = "count" will call the column with the counts "count" ( exciting, I know)
 # and 'sort=TRUE' will sort them from max to min
 count(animal_group_parent, name="count", sort=TRUE) %>% 
 mutate(percent = round(100*count/sum(count),2))


```

Do you see anything strange in these tables? 

Yes the strong difference between cats and the rest of the animals.They represent over half the total population.

Finally, let us have a loot at the notional cost for rescuing each of these animals. As the LFB says,

Please note that any cost included is a notional cost calculated based on the length of time rounded up to the nearest hour spent by Pump, Aerial and FRU appliances at the incident and charged at the current Brigade hourly rate.

There is two things we will do:

1. Calculate the mean and median `incident_notional_cost` for each `animal_group_parent`
2. Plot a boxplot to get a feel for the distribution of `incident_notional_cost` by `animal_group_parent`.


Before we go on, however, we need to fix `incident_notional_cost` as it is stored as a `chr`, or character, rather than a number.

```{r, parse_incident_cost,message=FALSE, warning=FALSE}

# what type is variable incident_notional_cost from dataframe `animal_rescue`
typeof(animal_rescue$incident_notional_cost)

# readr::parse_number() will convert any numerical values stored as characters into numbers
animal_rescue <- animal_rescue %>% 

 # we use mutate() to use the parse_number() function and overwrite the same variable
 mutate(incident_notional_cost = parse_number(incident_notional_cost))

# incident_notional_cost from dataframe `animal_rescue` is now 'double' or numeric
typeof(animal_rescue$incident_notional_cost)

```

Now tht incident_notional_cost is numeric, let us quickly calculate summary statistics for each animal group. 


```{r, stats_on_incident_cost,message=FALSE, warning=FALSE}

animal_rescue %>% 

 # group by animal_group_parent
 group_by(animal_group_parent) %>% 

 # filter resulting data, so each group has at least 6 observations
 filter(n()>6) %>% 

 # summarise() will collapse all values into 3 values: the mean, median, and count  
 # we use na.rm=TRUE to make sure we remove any NAs, or cases where we do not have the incident cos
 summarise(mean_incident_cost = mean (incident_notional_cost, na.rm=TRUE),
           median_incident_cost = median (incident_notional_cost, na.rm=TRUE),
           sd_incident_cost = sd (incident_notional_cost, na.rm=TRUE),
           min_incident_cost = min (incident_notional_cost, na.rm=TRUE),
           max_incident_cost = max (incident_notional_cost, na.rm=TRUE),
           count = n()) %>% 

 # sort the resulting data in descending order. You choose whether to sort by count or mean cost.
 arrange(mean_incident_cost)

```



Compare the mean and the median for each animal group. waht do you think this is telling us?
Anything else that stands out? Any outliers? 

Animals generate different costs, in addition the mean and medians are relatively different meaning that the distribution is spread. Regarding the outliers, I do not consider one stands out apart from Snake which tends to be higher.

Finally, let us plot a few plots that show the distribution of incident_cost for each animal group.

```{r, plots_on_incident_cost_by_animal_group,message=FALSE, warning=FALSE}

# base_plot
base_plot <- animal_rescue %>% 
 group_by(animal_group_parent) %>% 
 filter(n()>6) %>% 
 ggplot(aes(x=incident_notional_cost))+
 facet_wrap(~animal_group_parent, scales = "free")+
 theme_bw()

base_plot + geom_histogram()
base_plot + geom_density()
base_plot + geom_boxplot()
base_plot + stat_ecdf(geom = "step", pad = FALSE) +
 scale_y_continuous(labels = scales::percent)



```

Which of these four graphs do you think best communicates the variability of the `incident_notional_cost` values? Also, can you please tell some sort of story (which animals are more expensive to rescue than others, the spread of values) and speculate about the differences in the patterns.

The count/incident national cost per animal boxplot graph represents best the variability of the `incident_notional_cost` values as it give a good idea of the key distribution metrics (median, quartiles). Hence it gives the spread of incidents per costs and illustrates the potential differences between the quartiles and the median costs of incidents.

# Submit the assignment

Knit the completed R Markdown file as an HTML document (use the "Knit" button at the top of the script editor window) and upload it to Canvas.

## Details

If you want to, please answer the following

-   Who did you collaborate with: NONE
-   Approximately how much time did you spend on this problem set: 1H
-   What, if anything, gave you the most trouble: N/A