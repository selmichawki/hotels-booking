# hotels-dataset




## Loading packages

Installing packages 

```{r install packages}
install.packages("tidyverse")
install.packages("skimr")
install.packages("janitor")
```

Once a package is installed, we load packages

```r 
library(tidyverse)
library(skimr)
library(janitor)
```

## Importing data

The data is originally from the article Hotel Booking Demand Datasets (https://www.sciencedirect.com/science/article/pii/S2352340918315191), written by Nuno Antonio, Ana Almeida, and Luis Nunes for Data in Brief, Volume 22, February 2019.

You can learn more about the dataset here:
https://www.kaggle.com/jessemostipak/hotel-booking-demand

Creating a data set `hotel_bookings`
```{r load dataset}
hotel_bookings <- read_csv("hotel_bookings.csv")
```

## Getting to know the data

We use the `head()` function to preview the columns and the first several rows of data.

```{r head function}
head(hotel_bookings)
```
 Getting summaries of each column in data arranged horizontally.

```{r str function}
str(hotel_bookings)
```

```{r glimpse function}
glimpse(hotel_bookings)
```

```{r colnames function}
colnames(hotel_bookings)
```

## Manipulating data and visualization

We arrange the data by most lead time to least lead time because.

```{r arrange function descending} 
arrange(hotel_bookings, desc(lead_time))
```
We store this into new dataframe named 'hotel_bookings_v2'

```{r new dataframe}
hotel_bookings_v2 <-
  arrange(hotel_bookings, desc(lead_time))
```

Checking out the new data frame: 

```{r new dataframe part two}
head(hotel_bookings_v2)
```

We find out the maximum and minimum lead times.

```{r}
max(hotel_bookings$lead_time)
```

```{r}
min(hotel_bookings$lead_time)
```
Getting the average of lead time 

```{r mean}
mean(hotel_bookings$lead_time)
```


Installing and load the 'ggplot2' package
.

```{r loading and installing ggplot2, echo=FALSE, message=FALSE}
install.packages('ggplot2')
library(ggplot2)
```

We determine if people with children book hotel rooms in advance.
```{r creating a plot}
ggplot(data = hotel_bookings) +
  geom_point(mapping = aes(x = lead_time, y = children))
```

```{r}
ggplot(data = hotel_bookings) +
 geom_point(mapping = aes(x = stays_in_weekend_nights, y = children))
```
Creating new data set
```{r filter}
hotel_bookings_city <- 
  filter(hotel_bookings, hotel_bookings$hotel=="City Hotel")
```

Check out the new data set

```{r new dataset}
head(hotel_bookings_city)
```

Checking the average lead time for this set of hotels 

```{r average lead time city hotels}
mean(hotel_bookings_city$lead_time)
```

Checking the mean, min and max lead time 

```{r group and summarize}
hotel_summary <- 
  hotel_bookings %>%
  group_by(hotel) %>%
  summarise(average_lead_time=mean(lead_time),
            min_lead_time=min(lead_time),
            max_lead_time=max(lead_time))
```

Checking the new data set 

```{r}
head(hotel_summary)
```

Making a Bar Chart to know if the number of bookings for each distribution type is different depending on whether or not there was a deposit or what market segment they represent. 

```{r pressure, echo=FALSE}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = distribution_channel, fill=deposit_type))
```
Creating separate charts for each deposit type and market segment to understand the difference.

```{r creating a plot}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = distribution_channel)) +
  facet_wrap(~deposit_type)
```
Creating a different chart for each market segment:
```{r creating a plot}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = distribution_channel)) +
  facet_grid(~market_segment)
```

Create a bar chart showing each hotel type and market segment.

```{r bar chart}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = hotel, fill = market_segment))
```

Creating a separate plot for each market segment: 

```{r faceting a plot}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = hotel)) +
  facet_wrap(~market_segment)
```

Creating a data set `onlineta_city_hotels`

```{r filtering a dataset to just city hotels that are online TA}
onlineta_city_hotels <- filter(hotel_bookings, 
                           (hotel=="City Hotel" & 
                             hotel_bookings$market_segment=="Online TA"))
```

Checking out the new data set 

```{r View}
View(onlineta_city_hotels)
```

Creating new data set `onlineta_city_hotels_v2`:

```{r filtering a dataset with the pipe}
onlineta_city_hotels_v2 <- hotel_bookings %>%
  filter(hotel=="City Hotel") %>%
  filter(market_segment=="Online TA")
```

Checking out the new data set 

```{r view second dataframe}
View(onlineta_city_hotels_v2)
```
Comparison of market segments by hotel type for hotel bookings

```{r faceting a plot with a title}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = market_segment)) +
  facet_wrap(~hotel) +
  labs(title="Comparison of market segments by hotel type for hotel bookings")
```

Determine the time period the data covers. 

```{r latest date}
mindate <- min(hotel_bookings$arrival_date_year)
maxdate <- max(hotel_bookings$arrival_date_year)
```

City bar chart with timeframe

```{r city bar chart with timeframe}
ggplot(data = hotel_bookings) +
  geom_bar(mapping = aes(x = market_segment)) +
  facet_wrap(~hotel) +
  labs(title="Comparison of market segments by hotel type for hotel bookings",
       caption=paste0("Data from: ", mindate, " to ", maxdate),
       x="Market Segment",
       y="Number of Bookings")
```

Saving chart 

```{r save your plot}
ggsave('hotel_booking_chart.png',
       width=16,
       height=8)
```
