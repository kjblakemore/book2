# NetApp

Brian McKean from NetApp presented to the class a data analysis problem he'd
like to get some help on.

# Overview

Listen to his presentation carefully. Write down the answers to the following
questions to show your team's understanding of the basics of the problem.

## What is the problem?
NetApp minimizes the cost of the SSDs in their E-Series SAN storage systems, by using the least expensive SSDs that can survive a 5 year life span with < .5% wear out rate. As a measure of the wear out rate of their SSDs, NetApp keeps track of the number of disk operations which result in writes to the hard drives.  Daily reports of this measurement are sent to NetApp.

Each report contains a timestamp of when the data was gathered on the system. 
Adjacent terms in the sequence of timestamps for a particular system, should differ by 24 hours.

The problem is that the difference between adjacent terms in the timestamp sequences sometimes vary greatly from the 24 hour expected difference.

## Why is the problem important?
The problem is important, because the data can not be used to minimize the SSD costs, if it is not valid.

## What dataset has been made available?
Reports for the month of January 2015.

## What specific questions are being raised?
1. Is there enough valid data that can be used for the SSD wear-out analysis?

2. Are there any correlations in the data that could give insight into the cause of the invalid data?

# Q/A session

We will run a Q/A session. Before the session, compile a list of questions you
want to ask. Then, teams will take turns asking Brian follow up questions.
Each team gets to ask one question each time. Write down the questions your team
wanted to ask and the answers you received. If another team happens to ask the
same question, simply write down the answer you heard.

## What is causing the incorrect data?

Some of the base times are incorrect.  Maybe a timzone difference.  Systems could be offline during the time period.  There could be process or code errors.

## What is the expected disk write sequence over the life cycle of a system?

This is not a consideration for the data set that we are considering.

## What are the features of the data set?

* Date
* System Serial Number
* Controller - A or B
* Observation Time
* Base Time - time of previous observation.
* Delta Time - difference between Observation Time and Base Time
* Release
* Firmware Version
* Software Version

# Approach

Based on the information you've obtained during the Q/A session, come up with
plan how your team will tackle this problem.

## How should the dataset be imported into Tableau?
Load the .csv file directly into Tableau.

## How should the work be distributed among the team members?
Each team member will mine the data with tableau, looking for correlations which give insight into the erroneous data.

## What types of charts or visualizations to use to support the answer?
Chart delta times to determine if there are enough valid observations to analyze the wear-out rate of SSDs.

Find correlations between delta times and other features in the data set, such as serial number, controller, release, FW & SW versions.


## What questions may be too complex for Tableau and may require custom scripts to be written?
The development of [Time Maps](https://districtdatalabs.silvrback.com/time-maps-visualizing-discrete-events-across-many-timescales) might be to complex for Tableau.  Time Maps could be used to graph observations based on their time difference from the previous observation (x-axis) and the next observation (y-axis).  We already have the x-axis values.  These are the Delta Times.  We would need to calculate the y-axis values which are the deltas between the current Observation Time and the next for a particular system.
