# Power Outages

**Author: Kailey Wen**

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
|CLIMATE.REGION| U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)|
|CLIMATE.CATEGORY|This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)|
|TOTAL.CUSTOMERS|Annual number of total customers served in the U.S. state|



## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before we can begin to answer the question, it is imperative that we clean the data appropriately. The data was originally stored in an excel file, so first I had to convert the file into a csv and delete any empty rows and columns.

The power outage start date and time were also stored in separate columns, `OUTAGE.START.DATE` and `OUTAGE.START.TIME`, respectively. It would make more sense to have this data combined so I converted the time and data column into one pd.TimeStamp column. I have this column named as `OUTAGE.START`. I then did same thing with the outage restoration time data as it was stored in a similar format in the column `OUTAGE.RESTORATION`

All of the numeric columns had values that were stored as strings in the original file (`OUTAGE.DURATION`, `YEAR`, `CUSTOMERS.AFFECTED`,  `DEMAND.LOSS.WM` and `TOTAL.CUSTOMERS`). So I converted these columns into integers to ensure that I could work with these columns further in the analysis.

After data cleaning, the first five of rows of the cleaned DataFrame looks like the following:

|   YEAR |   MONTH | U.S._STATE   | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   DEMAND.LOSS.MW | CLIMATE.REGION     |   TOTAL.CUSTOMERS |
|-------:|--------:|:-------------|:-------------------|:-------------------|:------------------------|:--------------------|:---------------------|------------------:|---------------------:|-----------------:|:-------------------|------------------:|
|   2011 |       7 | Minnesota    | normal             | severe weather     | nan                     | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |              3060 |                70000 |              nan | East North Central |           2595696 |
|   2014 |       5 | Minnesota    | normal             | intentional attack | vandalism               | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |                 1 |                  nan |              nan | East North Central |           2640737 |
|   2010 |      10 | Minnesota    | cold               | severe weather     | heavy wind              | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |              3000 |                70000 |              nan | East North Central |           2586905 |
|   2012 |       6 | Minnesota    | normal             | severe weather     | thunderstorm            | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |              2550 |                68200 |              nan | East North Central |           2606813 |
|   2015 |       7 | Minnesota    | warm               | severe weather     | nan                     | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |              1740 |               250000 |              250 | East North Central |           2673531 |               250000 |


### Univariate Analysis

With the data cleaned up, I went in to do some Exploratory Data Analysis. Specifically I first looked into the distribution of when outages occurred by doing some temporal analysis. Shown below is the distribution of outages for every year in our dataset.

<iframe
  src="assets/outage_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot above shows the number of power outages every year on the United States. We can see that in the early 2000s there was an upwards trend, leading to a huge spike in 2011. In the more recent years, there has been a general decrease.

I then decided to look into the relationship between geographical location and outage distribution. Below is a plot of the distribution of power outages across the United States.

<iframe
  src="assets/outage_dist_US.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that there are some states that have much higher counts of power outages in comparison to others. California has the highest overall with almost 200 outages. Having some knowledge about the typical climate can also help increase the relevancy of this plot to our investigation on how time, location and climate can impact the number of power outages. The two states with the highest number of outages are California and Texas; while local weather conditions can vary from year to year, both of these states are known for having some of the hottest summer in the United States.

### Bivariate Analysis
After looking at the distribution of outages across different variables, I wanted to see if there were any significant relationships between variables. My investigation was centered around the metric of the severity of an outage. So, naturally I wanted to look into the relationship between variables that could be used to measure the severity of a power outage: `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED`.

<iframe
  src="assets/duration_vs_Customers_Affected.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The above plot shows that the duration of an outage doesn't really have a strong relationship with the number of affected customers. It also shows that most of our data points cluster under 10,000 minutes (~7 days)


### Interesting Aggregates
Another question I had about the data was if there was any months in particular that there were more power outages. In the beginning of my investigation, I suspected that variables related to climate and location could possibly influence number of power outages. To answer these questions I decided to look at the distribution of outages across months to see if there is any correlation to power outages and a given month. First I created a pivot table to aggregate the power outages per month per year.
Shown below is distribution of outages across months.

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

I also used a heatmap to help visualize the relationship.

<iframe
  src="assets/heatmap.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

The heatmap and pivot table above shows that there isn't too high of a correlation between the month a power outage occurs and the frequency of power outage. However, it does show that there is a slight positive relationship with power outages and months in the summer. One explanation of this could be that people tend to use more electricity during the summer due to the heat. The plot also shows a general increase in power outage over the years which is something we saw in our previous bar plot as well. It also helps to reinforce the logic that power outages have increased over the years as a result of things like growing industrialization. 


## Assessment of Missingness

Before progressing in the investigation, I did an assessment of missingness to determine the mechanisms behind why some columns had missing values.

### NMAR Analysis

The `CUSTOMERS.AFFECTED` column has 443 missing values. I believe that the missingness mechanism is most likely NMAR. The reason behind this is that those collecting the data might be less likely to record or report the numbers of customers affected if that value was smaller.

