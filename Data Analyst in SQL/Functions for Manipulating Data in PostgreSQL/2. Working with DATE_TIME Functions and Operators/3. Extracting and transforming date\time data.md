# 1. Extracting and transforming date / time data

In a previous video, you learned about the AGE function and how to use it to calculate the difference between two timestamps. Next we'll look at some additional built-in functions that will help us transform timestamp and interval data types and create new fields that will help us prepare data for analysis.

# 2. Extracting and transforming date and time data

Let's start by taking a look at how we can use the EXTRACT, DATE_PART and DATE_TRUNC functions to manipulate timestamp data and create new columns by extracting sub-fields from existing date and time values. This type of data manipulation is useful when the precision of a timestamp is not useful for analysis and you want to use date parts like year or month in your queries but the underlying data only contains a standard timestamp value. You may also not care about certain precision like time of day in some analyses and truncating timestamps may be necessary.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/57f40b69-1e42-4bea-ab0c-78f1205e0c81)


# 3. Extracting and transforming date / time data

This is where the EXTRACT and DATE_PART functions come in very handy. To use these functions in your queries you will need to pass two parameters. The field identifier and the source. The field parameter is an identifier (or string if you are using DATE_PART) that indicates what sub-field that you want to extract from the source. The various field identifiers include year, month, quarter, day of week, etc. The source parameter needs to be a valid timestamp, time, or interval data type. Both EXTRACT and DATE_PART will produce identical results and can be used interchangeably with only slight variations in how you pass in the field and source parameters. Now, let's get into some examples and see this in action.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/570b0c11-4ea3-43c7-9364-c0da68ad1406)


# 4. Extracting sub-fields from timestamp data

In our DVD Rentals database, every customer rental has a corresponding record in the payment table and each transaction is recorded with a timestamp in the payment_date column as the snippet below highlights. This level of detail is certainly necessary for an e-commerce application, but there will no doubt be times when you will want to be able to aggregate this data to use for training a model, reporting and/or trend analysis.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/1aa1224e-a3c9-4b60-a456-42644cdd4b63)


# 5. Extracting sub-fields from timestamp data

For example, you may want to identify the highest revenue by quarter. To do this we'll want to aggregate the amount column from the payment table and use the EXTRACT function to extract the quarter and year sub-fields from the payment_date column. Here you'll see that we also introduce a technique with the GROUP BY clause that allows us to specify the fields in the SELECT clause using a numeric reference which comes in handy when using functions to derive new columns. And you see the results of the query here which aggregates the amount column grouped by quarter and year.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/1bd19b59-6442-4700-b0f9-ec4c34f8f34b)


# 6. Truncating timestamps using DATE_TRUNC()

The DATE_TRUNC() function will truncate timestamp or interval data types to return a timestamp or interval at a specified precision. The precision values are a subset of the field identifiers that can be used with the EXTRACT() and DATE_PART() functions. For example, to truncate a date by year we pass the year identifier as the first parameter of the DATE_TRUNC function, as you see here and get the following result. Or we can truncate the same timestamp using month as the parameter and get this result. Unlike these functions, DATE_TRUNC() will return an interval or timestamp rather than a numeric value.

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/d0c7acb2-b34a-475f-ae08-651a0e34e8ce)

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

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/0537e811-5dcd-4f1c-9fa3-61b365fcae9a)
