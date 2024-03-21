## Introduction

The dataset contains major power outage data across the continental U.S. from January 2000 to 2016. Major outages are defined by the Department of Energy as outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW.

The main objective of this data science project is to investigate the characteristics of “severe” power outages. Power outages refer to the loss of electrical power supply to a specific area, region, or entire grid network. They can occur due to various reasons such as natural disasters, equipment failure, human error, or cyberattacks. As the world rapidly becomes more reliant on technology, the danger of power outages continues to become more and more relevant. In general, outages can pose numerous risks and dangers including disruption to services such as healthcare, communication, transportation and public safety systems. Understanding the characteristics of severe power outages is essential to mitigate risks, improve emergency response, enhance infrastructure resilience, inform policy decisions, and promote community preparedness.

The dataset used in this analysis comes from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure. The data was sourced from a collection of data published by: [1] the DOE’s Office of Electricity Delivery and Energy Reliability [2] the U.S. Energy Information Administration [3] National Oceanic and Atmospheric Administration [5] National Climatic Data Center [6] U.S. Department of Labor [7] U.S. Census Bureau. The dataset contains major power outage data across the continental U.S. from January 2000 to 2016. Major outages are defined by the Department of Energy as outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW.

The main question that I will be focusing on answering in this project is: What are the characteristics of major power outages with higher severity?

**Variables of Interest:**

| **Column**|**Description**|
|:------------|:--------    |
| YEAR        |Indicates the year when the outage event occurred|
| MONTH       |Indicates the month when the outage event occurred|    
| U.S._STATE  | Represents all the states in the continental U.S.|
| OUTAGE.DURATION|Duration of outage events (in minutes)|
| DEMAND.LOSS.MW|Amount of peak demand lost during an outage event (in Megawatt)|
| CUSTOMERS.AFFECTED|Number of customers affected by the power outage event|
| CAUSE.CATEGORY|Categories of all the events causing the major power outages|
| CAUSE.CATEGORY.DETAIL|Detailed description of the event categories causing the major power outages|



## Data Cleaning and Exploratory Data Analysis

Before we can begin to answer the question, it is imperative that we clean the data appropriately. The data was originally stored in an excel file, so first I had to convert the file into a csv and delete any empty rows and columns.

The power outage start date and time are currently found in separate columns, 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME', respectively. It would make more sense to have this data combined so I converted the time and data column into one pd.TimeStamp column. I have this column named as 'OUTAGE.START'. I then did same thing with the outage restoration time data as it was stored in a similar format.

All of the numeric columns had values that were stored as strings in the original file ('OUTAGE.DURATION', 'YEAR', 'CUSTOMERS.AFFECTED', and 'DEMAND.LOSS.WMW'). So I converted these columns into integers to ensure that I could work with these columns further in the analysis.

After data cleaning, the first five of rows of the cleaned DataFrame looks like the following:

|   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND | OUTAGE.START        | OUTAGE.RESTORATION   |\n|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|:--------------------|:---------------------|\n|   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |         0.4112 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |\n|   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |         0.3748 |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |\n|   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |         0.3924 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |\n|   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |         0.4224 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |\n|   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |         0.367  |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |
# Univariate Analysis

Now that we have our data cleaned up, I want to do some Exploratory Data Analysis. Let's look at the distribution of when outages occurred by doing some temporal analysis. First let us look at the distribution of outages for every year in our dataset.

<iframe
  src="assets/outage_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot above shows the number of power outages every year on the United States. We can see that in the early 2000s there was an upwards trend, leading to a huge spike in 2011. In the more recent years, there has been a general decrease.

# Interesting Aggregates

Now let's look at the distribution of outages across months to see if there is any correlation to power outages and a given month. First we will create a pivot table to aggregate the power outages per month per year.
Now let's look at the distribution of outages across months to see if there is any correlation to power outages and a given month. First we will create a pivot table to aggregate the power outages per month per year.