By incorporating additional data sources or variables that explain the missingness mechanism, we can potentially make the missingness of the "Customers Affected" column missing at random (MAR) and address biases in the dataset. An additional column that indicates official outage severity metrics would be helpful in explaining the missingness in `CUSTOMERS.AFFECTED` If a specific outage was particularly insignificant in severity, this could explain why `CUSTOMERS.AFFECTED` might have missing values as less severe outages would be less important to report on. There are several columns in the dataset that can imply severity (duration, demand loss) but upon analysis those columns are not strongly correlated and can't serve as the sole determining factor of severity.

### Missingness Dependency

I noticed that in the dataset there were 471 missing values in the Cause Detail column. I came to the conclusion that the most likely column that could explain the missingness. To test the theory, I conducted a permutation test. 

**1. Cause Category Detail and Cause Category (MAR)**

Before beginning the test, I looked into the distributions of cause category with and without cause category detail. From the histogram below, we notice there distribution is very different. 

<iframe
  src="assets/mar_dist.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

Null Hypothesis: The missingness of cause category detail is not dependent on cause category

Alternative Hypothesis: The missingness of cause category detail is dependent on cause category

I first created a new column indicating the missingness status of the  cause category detail column, and shuffled this column for our permutation. Since cause category is categorical, I used the total variation distance as my test statistic.

<iframe
  src="assets/permutation.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

After conducting the permutation test, I found that the p-value was equal to 0. This tells me that the observed difference in distribution is unlikely to occur by random chance alone. Therefore, I have evidence to reject the null hypothesis that the missingness of `DETAIL.MISSING` is independent of `CAUSE.CATEGORY`

Based on this result, I can conclude that there is a significant association between the missingness of `DETAIL.MISSING` and the values of `CAUSE.CATEGORY` In other words, I can conclude that the reason for missingness in the `DETAIL.MISSING` column is highly likely to be dependent on the categories of the `CAUSE.CATEGORY` column. Thus, we can say that the missing mechanism of `DETAIL.MISSING` is MAR.



**2. Demand Loss (MW) and Outage Duration (MCAR)**
The '`DEMAND.LOSS.MW' column was another column that I saw have a significant number of missing values. Longer and more severe power outages can lead to higher demand loss. When power is unavailable for an extended period, consumers may resort to using alternative sources of energy or reduce their consumption, leading to a higher demand loss. Following a similar process as I did with the cause detail column, I conducted a permutation test to determine the missingness mechanism of demand loss to see if it was dependent on outage duration.

Null Hypothesis: The missingness of demand loss is not dependent on outage duration.

Alternative Hypothesis: The missingness of demand loss detail is dependent on outage duration.\

<iframe
  src="assets/permutation2.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>


After conducting a permutation test, I get a p-value that is greater than our significance level of 5%.  Since the p-value is relatively high, we fail to reject the null hypothesis. In other words, we do not have sufficient evidence to conclude that there is a significant difference in mean outage duration between instances with and without missing values in `DEMAND.LOSS.MW` Thus, I can conclude that it is highly probable that the missigness of `DEMAND.LOSS.MW` does not depend on `OUTAGE.DURATION`

## Hypothesis Testing

Going back to the topic of the investigation, I want to see if there is an increasing trend of power outages over months where there are higher temperatures. So far in our EDA, we have seen an increase in number of power outages during summer months, which tend to be the months that have higher temperatures. Thus, the next step in this investigation is to conduct a permutation text on the relationship between climate and power outage severity. While there is no official metric to measure power outage severity from this dataset, I will be using customers affected as an indicator of severity for the purposes of this investigation. 

Null Hypothesis: There is no difference in the severity of power outages during summer months compared to the severity across non-summer months. 

Alternative Hypothesis: The severity of power outages is higher during summer months compared to the overall severity across all months. 

Test Statistic: We can use the difference in mean number of affected customers between summer months and non-summer months as our test statistic, where the difference is measured as the mean number of affected customers during summer months minus the mean number of affected customers across all months.


In order to conduct this permutation test I had to first create a column that indicated whether the power outage occured during the summer. I labeled this columns `IS.SUMMER` Before I began my testing I decided to do some exploratory data analysis. We can see here that the average number of customers affected is very similar regardless of whether the power outage occured during a summer month or not.

| IS.SUMMER   |   mean |   count |
|:------------|-------:|--------:|
| False       | 144597 |     712 |
| True        | 141312 |     379 |

Next, I wanted to see the distributions visually so I generated the plot below. Once again, it was very clear that the distributions were very similar.

<iframe
  src="assets/summer_dist.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

Moving into the fun part of the investigation, the permutation test. I first shuffled the `CUSTOMERS.AFFECTED` for permutation. The test statistic I decided to use for this test was a difference in means for outages in the summer and outages that weren't in the summer. 

<iframe
  src="assets/summer_permutation.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

After conducting our test , I got a p-value of 0.542. This suggests that under the null hypothesis (that there is no difference in the mean number of affected customers between summer months and non-summer months)

