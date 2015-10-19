# NetApp

Brian McKean, a senior engineer at NetApp, gave a talk about his company in class.
He shared a data problem for our class to help solve.

# Tool
Tableau

# Authors

This report is prepared by
* [Karen Blakemore](https://github.com/kjblakemore)
* [Mingqi Liew](https://github.com/Malaokia)
* [Matt Schroeder](https://github.com/mattschroeder97)

# What percentage of the data points have time deltas of one day.

![screenshot](deltas.png)

In the chart, above, the time deltas have been grouped into bins of 10,000 second increments. The number of deltas in the 80K-90K bin, which is approximately one day is 551,124.  With a total of 1,070,436 lines in the .csv file, the percent of time deltas that are one day in length is 51.5% (551,124/1,070,040).

# Is there a correlation between Firmware Version and Time Deltas?

![screenshot](fwvers.png)

The chart above shows the distribution of delta times in 10,000 second increments over the firmware versions.  Firmware version 7.86.38.30 shows a distinct pattern of delta times that are less than 1 day (80K bin).

# What is the mean and standard deviation of the delta times for each system serial number?

https://public.tableau.com/profile/publish/Bigdata_Book/Sheet1#!/publish-confirm

![screenshot](mingqi.png)

The distribution of standard deviations is shown in the top chart; the means in the bottom chart.

# Which release had the most instances of delta times around 24 hours?

![screenshot](matt.png)

The chart above shows the distribution of delta times in 10,000 second increments over releases.

# Further Analysis

Our team determines the following questions are too complex for Tableau and
require custom scripts to be written.

* The development of [Time Maps](https://districtdatalabs.silvrback.com/time-maps-visualizing-discrete-events-across-many-timescales) might be to complex for Tableau.  Time Maps could be used to graph observations based on their time difference from the previous observation (x-axis) and the next observation (y-axis).  We already have the x-axis values.  These are the Delta Times.  We would need to calculate the y-axis values which are the deltas between the current Observation Time and the next for a particular system.

* Calculate the most frequently occurring invalid delta time that is in common across both controller types, A and B.