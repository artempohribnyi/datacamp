# 1. Date/time components and aggregation

As with numerical and character data, sometimes we need to extract components of a date/time, or truncate the value, to aggregate the data in a meaningful way.

# 2. Common date/time fields

Functions exist to extract individual components of date/time data. These components are called fields. The fields are defined in the Postgres documentation. Many are based on the ISO 8601 standard. Let's look at some common fields starting with the largest unit of time. First, we can get the century or decade that a timestamp belongs in. January 1st, 2019 is in century 21 and decade 201. Date/time field definitions can be complicated and sometimes counterintuitive. It's always a good idea to read the documentation before using unfamiliar fields. Next, we can get the year, month, and day fields that make up a date. We can also get the hour, minute, and second fields that make up a time. Week is the week number in the year, based on the ISO 8601 definition. D-O-W is day of week. The week starts with Sunday, which has a value of 0, and ends on Saturday with a value of 6.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/f31821de-8623-49fc-b138-6143b0a7fbac)

# 3. Extracting fields

To extract these fields from a date or timestamp, you can use the date_part or extract functions. These functions expect a timestamp, but they automatically convert dates to timestamps. These two functions give the same output. They just have different syntax. date_part uses a comma to separate arguments, while extract uses the "from" keyword. The name of the field should be surrounded by single quotes when using the date_part function. With the extract function, the field name can be unquoted. Finally, the extract function, field name, and FROM keyword are typically written in all uppercase, while the date_part function is written in lowercase like other functions. The extract function actually calls the date_part function. You can see this in the output of the example query: the result of both the call to date_part and the call to extract are labeled with date_part in the output. The result of 1 indicates the first month of January.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/93fe4584-6d66-4f84-8ed3-9b462a2a31b2)

# 4. Extract to summarize by field

Extracting fields from dates is useful when looking at how data varies by one unit of time across a larger unit of time. For example, how do sales vary by month across years? Using sales from 2010-2016, are sales in January usually higher than those in March?

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/f19f184f-38e7-47a7-b052-16d5ff8aedc6)

# 5. Truncating dates

Instead of extracting single fields, you can also truncate dates and timestamps to a specified level of precision. Remember that dates and timestamps are ordered from left to right, largest units to smallest. You can use the date_trunc function, which is short for date truncate, to specify how much of a timestamp to keep, as you might with a numeric value. Valid field types include all of those we discussed except day of week. Date_trunc replaces fields smaller than, or less significant than, the one specified with zero, or one, as appropriate. Month and day are set to 1, while time fields are set to 0. Here, the year and month remain, and the rest of the fields are set to 0 or 1. The timezone remains unchanged.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/86a4e19a-6c60-4db3-b4f2-a65cca2b9673)

# 6. Truncate to keep larger units

Truncating dates is useful when you want to count, average, or sum data associated with timestamps or dates by larger units of time. For example, starting from individual timestamped sales transactions, what is the monthly trend in sales from June 2017 to January 2019?

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/5e8dd842-8939-4d78-b379-320b62d5de42)

# 7. Time to practice extracting and aggregating dates

Okay, time to start practicing using these new functions.

# Date parts

The date_part() function is useful when you want to aggregate data by a unit of time across multiple larger units of time. For example, aggregating data by month across different years, or aggregating by hour across different days.

Recall that you use date_part() as:

SELECT date_part('field', timestamp);
In this exercise, you'll use date_part() to gain insights about when Evanston 311 requests are submitted and completed.

1. How many requests are created in each of the 12 months during 2016-2017?

```
-- Extract the month from date_created and count requests
SELECT date_part('month', date_created) AS month, 
       COUNT(*)
  FROM evanston311
 -- Limit the date range
 WHERE date_created >= '2016-01-01'
   AND date_created < '2018-01-01'
 -- Group by what to get monthly counts?
 GROUP BY month;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/7030a155-0f27-4572-8248-5855e0bf4735)

2. What is the most common hour of the day for requests to be created?

```
-- Get the hour and count requests
SELECT date_part('hour', date_created) AS hour,
       count(*)
  FROM evanston311
 GROUP BY hour
 -- Order results to select most common
 ORDER BY count DESC
 LIMIT 1;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/f0c787c1-4140-4e94-bd8c-b609174fc8b5)

3. During what hours are requests usually completed? Count requests completed by hour.
Order the results by hour.

```
-- Count requests completed by hour
SELECT date_part('hour', date_completed) AS hour,
       COUNT(*)
  FROM evanston311
 GROUP BY hour
 ORDER BY count DESC;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/39a56305-7c1b-4357-81e1-7af572c03795)

You can also aggregate by day or week of the year, but make sure you read the documentation about how they are computed before using these units.

# Variation by day of week

Does the time required to complete a request vary by the day of the week on which the request was created?

We can get the name of the day of the week by converting a timestamp to character data:

to_char(date_created, 'day') 
But character names for the days of the week sort in alphabetical, not chronological, order. To get the chronological order of days of the week with an integer value for each day, we can use:

EXTRACT(DOW FROM date_created)
DOW stands for "day of week."


Select the name of the day of the week the request was created (date_created) as day.
Select the mean time between the request completion (date_completed) and request creation as duration.
Group by day (the name of the day of the week) and the integer value for the day of the week (use a function).
Order by the integer value of the day of the week using the same function used in GROUP BY.

```
-- Select name of the day of the week the request was created 
SELECT TO_CHAR(date_created, 'Day') AS day, 
       -- Select avg time between request creation and completion
       AVG(date_completed - date_created) AS duration
  FROM evanston311 
 -- Group by the name of the day of the week and 
 -- integer value of day of week the request was created
 GROUP BY day, EXTRACT(DOW FROM date_created)
 -- Order by integer value of the day of the week 
 -- the request was created
 ORDER BY EXTRACT(DOW FROM date_created);
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/40e4ad20-75d5-42ec-a990-a511cbd0d0cb)

Requests created at the beginning of the work week are closed sooner on average than those created at the end of the week or on the weekend.

# Date truncation

Unlike date_part() or EXTRACT(), date_trunc() keeps date/time units larger than the field you specify as part of the date. So instead of just extracting one component of a timestamp, date_trunc() returns the specified unit and all larger ones as well.

Recall the syntax:

date_trunc('field', timestamp)
Using date_trunc(), find the average number of Evanston 311 requests created per day for each month of the data. Ignore days with no requests when taking the average.

Write a subquery to count the number of requests created per day.
Select the month and average count per month from the daily_count subquery.

```
-- Aggregate daily counts by month
SELECT date_trunc('month', day) AS month,
       AVG(count)
  -- Subquery to compute daily counts
  FROM (SELECT date_trunc('day', date_created) AS day,
               COUNT(date_created) AS count
          FROM evanston311
         GROUP BY day) AS daily_count
 GROUP BY month
 ORDER BY month;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/df893407-4fee-4c18-bd0c-ba912941e328)

This query ignores dates with no requests. You'll learn how to account for missing dates in an upcoming exercise.