|   YEAR |   January |   February |   March |   April |   May |   June |   July |   August |   September |   October |   November |   December |
|-------:|----------:|-----------:|--------:|--------:|------:|-------:|-------:|---------:|------------:|----------:|-----------:|-----------:|
|   2000 |         2 |          0 |       3 |       0 |     6 |      3 |      0 |        4 |           0 |         0 |          0 |          1 |
|   2001 |         2 |          0 |       6 |       0 |     3 |      3 |      0 |        1 |           0 |         0 |          0 |          0 |
|   2002 |         2 |          1 |       1 |       1 |     0 |      0 |      0 |        2 |           0 |         1 |          2 |          6 |
|   2003 |         1 |          1 |       0 |       4 |     2 |      1 |     11 |        7 |           5 |         1 |          7 |          6 |
|   2004 |         7 |          4 |       3 |       3 |    10 |      6 |      8 |        9 |          11 |         5 |          2 |          3 |
|   2005 |         5 |          0 |       1 |       3 |     2 |      8 |      7 |        9 |          11 |         3 |          4 |          2 |
|   2006 |         2 |         10 |       1 |       7 |     2 |      4 |     12 |        2 |           3 |         8 |          4 |         11 |
|   2007 |         2 |          4 |       1 |       6 |     6 |      4 |      7 |        8 |           7 |         6 |          0 |          5 |
|   2008 |         6 |         16 |       1 |       1 |     8 |     22 |     12 |        7 |          18 |         2 |          3 |         15 |
|   2009 |        14 |          6 |       5 |       7 |     3 |     12 |     11 |        8 |           0 |         5 |          1 |          6 |
|   2010 |         7 |          9 |       8 |       3 |     1 |     23 |     13 |       13 |           8 |         8 |          7 |          6 |
|   2011 |        14 |         21 |      17 |      21 |    30 |     27 |     35 |       45 |          19 |        19 |          6 |         15 |
|   2012 |        15 |         11 |      12 |      18 |     7 |     22 |     21 |       11 |           5 |        36 |          9 |          7 |
|   2013 |        10 |         12 |       8 |      10 |    17 |     21 |     15 |       19 |           3 |         7 |         18 |         13 |
|   2014 |        38 |         20 |      15 |       9 |    12 |     18 |      0 |        0 |           0 |         0 |          0 |          0 |
|   2015 |         4 |         12 |       8 |       9 |     8 |     18 |     16 |        8 |           4 |         8 |          9 |         15 |
|   2016 |         5 |          9 |      10 |       9 |    10 |      3 |     13 |        0 |           0 |         0 |          0 |          0 |

We can use a heatmap to help us visualize the relationship.

<iframe
  src="assets/heatmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## NMAR Analysis

Our 'CUSTOMERS.AFFECTED' column has 443 missing values. I believe that the missingness mechanism is most likely NMAR. The reason behind this is that those collecting the data might be less likely to record or report the numbers of customers affected if that value was smaller.

By incorporating additional data sources or variables that explain the missingness mechanism, we can potentially make the missingness of the "Customers Affected" column missing at random (MAR) and address biases in the dataset. An additional column that indicates official outage severity metrics would be helpful in explaining the missingness in 'CUSTOMERS.AFFECTED.' If a specific outage was particularly insignificant in severity, this could explain why 'CUSTOMERS.AFFECTED' might have missing values as less severe outages would be less important to report on. There are several columns in the dataset that can imply severity (duration, demand loss) but upon analysis those columns are not strongly correlated and can't serve as the sole determining factor of severity.

## Assessment of Missingness

### 1. Cause Category Detail and Cause Category (MAR)
Let's look at 'CAUSE.CATEGORY.DETAIL' (as it has 471 missing values) and see if its missing values depends on 'CAUSE.CATEGORY'
**Null Hypothesis:** The missingness of cause category detail is not dependent on cause category

**Alternative Hypothesis:** The missingness of cause category detail is dependent on cause category

After conducting the permutation test, I found that the p-value is equal to 0. This tells us that the observed difference in distribution is unlikely to occur by random chance alone. Therefore, I have evidence to reject the null hypothesis that the missingness of 'DETAIL.MISSING' is independent of 'CAUSE.CATEGORY'.

Based on this result, I can conclude that there is a significant association between the missingness of 'DETAIL.MISSING' and the values of 'CAUSE.CATEGORY'. In other words, I can conclude that the reason for missingness in the 'DETAIL.MISSING' column is highly likely to be dependent on the categories of the 'CAUSE.CATEGORY' column. Thus, we can say that the missing mechanism of 'DETAIL.MISSING' is MAR.



### 2. Demand Loss (MW) and Outage Duration (MCAR)

**Null Hypothesis:** The missingness of demand loss is not dependent on outage duration.

**Alternative Hypothesis:** The missingness of demand loss detail is dependent on outage duration.

After conducting a permutation test, I get a p-value that is greater than our significance level of 5%.  Since the p-value is relatively high, we fail to reject the null hypothesis. In other words, we do not have sufficient evidence to conclude that there is a significant difference in mean outage duration between instances with and without missing values in 'DEMAND.LOSS.MW'. Thus, I can conclude that it is highly probable that the missigness of 'DEMAND.LOSS.MW' does not depend on 'OUTAGE.DURATION'

## Hypothesis Testing