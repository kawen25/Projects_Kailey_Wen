# Introduction

The dataset contains major power outage data across the continental U.S. from January 2000 to 2016. Major outages are defined by the Department of Energy as outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW.

The main objective of this data science project is to investigate the characteristics of “severe” power outages. Power outages refer to the loss of electrical power supply to a specific area, region, or entire grid network. They can occur due to various reasons such as natural disasters, equipment failure, human error, or cyberattacks. As the world rapidly becomes more reliant on technology, the danger of power outages continues to become more and more relevant. In general, outages can pose numerous risks and dangers including disruption to services such as healthcare, communication, transportation and public safety systems. Understanding the characteristics of severe power outages is essential to mitigate risks, improve emergency response, enhance infrastructure resilience, inform policy decisions, and promote community preparedness.

The dataset used in this analysis comes from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure. The data was sourced from a collection of data published by: [1] the DOE’s Office of Electricity Delivery and Energy Reliability [2] the U.S. Energy Information Administration [3] National Oceanic and Atmospheric Administration [5] National Climatic Data Center [6] U.S. Department of Labor [7] U.S. Census Bureau. The dataset contains major power outage data across the continental U.S. from January 2000 to 2016. Major outages are defined by the Department of Energy as outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW.

The main question that I will be focusing on answering in this project is: What are the characteristics of major power outages with higher severity?

Variables of Interest:

|   Column    |Description |
|:------------|:--------    |
| YEAR        |Indicates the year when the outage event occurred|
| MONTH       |Indicates the month when the outage event occurred|    
| U.S._STATE  | Represents all the states in the continental U.S.|
| OUTAGE.DURATION|Duration of outage events (in minutes)|
| DEMAND.LOSS.MW|Amount of peak demand lost during an outage event (in Megawatt)|
| CUSTOMERS.AFFECTED|Number of customers affected by the power outage event|
| CAUSE.CATEGORY|Categories of all the events causing the major power outages|
| CAUSE.CATEGORY.DETAIL|Detailed description of the event categories causing the major power outages|

YEAR- Indicates the year when the outage event occurred
MONTH- Indicates the month when the outage event occurred
U.S._STATE- Represents all the states in the continental U.S.
OUTAGE.DURATION- Duration of outage events (in minutes)
DEMAND.LOSS.MW- Amount of peak demand lost during an outage event (in Megawatt)
CUSTOMERS.AFFECTED- Number of customers affected by the power outage event
CAUSE.CATEGORY- Categories of all the events causing the major power outages
CAUSE.CATEGORY.DETAIL- Detailed description of the event categories causing the major power outages
\
