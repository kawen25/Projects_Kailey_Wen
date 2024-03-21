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

## NMAR Analysis

Our 'CUSTOMERS.AFFECTED' column has 443 missing values. I believe that the missingness mechanism is most likely NMAR. The reason behind this is that those collecting the data might be less likely to record or report the numbers of customers affected if that value was smaller.

By incorporating additional data sources or variables that explain the missingness mechanism, we can potentially make the missingness of the "Customers Affected" column missing at random (MAR) and address biases in the dataset. An additional column that indicates official outage severity metrics would be helpful in explaining the missingness in 'CUSTOMERS.AFFECTED.' If a specific outage was particularly insignificant in severity, this could explain why 'CUSTOMERS.AFFECTED' might have missing values as less severe outages would be less important to report on. There are several columns in the dataset that can imply severity (duration, demand loss) but upon analysis those columns are not strongly correlated and can't serve as the sole determining factor of severity.

## Missingness Dependency

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