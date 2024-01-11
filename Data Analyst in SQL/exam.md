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
	"Price",
	"Delivery option",
	"Dine in option",
	"Takeout option"
FROM coffee;

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
FROM clean;
```
