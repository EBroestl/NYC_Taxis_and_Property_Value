# Dashboard
https://public.tableau.com/views/PropertyValueandTravelPatterns/Dashboard2?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link

# Summary
----

Combining multiple datasets for NYC taxicab data to highlight trends in the frequency of trips over a multi-year period. Intent to juxtapose with housing prices, to determine if ride volume and frequency correlates with property values. Develop a time filterable dynamic dashboard. An exploration to see if Taxi volume has risen or fallen in the app era, and if ridership has been affected disproportionately in different property value areas.

## Data Questions
----

 - Does property value influence travel patterns?

 - Did rideshare services offset these patterns?

 - What areas continued service the longest, following Covid-19 restrictions?

## Data Challenges
----
#### Large Files
----
All files necessary to move forward answering my data questions totaled 263 GB. This is not an easily accessible amount of data to access inside a tableau dashboard. Nor is this an easy amount of data to process, as a single month of NYC Taxi data averaged 14 million rows, and I intend to use Lat/Long coordinates to group my data. Geospatial joins are easy enough to perform, however a single sheet loaded and processed would use 24 GB of system memory to address the full dataframe directly.

I attempted multiple coding solutions to process these flat-files to a more easily addressable size, with varying success.

Per month, using numpy matching with mathematical spatial formulas would take on average 3.8 hours, and geojoins would take on average 1.2 hours. My system was only utilizing 17% of my processing power, which lead to explorations into parallel processing within python. Unfortunately, python does not natively support parallel processing inside a windows environment. I researched virtual environments such as Docker, to load my code in a unix environment, and process the sheets, but this would not resolve memory usage issues, nor did I feel confident becoming adept at this technology in my given timeframe. Following, I attempted to utilize the Dask library, which does allow for parallel processing inside of Jupyter Notebooks on a windows machine, and in conjunction I was able to process and save a sheet to Parquet files only utilizing 24% of my system memory, and 80% of my CPU.

The results were less than impressive, however. Theoretically using this method should allow me to process 40 million rows per hour, though 28 million rows was taking my system 1.8 hours. It was extremely interesting to quickly implement these technologies quickly, and have them function with expected output, though their efficacy was a non-starter moving forward.

#### Spatial Files
----

NYC taxi data prior to 2016 has a high specificity geospatially. Following June 2016, this granularity is revised to be published within taxi zones, as opposed to Lat/Long coordinates, as to provide anonymity for taxi drivers. This consideration for the ethical publication of data is important, however presents a problem for the data questions.

NYC has:

 - 248 defined polygons in their published Zip Code shape file.

 - 263 defined polygons in their published Taxi Zones shape file.

Aside from the 15 duplicate matches we would expect, there are also many Zip Codes that fall in a single Taxi Zone. Converting these zones to centroids, and matching them using the Haversine formula, we find 183 distinct matches for Zip Codes within the Taxi Zones. This unified shape file will be used to join the Taxi and Property value data.

## Conclusions
----

 - There is a meaningful correlation between property value and volume of Taxi usage in Manhattan.

 - Rideshare services such as Uber and Lyft did indeed offset these correlations in later years.

 - Manhattan continued its usage of Taxi's the longest leading into Covid-19 lockdowns.

 - Staten Island, which has a higher property value on average, utilized private limo's and low volume ride share significantly more than any other borough during the time leading up to lockdowns.

## Data Sources
----

https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
https://www1.nyc.gov/site/finance/taxes/property-rolling-sales-data.page
