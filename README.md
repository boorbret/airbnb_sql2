#AirBnB SQL Challenge

1. What's the most expensive listing? What else can you tell me about the listing?
```
SELECT
	*
FROM
	listings
ORDER BY 
	price
DESC
LIMIT 1
```
This listing is $10,000 per night! It has zero reviews and is available very often at 270 nights per year.
The name refers to it as a MEGA LISTING for groups of 50-100 so it must be extremely large. 


2. What neighborhoods seem to be the most popular?

For this question, I'm assuming the field "Availability 365" is lower for more popular listings. This is because AirBnB will often urge you to book based on how rarely a given listing is available. I have aggregated this data by neighborhood. I will assume that the lower availability the more popular that neighborhood is.
```
SELECT
	neighbourhood,
	SUM(availability_365) days_per_hood
FROM
	listings
GROUP BY 1
ORDER BY
	days_per_hood
ASC
```
3. What time of year is the cheapest time to go to your city? What about the busiest?

For the busiest time of the year I extracted the month from the calendar table and grouped the months by total availability. 
```
SELECT
	EXTRACT(MONTH FROM date) AS month,
	COUNT(*),
	available
FROM
	calendar
WHERE
	available IS TRUE
GROUP BY month, available
ORDER BY
	count
ASC
```
The lowest availability is in September and October, which leads me to believe that the fall is the most popular time in Chicago.

To find the cheapest time, I kept my month extractor in place and aggregated by average price.
```
SELECT
	EXTRACT(MONTH FROM date) AS month,
	AVG(price) AS avg_price
FROM
	calendar
GROUP BY month
ORDER BY 
	avg_price
DESC
```
The cheapest times were January and February, which is not surprising given the weather in Chicago. Something for further investigation in the future: although they had the lowest availability, September and October were in the middle of the pack for price. This could mean there is a different reason for their low availability implying that the most popular is better proxied by some other variable (like price).
