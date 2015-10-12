# NetApp

Brian McKean from NetApp presented to the class a data analysis problem he'd
like to get some help on.

# Overview

Listen to his presentation carefully. Write down the answers to the following
questions to show your team's understanding of the basics of the problem.

## What is the problem?
Each report contains a timestamp of when the data was gathered on the system. 
Adjacent terms in the sequence of timestamps for a particular system, should differ by 24 hours.
​
The problem is that the difference between adjacent terms in the timestamp sequences sometimes vary greatly from the 24 hour expected difference.The correct observation should should be contained under 1 day. Nevertheless,
the dataset provided day that is more than 1 day. So, which data points are consider
right or wrong, and which should be elimminated.

## Why is the problem importnat?
Data collected will be analyzed to minimize SSD cost, so if the data is not valid, then profit will go down. Only if we can obtained and analyzed data, can we improve the overall system and consequence reduce the cost. 

## What dataset has been made available?
Reports for the month of January 2015. ASUP dataset.

## What specific questions are being raised?
1. Is there enough valid data that can be used for the SSD wear-out analysis?
​
2. Are there any correlations in the data that could give insight into the cause of the invalid data?

# Q/A session

We will run a Q/A session. Before the session, compile a list of questions you
want to ask. Then, teams will take turn to ask Brian follow up questions.
Each team gets to ask one question each time. Write down the questions your team
wanted to ask and the answers you received. If another team happens to ask the
same question, simply write down the answer you heard.

## What is the expected disk write sequence over the life cycle of a system?
Answer: This is a followed up question which we are not considering for this dataset.


## What are the features of the data set?
Answer: * Date
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
import directly as .csv.

## How should the work be distributed among the team members?
Each team member will mine data via tableau, and try to search for insight of the erroneaus data.

## What types of charts or visualizations to use to support the answer?
Chart delta times to determine if there are enough valid observations to analyze the wear-out rate of SSDs.
​
Find correlations between delta times and other features in the data set, such as serial number, controller, release, FW & SW versions.

## What questions may be too complex for Tableau and may require custom scripts to be written?
Time Maps. It was used to graph events based on their time differences from previous observation and next observation. Although we have delta times, but to calculate differences between observation time being plotted and the next observation time is the sequence for the system might be too complex to accomplish.
