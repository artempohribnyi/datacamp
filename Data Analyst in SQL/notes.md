```
-- Count number of NUULs
CREATE TEMP TABLE null_count AS SELECT
	COUNT(*) - COUNT("Region") AS "Region",
	COUNT(*) - COUNT("Place name") AS "Place name",
	COUNT(*) - COUNT("Place type") AS "Place type",
	COUNT(*) - COUNT("Rating") AS "Rating",
	COUNT(*) - COUNT("Reviews") AS "Reviews",
	COUNT(*) - COUNT("Price") AS "Price",
	COUNT(*) - COUNT("Delivery option") AS "Delivery option",
	COUNT(*) - COUNT("Dine in option") AS "Dine in option",
	COUNT(*) - COUNT("Takeout option") AS "Takeout option"
FROM coffee;

SELECT *
FROM null_count
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/5a492faf-a9b6-418b-a7cb-687e895d5249)

```
-- NULLs count
CREATE TEMP TABLE null_count AS 
SELECT
	COUNT(*) - COUNT("Region") AS "Region",
	COUNT(*) - COUNT("Place name") AS "Place name",
	COUNT(*) - COUNT("Place type") AS "Place type",
	COUNT(*) - COUNT("Rating") AS "Rating",
	COUNT(*) - COUNT("Reviews") AS "Reviews",
	COUNT(*) - COUNT("Price") AS "Price",
	COUNT(*) - COUNT("Delivery option") AS "Delivery option",
	COUNT(*) - COUNT("Dine in option") AS "Dine in option",
	COUNT(*) - COUNT("Takeout option") AS "Takeout option"
FROM coffee;


-- Cleansing
-- "Place name" fix capitalisations issues
-- "Rating" Missing values should be replaced with 0
-- "Reviews" Missing values should be replaced with the overall median number, rounded to the nearest interger value.
-- "Dine in option" alter 1 - True, NULL - False
-- "Takeout option" alter 1 - True, NULL - False



