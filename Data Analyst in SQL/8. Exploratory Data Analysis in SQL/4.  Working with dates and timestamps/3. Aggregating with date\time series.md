# 1. Aggregating with date/time series

When counting observations by month or day, the result only includes rows for values that appear in your data. How do you find periods of time with no observations?

# 2. Generate series

Recall the generate_series function, which you used with numeric data. The same function can be used with date/time data. generate_series expects timestamps for the from and to arguments. Dates will automatically be cast to a timestamp. The last argument is an interval. For example, here we have an interval of two days. The result is a series of timestamps between the start and end values separated by the interval.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/fdb0560c-27d0-4a90-85d4-ae8fe0fc8012)

# 3. Generate series

Here's an example with an interval of hours. The last value in the series will be less than or equal to the ending timestamp specified. For example, here the series ends at 8pm on January 1st, because the next value in the series would be greater than the 0th hour of January 2nd.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/3c51f8f6-08e1-4ae4-8dae-6448f67f94e3)

4. Generate series from the beginning

To get consistent values, generate series using the beginning of a month or year, not the end. For example, attempting to generate a series for the last day in each month produces unexpected results. When you add one month to January 31st, you get the last day in February, the 28th, because there is no 31st. But then 1 month after February 28th is March 28th, not March 31st.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/80011b21-8ba7-4460-8dd6-6f798619d1ca)

# 5. Generate series from the beginning

To correctly generate a series for the last day of each month, generate a series using the beginning of each month, then subtract 1 day from the result.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/113095f5-ce47-43e1-9437-3a3d79216f20)

# 6. Normal aggregation

Series can also be used to find units of time with no observations. For example, you might want to count sales by the hour of the day they occurred. Here's some sample sales data in its original form. Then with the number of sales counted by hour. Looking at the counts, it's hard to tell at a glance that there were no sales in the 11 o'clock hour.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/9819c967-5a35-4d09-94b8-61c97f34c1e6)

# 7. Aggregation with series

To include hours with no sales, generate a series of hours, and then join this to the original data to introduce rows for the missing hours. First, use a WITH clause to create the series of hours from 9am to 2pm and call this hour_series. Then, join this to the sales data, matching the hour from the series to the sales date truncated to the hour. Count the date column, instead of counting the rows, because we don't want to count null values. Group and order by hours to get the count of sales per hour.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/48ce3d10-b8cf-4370-922e-f3bbe901a1a3)

# 8. Aggregation with series: result

The result now includes all hours between 9am and 2pm, with zeros for hours with no sales. We're less likely to overlook that some hours have no sales.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/76156fbd-2efa-4a3d-94e3-ddc90b8507df)

# 9. Aggregation with bins

If you want to aggregate data by an interval that is not equal to one unit of a date/time field, you can create bins. Recall this strategy from working with numeric data. Let's count sales in 3 hour intervals during the day. First, create two series, one for the lower bound of each bin and one for the upper. The series for the upper bound starts and ends 3 hours after the lower bound. This is the amount of the interval. We alias the result as bins. Then, join bins to the sales data, where the sales date is greater than or equal to the lower bin and less than the upper bin. Then group and order by the bin bounds.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/be324724-13b3-4c29-abdf-ac937c236a36)

# 10. Bin result

The result is the count of sales made during each of the three hour intervals.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/f75c1311-5d78-40f2-90d6-add3a6b855ba)

# 11. Practice generating series!

OK, time to practice using series with date/time data.

# Find missing dates

The generate_series() function can be useful for identifying missing dates.

Recall:

generate_series(from, to, interval)
where from and to are dates or timestamps, and interval can be specified as a string with a number and a unit of time, such as '1 month'.

Are there any days in the Evanston 311 data where no requests were created?

Write a subquery using generate_series() to get all dates between the min() and max() date_created in evanston311.
Write another subquery to select all values of date_created as dates from evanston311.
Both subqueries should produce values of type date (look for the ::).
Select dates (day) from the first subquery that are NOT IN the results of the second subquery. This gives you days that are not in date_created.

