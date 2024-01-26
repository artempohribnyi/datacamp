# 1. Extracting and transforming date / time data

In a previous video, you learned about the AGE function and how to use it to calculate the difference between two timestamps. Next we'll look at some additional built-in functions that will help us transform timestamp and interval data types and create new fields that will help us prepare data for analysis.

# 2. Extracting and transforming date and time data

Let's start by taking a look at how we can use the EXTRACT, DATE_PART and DATE_TRUNC functions to manipulate timestamp data and create new columns by extracting sub-fields from existing date and time values. This type of data manipulation is useful when the precision of a timestamp is not useful for analysis and you want to use date parts like year or month in your queries but the underlying data only contains a standard timestamp value. You may also not care about certain precision like time of day in some analyses and truncating timestamps may be necessary.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/d8508b72-d0e1-4154-8ba2-763896739adc)

# 3. Extracting and transforming date / time data

This is where the EXTRACT and DATE_PART functions come in very handy. To use these functions in your queries you will need to pass two parameters. The field identifier and the source. The field parameter is an identifier (or string if you are using DATE_PART) that indicates what sub-field that you want to extract from the source. The various field identifiers include year, month, quarter, day of week, etc. The source parameter needs to be a valid timestamp, time, or interval data type. Both EXTRACT and DATE_PART will produce identical results and can be used interchangeably with only slight variations in how you pass in the field and source parameters. Now, let's get into some examples and see this in action.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/0ed10313-3532-4003-850b-2bdedb73e35e)

# 4. Extracting sub-fields from timestamp data

In our DVD Rentals database, every customer rental has a corresponding record in the payment table and each transaction is recorded with a timestamp in the payment_date column as the snippet below highlights. This level of detail is certainly necessary for an e-commerce application, but there will no doubt be times when you will want to be able to aggregate this data to use for training a model, reporting and/or trend analysis.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/153425e7-4b52-4e7f-8de2-a32bbf5687dc)

# 5. Extracting sub-fields from timestamp data

For example, you may want to identify the highest revenue by quarter. To do this we'll want to aggregate the amount column from the payment table and use the EXTRACT function to extract the quarter and year sub-fields from the payment_date column. Here you'll see that we also introduce a technique with the GROUP BY clause that allows us to specify the fields in the SELECT clause using a numeric reference which comes in handy when using functions to derive new columns. And you see the results of the query here which aggregates the amount column grouped by quarter and year.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/b8e2ca79-7060-4562-96ef-1c7271f59718)

# 6. Truncating timestamps using DATE_TRUNC()

The DATE_TRUNC() function will truncate timestamp or interval data types to return a timestamp or interval at a specified precision. The precision values are a subset of the field identifiers that can be used with the EXTRACT() and DATE_PART() functions. For example, to truncate a date by year we pass the year identifier as the first parameter of the DATE_TRUNC function, as you see here and get the following result. Or we can truncate the same timestamp using month as the parameter and get this result. Unlike these functions, DATE_TRUNC() will return an interval or timestamp rather than a numeric value.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/a2023fd3-b10d-48eb-8cd9-2452bf103dfc)

# 7. Let's practice!

Let's put these new skills to use in the exercises!

# Using EXTRACT

You can use EXTRACT() and DATE_PART() to easily create new fields in your queries by extracting sub-fields from a source timestamp field.

Now suppose you want to produce a predictive model that will help forecast DVD rental activity by day of the week. You could use the EXTRACT() function with the dow field identifier in our query to create a new field called dayofweek as a sub-field of the rental_date column from the rental table.

You can COUNT() the number of records in the rental table for a given date range and aggregate by the newly created dayofweek column.

1. Get the day of the week from the rental_date column.

```
SELECT 
  -- Extract day of week from rental_date
  EXTRACT(dow FROM rental_date) AS dayofweek 
FROM rental 
LIMIT 100;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/7d51c806-b95e-41c2-b231-5a2f93ca7ae3)

2. Count the total number of rentals by day of the week.

```
-- Extract day of week from rental_date
SELECT 
  EXTRACT(dow FROM rental_date) AS dayofweek, 
  -- Count the number of rentals
  COUNT(*) as rentals 
FROM rental 
GROUP BY 1;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/6f15adec-2a86-4c72-9199-b0b95d975f3f)

Using the EXTRACT() function can help determine hidden insights in your data like what days of the week are the busiest for DVD rentals.

# Using DATE_TRUNC