CREATE TEMP TABLE clean AS
SELECT
	COALESCE("Region", 'Unknown') AS "Region",
	COALESCE("Place name", 'Unknown') AS "Place name",
	COALESCE("Place type", 'Unknown') AS "Place type",
	COALESCE("Rating", 0) AS "Rating",
	COALESCE("Reviews", (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews") FROM coffee)) AS "Reviews",
	COALESCE("Price", 'Unknown') AS "Price",
	COALESCE("Delivery option", 'False') AS "Delivery option",
	CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE 'True' END AS "Dine in option",
	CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE 'True' END AS "Takeout option"
FROM coffee;

SELECT
	*
FROM clean;
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/25e1bbe3-50c4-422d-8a64-0c13dbe4f4cd)

```
CREATE TEMP TABLE clean AS
SELECT
	COALESCE("Region", 'Unknown') AS "Region",
	COALESCE("Place name", 'Unknown') AS "Place name",
	COALESCE("Place type", 'Unknown') AS "Place type",
	COALESCE("Rating", 0) AS "Rating",
	COALESCE("Reviews", (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")) FROM coffee)) AS "Reviews",
	COALESCE("Price", 'Unknown') AS "Price",
	COALESCE("Delivery option", 'False') AS "Delivery option",
	CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE 'True' END AS "Dine in option",
	CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE 'True' END AS "Takeout option"
FROM coffee;

SELECT
	"Place type", 
	MIN("Reviews") AS "min_review", 
	MAX("Reviews") AS "max_review"
FROM clean
GROUP BY "Place type";
```

![image](https://github.com/artempohribnyi/datacamp/assets/113499718/3dc6f520-7bd0-4254-b474-631caa8e9b5d)

```
-- NULLs count
CREATE TEMP TABLE null_count AS 
SELECT
	COUNT(*) - COUNT("Region") AS "Region",
	COUNT(*) - COUNT("Place name") AS "Place name",
	COUNT(*) - COUNT("Place type") AS "Place type",
	COUNT(*) - COUNT("Rating") AS "Rating",
	COUNT(*) - COUNT("Reviews") AS "Reviews",
	COUNT(*) - COUNT("Price") AS "Price",
	COUNT(*) - COUNT("Delivery option") AS "Delivery option",
	COUNT(*) - COUNT("Dine in option") AS "Dine in option",
	COUNT(*) - COUNT("Takeout option") AS "Takeout option"
FROM coffee;


-- Cleansing
-- "Rating" Missing values should be replaced with 0
-- "Reviews" Missing values should be replaced with the overall median number, rounded to the nearest interger value.
-- "Dine in option" alter 1 - True, NULL - False
-- "Takeout option" alter 1 - True, NULL - False



CREATE TEMP TABLE clean AS
SELECT
	COALESCE("Region", 'Unknown') AS "Region",
	COALESCE("Place name", 'Unknown') AS "Place name",
	COALESCE("Place type", 'Unknown') AS "Place type",
	COALESCE("Rating", 0) AS "Rating",
	COALESCE("Reviews", (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")) FROM coffee)) AS "Reviews",
	COALESCE("Price", 'Unknown') AS "Price",
	COALESCE("Delivery option", 'False') AS "Delivery option",
	CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE 'True' END AS "Dine in option",
	CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE 'True' END AS "Takeout option"
FROM coffee;

SELECT
	"Region"::VARCHAR AS "Region",
	"Place name"::VARCHAR AS "Place name",
	"Place type"::VARCHAR AS "Place type",
	"Rating"::NUMERIC AS "Rating",
	"Reviews"::VARCHAR AS "Reviews",
	"Price"::VARCHAR AS "Price",
	"Delivery option"::VARCHAR AS "Delivery option",
	"Dine in option"::VARCHAR AS "Dine in option",
	"Takeout option"::VARCHAR AS "Takeout option"
FROM clean;
```
```
WITH data_clean AS
(SELECT
	"Region"::VARCHAR,
	"Place name"::VARCHAR,
	"Place type"::VARCHAR,
	COALESCE("Rating", 0)::NUMERIC AS "Rating",
	COALESCE("Reviews", (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")::NUMERIC) FROM coffee)) AS "Reviews",
	"Price"::VARCHAR,
	COALESCE("Delivery option", 'False') AS "Delivery option",
	(CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE "Dine in option" END) AS "Dine in option",
	(CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE "Takeout option" END) AS "Takeout option"
FROM coffee)

SELECT
	*
FROM data_clean
```

```
CREATE TEMP TABLE clean AS
SELECT
	COALESCE("Region", 'Unknown') AS "Region",
	COALESCE("Place name", 'Unknown') AS "Place name",
	COALESCE("Place type", 'Unknown') AS "Place type",
	COALESCE("Rating", 0) AS "Rating",
	COALESCE("Reviews", (SELECT ROUND(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY "Reviews")) FROM coffee)) AS "Reviews",
	COALESCE("Price", 'Unknown') AS "Price",
	COALESCE("Delivery option", 'False') AS "Delivery option",
	CASE WHEN "Dine in option" IS NULL THEN 'False' ELSE 'True' END AS "Dine in option",
	CASE WHEN "Takeout option" IS NULL THEN 'False' ELSE 'True' END AS "Takeout option"
FROM coffee;

SELECT
	"Place type", 
	MIN("Reviews") AS "min_review", 
	MAX("Reviews") AS "max_review"
FROM clean
GROUP BY "Place type";
```
# Last
```
-- Write your query for task 2 in this cell

CREATE TEMP TABLE clean_products AS 
SELECT
	product_id,
	product_type,
	CASE WHEN brand = '-' THEN 'Unknown' ELSE brand END AS brand,
	ROUND(LEFT(weight, POSITION(' ' IN weight)-1)::NUMERIC, 2) AS weight,
	ROUND(price::NUMERIC, 2)::money AS price,
	average_units_sold::INTEGER,
	COALESCE(year_added, '2022') AS year_added,
	UPPER(COALESCE(stock_location, 'Unknown')) AS stock_location
FROM
	products;
	

SELECT 
	*
FROM clean_products

```

```
-- Write your query for task 2 in this cell

SELECT
	product_id,
	product_type, -- No missing values and all are matched the criteria
	REPLACE(brand, '-', 'Unknown') AS brand, -- There is a missing value '-'
	REPLACE(weight, 'grams', '')::NUMERIC AS weight, -- There is a redandency in data 'grams'
	price, -- Matched the criteria 
	average_units_sold, -- Matched the criteria
	COALESCE(year_added, 2022) AS year_added, -- The missing values replaced with 2022
	UPPER(stock_location) AS stock_location -- Had some lowercase values
FROM
	products;
```

```
/*

A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to  decimal places.
Enter your query here.
*/

SET @rowindex := -1;
 
SELECT ROUND(AVG(d.lat_n), 4) as median 
  FROM (SELECT @rowindex:=@rowindex + 1 AS rowindex,
               s.lat_n AS lat_n
          FROM station AS s
      ORDER BY s.lat_n) AS d
 WHERE d.rowindex IN (FLOOR(@rowindex / 2), CEIL(@rowindex / 2));
```

```
SELECT hacker_id,
       name
  FROM (SELECT CASE WHEN c.difficulty_level = 1 AND s.score >= 20 THEN h.hacker_id
                    WHEN c.difficulty_level = 2 AND s.score >= 30 THEN h.hacker_id
                    WHEN c.difficulty_level = 3 AND s.score >= 40 THEN h.hacker_id
                    WHEN c.difficulty_level = 4 AND s.score >= 60 THEN h.hacker_id
                    WHEN c.difficulty_level = 5 AND s.score >= 80 THEN h.hacker_id
                    WHEN c.difficulty_level = 6 AND s.score >= 100 THEN h.hacker_id
                    WHEN c.difficulty_level = 7 AND s.score >= 120 THEN h.hacker_id END AS hacker_id,
                    h.name,
                    COUNT(c.challenge_id) AS num_of_challenges              
               FROM hackers AS h
         INNER JOIN challenges AS c ON h.hacker_id = c.hacker_id
         INNER JOIN submissions AS s ON c.challenge_id = s.challenge_id
           GROUP BY 1,2) AS count
   WHERE num_of_challenges > 1
ORDER BY num_of_challenges DESC, hacker_id;
```

```
/*
Enter your query here.
*/

    SELECT contest_id, 
           hacker_id, 
           name, 
           SUM(total_submissions), 
           SUM(total_accepted_submissions), 
           SUM(total_views), 
           SUM(total_unique_views)
      FROM contests AS c
INNER JOIN colleges AS co USING(contest_id)
INNER JOIN challenges AS ca USING(college_id)
INNER JOIN view_stats AS v USING(challenge_id)
INNER JOIN submission_stats AS s USING(challenge_id)
  GROUP BY 1, 2, 3
    HAVING SUM(total_submissions) > 0 AND 
           SUM(total_accepted_submissions) > 0 AND 
           SUM(total_views) > 0 AND
           SUM(total_unique_views) > 0     
  ORDER BY contest_id;
```
