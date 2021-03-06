---
layout: post
title: An R Shiny App to Track Building Energy Data 
date:   2017-08-28 10:10:49
categories: climate
---

_Cross-posted from NYC Data Science Academy's Blog: [Original Post](https://blog.nycdatascience.com/student-works/r-shiny/building-energy-benchmarking-data-us/)_

_Source code for this application can be found on [github](https://github.com/afeierman/benchmarking-data-explorer)._

## Tracking Building Energy Data in the US

We don't know enough about how buildings use energy. Granted, we know some things: on a large scale, the US Department of Energy's Energy Information Administration releases [estimates of energy consumption](https://www.eia.gov/consumption/) at both the Federal and State level, while at a smaller scale, [energy modeling](https://energy.gov/eere/buildings/building-energy-modeling) can estimate the energy consumption of any individual building. And yet, given that buildings consume almost 75% of all electricity generated in the US and accounts for nearly 40% of all greenhouse gas (GHG) emissions in the country, we know far too little about how buildings consume energy.

About a decade ago, progressive cities and states in the US begun passing legislation that requires the owners of large buildings to measure and report building energy consumption on an annual basis.  Cities then, in most cases, compile and release these data on open data portals. My project analyzed some of this publicly available data, exploring trends in building energy consumption and building a proof-of-concept to see if building energy data across the country could be easily compiled into a central, easy to use repository.

Enter the Building Energy Dashboard:

![The Building Energy Dashboard](http://blog.nycdatascience.com/wp-content/uploads/2017/02/App1.png)

Ideally, buildings in the United States would be very energy efficient. Wasted energy is, in essence, wasted money. Efficient buildings don't waste energy. Having energy efficient buildings would save money for business and households alike, and reduce GHG emissions. And yet, buildings in the United States are not very efficient! Why are our buildings inefficient? It's complicated! Buildings range in size, climate, use, and occupancy. It's really difficult to create a [common standard of energy consumption](https://www.energystar.gov/buildings) for buildings to meet.



About 20 cities currently have regulations requiring building energy data transparency on the books, and roughly half of these cities have reached a point where they are releasing useable open data sets. Some cities have already used their energy transparency data to create awesome visualization tools-- shoutout to Philadelphia's Mayor's Office of Sustainability for their excellent [Building Benchmarking tool](http://visualization.phillybuildingbenchmarking.com/). However, most cities do not take the extra step to visualize their results, and instead release raw data through various means (some use open data portals, others dump excel files on an agency website).

For my project, I attempted a proof-of-concept to see if data from multiple cities could be brought together to create a centralized tool for energy transparency data. My project involved downloading, cleaning, and combining data from San Francisco, New York City, and Washington, DC.

### Does Location Influence Building Efficiency? Is Location a Proxy for Other Building Characteristics?

After combining energy data from each city into a [tidy format](http://vita.had.co.nz/papers/tidy-data.html), I used [leaflet](http://leafletjs.com/) to plot building energy data onto a map of each city. Building location was not always available, so I aggregated buildings by ZIP Code. Not having building location data did limit analysis somewhat, but still allowed for me to see if different neighborhoods had noticeable differences in energy consumption. Each ZIP Code is represented on the map by a circle whose color represents how much energy an average building in the area used, and whose size represents the number of buildings that reported in that ZIP Code during a given year.

It is likely that location does not cause buildings to be energy efficient or not. More likely is that buildings in a given neighborhood tend to cluster in their age, size, and use, all of which tend to influence building energy consumption. So, while the mapped portion of my tool was interesting to look through, it was most useful as a starting point to identify possible trends, which I would explore in another area of my app.

![ZIP Codes Appearing to Cluster by Energy Consumption in San Francisco](http://blog.nycdatascience.com/wp-content/uploads/2017/02/NY1.png)


### Can we identify building efficiency trends on a city-wide level?

Each year, an increasingly large number of cities are releasing data on building energy consumption. This growth of open data is generally positive-- however, this growth leads to a need for user-friendly tools to help people explore, understand, and identify trends within large data sets.

Using the second part of the app, the Data Explorer, users can visualize data themselves using custom parameters.

![A scatterplot of building size vs energy intensity. Each point is a building from either DC or San Francisco.](https://blog.nycdatascience.com/wp-content/uploads/2017/02/DataExplorer-768x375.png)

One issue I encountered when plotting these data (about 100,000 observations in total, across the 5 years of data collected from 3 cities) was that the data appeared "clumped." That is, while the data spanned a large set of values, the majority of the data fit within a smaller range of observations. When thinking about buildings, this is intuitive-- while there are some very large buildings (airports and skyscrapers) and big energy users (manufacturing facilities and data centers), most buildings fall into a small range of size and uses. To help identify trends among this clumped data, I added the ability to overlay a trendline for each city displayed in the data explorer:

![Trendlines aggregating data from DC and San Francisco](http://blog.nycdatascience.com/wp-content/uploads/2017/02/de2.png)

What can we tell from this? I have my ideas, but more importantly, the tool is set up for any user to come in and draw their own conclusions from the data.

### Some Other Features

Not all users will be interested in energy consumption data on a national scale. In fact, I anticipate that most users of a tool like this would be interested in getting information about one particular city (either as a resident, prospective investor, or policy maker). To this end, a user can use the "City Explorer" to get a high-level overview of energy disclosure and use within a given city:

![A City-Level View of Washington, DC](https://blog.nycdatascience.com/wp-content/uploads/2017/02/Screenshot-from-2017-02-05-10-56-01-1024x569.png)

This city-specific view can provide a number of unique views of city data. Let's focus on the [violin plot](http://www.datavizcatalogue.com/methods/violin_plot.html) in the bottom right-hand corner.

![A violin plot for Washington, DC. Hospitals have the greatest variety in energy intensity, while offices and educational buildings have more uniform energy consumption.](http://blog.nycdatascience.com/wp-content/uploads/2017/02/DCceViolin.png)

### How do different property types use data?

Using a violin plot, we can quickly see how buildings that have different uses (essentially, different property types) consume energy differently. A violin plot is similar to the more common box-and-whisker plot. On this plot, each building type is represented by a different color. The y-axis of this plot is energy use per square foot, which allows us to compare buildings of different sizes. Each value on the y-axis is measuring how much energy is used per square foot (a unit of size) within a building.

Let's focus on the red Education property type on the left. In a violin plot, the width of a shape represents how much data falls within a given range being measured. In this case, the red Education plot is very wide towards the bottom of the shape, at an energy use per square foot value (the y-axis) of about 125 kBtu/ft2. This part of the shape being very wide means that most educational buildings have similar energy use per square foot, so they are grouped together in the wide part of the plot. By contrast, the yellow Hospital shape shows that hospitals have greater variance in how much energy they use, as the yellow Hospital shape is long and narrow.

### What's Next?

Moving forward, this tool could be a starting point for an aggregated visualization platform for all 20+ cities who have passed energy benchmarking regulations across the country. Quite simply, buildings are complicated. Improving building efficiency is going to be an long process, requiring many actors and localized knowledge to drive actionable insights. In some cases, [great research on building efficiency data](https://www.researchgate.net/profile/Constantine_Kontokosta/publication/237818977_Energy_disclosure_market_behavior_and_the_building_data_ecosystem/links/57976d2208aec89db7b9a267.pdf) has already been done. However, as more cities release energy data in the coming years, it will be increasingly important for end-users to have an accessible platform to view building energy data. By enabling users see when, where, and how our buildings use energy, this tool can help people understand why and if their buildings are energy efficient.


Source code for this application can be found on [https://github.com/afeierman/benchmarking-data-explorer](github).
