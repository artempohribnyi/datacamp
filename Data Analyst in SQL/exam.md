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