The DATE_TRUNC() function will truncate timestamp or interval data types to return a timestamp or interval at a specified precision. The precision values are a subset of the field identifiers that can be used with the EXTRACT() and DATE_PART() functions. DATE_TRUNC() will return an interval or timestamp rather than a number. For example
```
SELECT DATE_TRUNC('month', TIMESTAMP '2005-05-21 15:30:30');
```
Result: 2005-05-01 00;00:00

Now, let's experiment with different precisions and ultimately modify the queries from the previous exercises to aggregate rental activity.

1. Truncate the rental_date field by year.

```
-- Truncate rental_date by year
SELECT DATE_TRUNC('YEAR', rental_date) AS rental_year
FROM rental;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/684f6420-88e9-48d9-9865-0b7c95ff1ee4)

2. Now modify the previous query to truncate the rental_date by month.

```
-- Truncate rental_date by month
SELECT DATE_TRUNC('MONTH', rental_date) AS rental_month
FROM rental;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/7ec4f0cb-8b5a-4682-af61-a936351e3ec5)

3. Let's see what happens when we truncate by day of the month.

```
-- Truncate rental_date by day of the month 
SELECT DATE_TRUNC('DAY', rental_date) AS rental_day 
FROM rental;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/af28dfd6-104e-424c-8633-d93677d12b34)

4. Finally, count the total number of rentals by rental_day and alias it as rentals.

```
SELECT 
  DATE_TRUNC('day', rental_date) AS rental_day,
  -- Count total number of rentals 
  COUNT(*) AS rentals 
FROM rental
GROUP BY 1;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/904c65a5-9f79-4af9-8b74-1857f06ca9e7)

You can now use DATE_TRUNC() to manipulate timestamp data types and create new fields with different levels of precision.

# Putting it all together

Many of the techniques you've learned in this course will be useful when building queries to extract data for model training. Now let's use some date/time functions to extract and manipulate some DVD rentals data from our fictional DVD rental store.

In this exercise, you are going to extract a list of customers and their rental history over 90 days. You will be using the EXTRACT(), DATE_TRUNC(), and AGE() functions that you learned about during this chapter along with some general SQL skills from the prerequisites to extract a data set that could be used to determine what day of the week customers are most likely to rent a DVD and the likelihood that they will return the DVD late.

1. Extract the day of the week from the rental_date column using the alias dayofweek.
Use an INTERVAL in the WHERE clause to select records for the 90 day period starting on 5/1/2005.

```
SELECT 
  -- Extract the day of week date part from the rental_date
  EXTRACT(DOW FROM rental_date) AS dayofweek,
  AGE(return_date, rental_date) AS rental_days
FROM rental AS r 
WHERE 
  -- Use an INTERVAL for the upper bound of the rental_date 
  rental_date BETWEEN CAST('2005-05-01' AS TIMESTAMP)
   AND CAST('2005-05-01' AS TIMESTAMP) + INTERVAL '90 day';
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/13c871ba-be06-4b11-901f-6da82759c60d)

2. Finally, use a CASE statement and DATE_TRUNC() to create a new column called past_due which will be TRUE if the rental_days is greater than the rental_duration otherwise, it will be FALSE.

```
SELECT 
  c.first_name || ' ' || c.last_name AS customer_name,
  f.title,
  r.rental_date,
  -- Extract the day of week date part from the rental_date
  EXTRACT(dow FROM r.rental_date) AS dayofweek,
  AGE(r.return_date, r.rental_date) AS rental_days,
  -- Use DATE_TRUNC to get days from the AGE function
  CASE WHEN DATE_TRUNC('day', AGE(r.return_date, r.rental_date)) > 
  -- Calculate number of d
    f.rental_duration * INTERVAL '1' day 
  THEN TRUE 
  ELSE FALSE END AS past_due 
FROM 
  film AS f 
  INNER JOIN inventory AS i 
  	ON f.film_id = i.film_id 
  INNER JOIN rental AS r 
  	ON i.inventory_id = r.inventory_id 
  INNER JOIN customer AS c 
  	ON c.customer_id = r.customer_id 
WHERE 
  -- Use an INTERVAL for the upper bound of the rental_date 
  r.rental_date BETWEEN CAST('2005-05-01' AS DATE) 
  AND CAST('2005-05-01' AS DATE) + INTERVAL '90 day';
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/ad13f0a6-93bc-401f-8acb-debde049afab)

PostgreSQL date/time functions provide powerful tools for manipulating, cleaning and transforming transactional data.

