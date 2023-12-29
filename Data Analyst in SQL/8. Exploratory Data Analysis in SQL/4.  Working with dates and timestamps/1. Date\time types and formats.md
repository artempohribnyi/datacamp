What time is it? In this chapter, you'll learn how to find out. You'll aggregate date/time data by hour, day, month, or year and practice both constructing time series and finding gaps in them.

# 1. Date/time types and formats

The last type of data we're exploring is date/time data. As the name suggests, date/time refers to columns that store dates and or times.

# 2. Main types

There are two main types: date and timestamp. Dates only include year, month, and day. Timestamps include a date plus a time. Times are specified in terms of hours from 0 to 24, minutes, and seconds. Seconds can be fractional down to microseconds.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/450842f2-1d38-426c-9f99-b37aaa3e5b9a)

# 3. Intervals

There is also a third date/time type you should know: an interval. Intervals represent time durations. For example, 6 days, 1 hour, 48 minutes, and 8 seconds, or 51 minutes and 3 seconds. Columns can be of type interval, but it's more common to encounter intervals as a result of subtracting one date or timestamp from another. Intervals will default to display the number of days, if any, and the time.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/52614770-dd8e-4df8-9a45-a2052b091da1)

# 4. Date/time format examples

Date/time data can be difficult to work with because people record dates in many different formats. Consider some of the different ways people might write 1pm on January 10th, 2018. They might write the date with either the month or day first. They can use two digits for the year or four. They could spell out the month name, abbreviate it, or use numbers. They might specify the time using a 12 hour clock or 24 hour clock.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/176fb374-d9fa-4816-b71b-3ceb812ddb72)

# 5. ISO 8601

To address this ambiguity, Postgres stores date/time information according to something called the ISO 8601 standard. ISO 8601 specifies one way to record date/time information. The units are listed in order from the largest to the smallest - the way we write numbers. Years come first, followed by months and then days. When time information is added, it starts with hours, then minutes, then seconds. Each component has a fixed number of digits, so smaller values must be padded with a leading zero.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/9bb416a1-1031-4497-ba35-2bf46cb69ef8)

# 6. UTC and timezones

Timezones are another way datetime information can get complicated. Postgres stores timestamps according to UTC, or Coordinated Universal Time. Timezones are defined in terms of their offset from UTC. Timestamps in Postgres can include timezone information or not. When timezones are included, they appear at the end with a plus or minus, followed by the number of hours the timezone is offset from UTC. The example timestamp here is 2 hours ahead of UTC.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/12d4e2c6-5d4c-4125-8f5d-8a50339c82a7)

# 7. Date and time comparisons

So how do we work with dates and timestamps? Date/time entries can be compared to each other as numbers can: with greater than, less than, and equals signs. You can get the current timestamp with the now function. This can be useful when comparing values to the current date and time. Note how dates in these examples are specified in ISO 8601 format. They are surrounded by single quotes like character data.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/a0f9bb61-b348-49d6-9473-bd1b161c2c67)

# 8. Date subtraction

In addition to comparing dates, you can also subtract them from each other. The result is of type interval.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/ef5a0e03-abaf-40a5-b00a-9499e4f2f78a)

# 9. Date addition

You can also add time to or subtract time from existing dates. Adding an integer value to a date will add days. Adding an integer to a timestamp, however, will cause an error. Other amounts of time, from years to seconds, can be added with intervals. You specify an interval with a combination of numbers and words inside single quotes, then cast this as an interval. For example, you can add an interval of one year. Or, you can specify the interval in terms of multiple units, such as 1 year, 2 days, and 3 minutes.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/abab0342-cb9a-4dac-a941-6782ca174529)

# 10. Let's practice!

Alright, it's that time again: time to try some exercises with dates.

# ISO 8601
Which date format below conforms to the ISO 8601 standard?

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/a08b19d6-8d8d-428e-83e2-0c8c396b47f4)

The units are ordered from largest to smallest, and single digit values have a leading zero.