```
SELECT day
-- 1) Subquery to generate all dates
-- from min to max date_created
  FROM (SELECT generate_series(MIN(date_created),
                               MAX(date_created),
                               '1 day')::date AS day
          -- What table is date_created in?
          FROM evanston311) AS all_dates
-- 4) Select dates (day from above) that are NOT IN the subquery
 WHERE day NOT IN
       -- 2) Subquery to select all date_created values as dates
       (SELECT date_created::date
          FROM evanston311);
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/c7a5e2f8-a632-4869-85ed-6fb82d12880a)

This approach works for finding missing values of other units of time, such as hours or months, as well.

# Custom aggregation periods

Find the median number of Evanston 311 requests per day in each six month period from 2016-01-01 to 2018-06-30. Build the query following the three steps below.

Recall that to aggregate data by non-standard date/time intervals, such as six months, you can use generate_series() to create bins with lower and upper bounds of time, and then summarize observations that fall in each bin.

Remember: you can access the slides with an example of this type of query using the PDF icon link in the upper right corner of the screen.

1. Use generate_series() to create bins of 6 month intervals. Recall that the upper bin values are exclusive, so the values need to be one day greater than the last day to be included in the bin.

Notice how in the sample code, the first bin value of the upper bound is July 1st, and not June 30th.
Use the same approach when creating the last bin values of the lower and upper bounds (i.e. for 2018).

```
-- Generate 6 month bins covering 2016-01-01 to 2018-06-30

-- Create lower bounds of bins
SELECT generate_series('2016-01-01',  -- First bin lower value
                       '2018-01-01',  -- Last bin lower value
                       '6 months'::interval) AS lower,
-- Create upper bounds of bins
       generate_series('2016-07-01',  -- First bin upper value
                       '2018-07-30',  -- Last bin upper value
                       '6 months'::interval) AS upper;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/9d6233cc-e3c6-4aae-a7ef-e0cf8fea6a8f)

2. Count the number of requests created per day. Remember to not count *, or you will risk counting NULL values.

Include days with no requests by joining evanston311 to a daily series from 2016-01-01 to 2018-06-30.

- Note that because we are not generating bins, you can use June 30th as your series end date.

```
-- Count number of requests made per day
SELECT day, COUNT(date_created) AS count
-- Use a daily series from 2016-01-01 to 2018-06-30 
-- to include days with no requests
  FROM (SELECT generate_series('2016-01-01',  -- series start date
                               '2018-06-30',  -- series end date
                               '1 day'::interval)::date AS day) AS daily_series
       LEFT JOIN evanston311
       -- match day from above (which is a date) to date_created
       ON day = date_created::date
 GROUP BY day;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/ebd48808-94b3-4249-a9d4-50350b8aa209)

3. Assign each daily count to a single 6 month bin by joining bins to daily_counts.
Compute the median value per bin using percentile_disc().

```
-- Bins from Step 1
WITH bins AS (
	 SELECT generate_series('2016-01-01',
                            '2018-01-01',
                            '6 months'::interval) AS lower,
            generate_series('2016-07-01',
                            '2018-07-01',
                            '6 months'::interval) AS upper),
-- Daily counts from Step 2
     daily_counts AS (
     SELECT day, count(date_created) AS count
       FROM (SELECT generate_series('2016-01-01',
                                    '2018-06-30',
                                    '1 day'::interval)::date AS day) AS daily_series
            LEFT JOIN evanston311
            ON day = date_created::date
      GROUP BY day)
-- Select bin bounds 
SELECT lower, 
       upper, 
       -- Compute median of count for each bin
       percentile_disc(0.5) WITHIN GROUP (ORDER BY count) AS median
  -- Join bins and daily_counts
  FROM bins
       LEFT JOIN daily_counts
       -- Where the day is between the bin bounds
       ON day >= lower
          AND day < upper
 -- Group by bin bounds
 GROUP BY lower, upper
 ORDER BY lower;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/0af54bfe-5ffe-4a32-bab3-7a0e8cfe2305)

You might need to create custom bins to correspond to fiscal years, academic years, 2-week periods, or other reporting periods for your organization.

# Monthly average with missing dates

Find the average number of Evanston 311 requests created per day for each month of the data.

This time, do not ignore dates with no requests.

Generate a series of dates from 2016-01-01 to 2018-06-30.
Join the series to a subquery to count the number of requests created per day.
Use date_trunc() to get months from date, which has all dates, NOT day.
Use coalesce() to replace NULL count values with 0. Compute the average of this value.

```
-- generate series with all days from 2016-01-01 to 2018-06-30
WITH all_days AS 
     (SELECT generate_series('2016-01-01',
                             '2018-06-30',
                             '1 day'::interval) AS date),
     -- Subquery to compute daily counts
     daily_count AS 
     (SELECT date_trunc('day', date_created) AS day,
             count(*) AS count
        FROM evanston311
       GROUP BY day)
-- Aggregate daily counts by month using date_trunc
SELECT date_trunc('month', date) AS month,
       -- Use coalesce to replace NULL count values with 0
       avg(coalesce(count, 0)) AS average
  FROM all_days
       LEFT JOIN daily_count
       -- Joining condition
       ON all_days.date = daily_count.day
 GROUP BY month
 ORDER BY month; 
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/76229404-5366-4bc6-a00a-134805f4e8fc)

Because there are few days with no requests, including them doesn't change the averages much. But, including them is always the right way to compute accurate averages!