We fail to reject the null hypothesis at conventional significance levels (5%). This means that there is insufficient evidence to conclude that the severity of power outages (measured by the mean number of affected customers) is higher during summer months compared to non-summer months.

In other words, based on the data and the permutation test, we do not have enough evidence to support the alternative hypothesis that the severity of power outages is higher during summer months.


## Framing a Prediction Problem

The main goal of this investigation is to develop analysis that can be useful in reducing the negative effects of major power outages. Similarly, access to a predictive model that can successfull predict the severity of a major power outage could be extremely useful for improving response and overall customer safety. In this next section, I will work towards creating an effective predictive model that can predict the severity in terms of a a response variable: the duration of a major power outage. The benefits of such a model are as follows:

- Power companies can provide their customers with information about the duration of how long an outage will occur for. This helps to enhance transparency and communication, leading to higher levels of customer satisfaction and trust.

- Individuals and households can better prepare for power outages by making arrangements for alternative energy sources, securing perishable goods, and planning for temporary accommodations, leading to less disruption and inconvenience.

- Utilities can allocate repair crews, equipment, and supplies more efficiently based on the forecasted severity of an outage, ensuring that resources are deployed where they are needed most and minimizing response times.

## Baseline Model

For the baseline model, I started with trying to predict `OUTAGE.DURATION` using multiple linear regression with the following features:
- `MONTH:`Outage duration may vary depending on the month due to seasonal factors such as weather conditions, holidays, or maintenance schedules. For example, winter months might experience longer outage durations due to snowstorms or colder temperatures affecting power lines.
- `CLIMATE.REGION:` Different climate regions experience different weather patterns, which can affect the frequency and severity of power outages. For instance, regions prone to hurricanes or thunderstorms may have longer outage durations compared to regions with milder climates.
- `CAUSE.CATEGORY:`The cause of the outage can provide valuable insights into its duration. For example, outages caused by severe weather events like storms or hurricanes might have longer durations compared to outages caused by equipment failures or maintenance.
- `TOTAL.CUSTOMERS:`The total number of customers affected by an outage can indicate the scale and severity of the event. Areas with higher population densities may experience longer outage durations due to the logistical challenges of restoring power to a larger number of customers.

For the baseline model I decided to use `sklearn` one hot encoding to transform my categorical variables. The RMSE values are relatively close to each other, suggesting that the baseline model is not overfitting the training data too severely. On the other hand, the r^2 tells me that while the model explains some of the variance in the outage duration, there is still a considerable amount of unexplained variability that the model doesn't account for, especially when considering the test data.

## Final Model

For my final model, I chose to use RandomForestRegressor, which is an ensemble learning method based on decision trees. Random forests combine multiple decision trees and average their predictions to improve generalization and reduce overfitting.

The hyperparameters I chose to tune were n_estimators and max_depth. n_estimators represents the number of trees in the forest, while max_depth controls the maximum depth of each tree. I used GridSearchCV to perform a grid search with 5-fold cross-validation. 

After performing hyperparameter tuning using GridSearchCV, the best hyperparameters found were n_estimators=250 and max_depth=4. This means that the best random forest model had 100 trees and each tree had a maximum depth of 4 levels.

The overall performance of my final model can be evaluated by comparing it to the baseline model and the values that both models got for RMSE and r^2. The RMSE was lower which indicates an improvement in the predictive performance of the model. The decrease in RMSE implies that the final model is making more accurate predictions compared to the baseline model. This reduction in error is beneficial because it means that the model is better capturing the underlying patterns and relationships in the data. Additionally the higher r^2 suggests that the final model explains a larger proportion of the variance in the target variable compared to the baseline model.

## Fairness Analysis

To perform a fairness analysis of my Final Model, we need to choose two groups to compare and an appropriate evaluation metric. Let's say  compare the performance of the model for states with high population density (Group X) versus states with low population density (Group Y), using RMSE as the evaluation metric.

Choice of Groups X and Y:

Group X: States with total customers above the median.

Group Y: States with total customers at or below the median.

Evaluation Metric:
RMSE (Root Mean Squared Error): This metric measures the average difference between the predicted and actual outage durations. Lower RMSE indicates better model performance.

Null Hypothesis: The model's RMSE for states with total customers above the median is  the same as its RMSE for states with total customers at or below the median. Any differences observed are due to random chance.

Alternative Hypothesis: The model's RMSE for states with total customers above the median is significantly different than its RMSE for states with total customers at or below the median, indicating potential unfairness.

Choice of Test Statistic:
The test statistic used is the difference in RMSE between states with total customers above the median and states with total customers at or below the median.

Significance Level:
The significance level will be set at 0.05. This means that we will reject the null hypothesis if the probability of observing the test statistic under the null hypothesis (p-value) is less than 0.05.


<iframe
  src="assets/RMSE_perm.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

With a significance level of 0.05 (typical significance level), a p-value of 0.021 is less than 0.05. Therefore, we reject the null hypothesis and conclude that there is a statistically significant difference in model performance between states with high and low total customers. In other words, the model's RMSE varies significantly between these two groups, suggesting potential fairness issues in the model's predictions across different levels of total customers.