# Date comparisons

When working with timestamps, sometimes you want to find all observations on a given day. However, if you specify only a date in a comparison, you may get unexpected results. This query:

SELECT count(*) 
  FROM evanston311
 WHERE date_created = '2018-01-02';
returns 0, even though there were 49 requests on January 2, 2018.

This is because dates are automatically converted to timestamps when compared to a timestamp. The time fields are all set to zero:

SELECT '2018-01-02'::timestamp;
 2018-01-02 00:00:00
When working with both timestamps and dates, you'll need to keep this in mind.

1. Count the number of Evanston 311 requests created on January 31, 2017 by casting date_created to a date.

```
-- Count requests created on January 31, 2017
SELECT count(*) 
  FROM evanston311
 WHERE date_created::date = '2017-01-31';
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/fb7e3fa5-f7f7-428b-9220-f6b3e15d44d9)

2. Count the number of Evanston 311 requests created on February 29, 2016 by using >= and < operators.

```
-- Count requests created on February 29, 2016
SELECT count(*)
  FROM evanston311 
 WHERE date_created >= '2016-02-29' 
   AND date_created < '2016-03-01';
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/954fdbdd-48be-40e2-9436-c473497b3d5f)

3. Count the number of requests created on March 13, 2017.
Specify the upper bound by adding 1 to the lower bound.

```
-- Count requests created on March 13, 2017
SELECT count(*)
  FROM evanston311
 WHERE date_created >= '2017-03-13'
   AND date_created < '2017-03-13'::date + 1;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/72024d57-3084-4627-ae15-0b88f0257864)

The strategy used in the first step is the simpliest and will often work, but the other approaches may be useful in different circumstances.

# Date arithmetic

You can subtract dates or timestamps from each other.

You can add time to dates or timestamps using intervals. An interval is specified with a number of units and the name of a datetime field. For example:

'3 days'::interval
'6 months'::interval
'1 month 2 years'::interval
'1 hour 30 minutes'::interval
Practice date arithmetic with the Evanston 311 data and now().

1. Subtract the minimum date_created from the maximum date_created.

```
-- Subtract the min date_created from the max
SELECT MAX(date_created) - MIN(date_created)
  FROM evanston311;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/5e12113c-ad9a-44a1-b865-6da6d6c1b16f)

2. Using now(), find out how old the most recent evanston311 request was created.

```
-- How old is the most recent request?
SELECT NOW() - MAX(date_created)
  FROM evanston311;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/d95cf65c-4b3d-4e63-a6bb-765ee464293b)

3. Add 100 days to the current timestamp.

```
-- Add 100 days to the current timestamp
SELECT NOW() + '100 days'::interval;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/23fb48ef-1b2c-4014-a2ef-fe3954b8a8d1)

4. Select the current timestamp and the current timestamp plus 5 minutes.

```
-- Select the current timestamp, 
-- and the current timestamp + 5 minutes
SELECT NOW(), NOW() + '5 minutes'::interval;
```
![image](https://github.com/artempohribnyi/datacamp/assets/113499718/25f72b5d-7df2-4515-a7a3-fd137a479c7b)

The 's' at the of 'minutes' is optional. The query will work with or without it.

# Completion time by category

The evanston311 data includes a date_created timestamp from when each request was created and a date_completed timestamp for when it was completed. The difference between these tells us how long a request was open.

Which category of Evanston 311 requests takes the longest to complete?

Compute the average difference between the completion timestamp and the creation timestamp by category.
Order the results with the largest average time to complete the request first.

```
-- Select the category and the average completion time by category
SELECT category, 
       AVG(date_completed - date_created) AS completion_time
  FROM evanston311
 GROUP BY category
-- Order the results
 ORDER BY completion_time DESC;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/d638432a-d35b-4f07-816c-fb9c9249659f)

The results of one query can help you generate further questions to answer: Why do rat issues take so long to resolve? We'll investigate in an upcoming exercise.
