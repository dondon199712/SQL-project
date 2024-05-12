<h1>Churn Rates</h1>

<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#casestudyquestionsandsolutions">Case Study Questions & Solutions</a></li>


<h1><a name="introduction">Introduction</a></h1>
<p>Codeflix, a new streaming platform for learning videos, wants to understand how many users stop subscribing over time (churn rate). We'll be helping them answer these key questions:

1. Company Background: <br>
   - How long has Codeflix been around? <br>
   - For which months can we calculate churn? <br>
   - Are there different user groups (segments) on the platform? <br>

2. Churn Trend: <br>
   - How has the overall churn rate changed since launch? <br>

3. Segment Comparison: <br>
   - Do different user segments churn at different rates? <br>
  
4. Growth Focus: <br>
   - Based on churn, which user segment should Codeflix prioritize for growth?




<h1><a name="casestudyquestionsandsolutions"></a>Case Study Questions & Solutions</h1>

<h4><a name="database schema"></a>database schema</h4>

 <img width="500" src="https://github.com/dondon199712/SQL-project/blob/main/User%20Churn/database.png" alt="User Churn Database Schema">

<ol> 
  <li><h5> Which months will you be able to calculate churn for?</h5></li>

  ```sql
SELECT MIN(subscription_start)
FROM subscriptions;

SELECT MAX(subscription_start)
FROM subscriptions;
```


  <h6>From 2016-12-01 to 2017-03-30 </h6>

  
  <li><h5>

calculate the churn rate for both segments (87 and 30) over the first 3 months of 2017 <br>
</h5></li>
  
  ```sql
/* create a temporary table of months. */

WITH months AS (
SELECT 
'2017-01-01' as first_day, 
'2017-01-31' as last_day
UNION

SELECT 
'2017-02-01' as first_day,
'2017-02-28' as last_day
UNION

SELECT 
'2017-03-01' as first_day,
'2017-03-31' as last_day
) ,

/* Use month table to join subscriptions table */

cross_join AS (
SELECT *
FROM subscriptions
CROSS JOIN months
),

/* Create a temp_status */

status AS (
  SELECT
  id,
  first_day AS month,
  CASE 
    WHEN (subscription_start<first_day
) AND (subscription_end > first_day OR subscription_end IS NULL) AND (segment = 87)
THEN 1
ELSE 0
END AS is_active_87,
CASE 
    WHEN (subscription_start<first_day
) AND (subscription_end > first_day OR subscription_end IS NULL) AND (segment = 30)
THEN 1
ELSE 0
END AS is_active_30,
```

  <h6>
    is_active_87 created using a CASE WHEN to find any users from segment 87 
    who existed prior to the beginning of the month. This is 1 if true and 0 otherwise. same for segment 30 </h6>

    
    


  <li><h5> Add an is_canceled_87 and an is_canceled_30 column to temp_status </h5></li>

  ```sql
CASE
   WHEN (subscription_end BETWEEN first_day AND last_day) AND (segment=87 ) THEN 1
   ELSE 0
   END AS is_canceled_87,
   CASE
     WHEN (subscription_end BETWEEN first_day AND last_day) AND (segment=30 ) THEN 1
   ELSE 0
   END AS is_canceled_30
FROM cross_join
) ,
```



  <li><h5>Create a status_aggregate temporary table that is a SUM of the active and canceled subscriptions for each segment,
    for each month.</h5></li>

  ```sql
status_aggregate AS (
  SELECT 
  month,
  SUM(is_active_87) AS sum_active_87,
  SUM(is_active_30) AS sum_active_30,
  SUM(is_canceled_87) AS sum_canceled_87,
  SUM(is_canceled_30) AS sum_canceled_30
  FROM status
  GROUP BY month
)
```
   

  <li><h5>Calculate the churn rates for the two segments over the three month period.
    Which segment has a lower churn rate?</h5></li>

  ```sql

status_aggregate AS (
  SELECT 
  month,
  SUM(is_active_87) AS sum_active_87,
  SUM(is_active_30) AS sum_active_30,
  SUM(is_canceled_87) AS sum_canceled_87,
  SUM(is_canceled_30) AS sum_canceled_30
  FROM status
  GROUP BY month
)
-- calculate churn rate each month --

SELECT month,
1.0 * sum_canceled_87/sum_active_87 AS churn_rates_87,
1.0 * sum_canceled_30/sum_active_30 AS churn_rate_30
FROM status_aggregate;

```
 <p>Month with Lower Churn Rate:<br>

March 2017 (0.17) and January (0.25)has the lower churn rate compared to other months <br>

 In this case, March for segment_30  and Janauary for segment_87 have a lower percentage of users stopping their subscription compared to the rest. 
 This indicates that Codeflix was able to retain a higher proportion of its users in March for segment_30 and Janauary for segment_87.
</P>


