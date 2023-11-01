Question 1: Provide a table breaking down the countries where the company operates in multiple cities. Gives a sense of where the company operates to most in. 

SQL Queries:

SELECT country, COUNT(DISTINCT(city))
FROM all_sessions_clean
WHERE city != 'not available in demo dataset' AND city != '(not set)'
GROUP BY country
HAVING count(DISTINCT(city)) > 1
ORDER BY count(DISTINCT(city)) desc

Answer: US with the most at 104. Next highest is India at 17 and Canada with 15




Question 2: What canadian cities does the company work in?

SQL Queries:

SELECT country, city
FROM all_sessions_clean
WHERE country = 'Canada' AND city != 'not available in demo dataset' AND city != '(not set)'
GROUP By country, city 

Answer: 15 different cities as listed in the table. 



Question 3: How many different products are being sold broken down by category in Toronto?

SQL Queries: 
SELECT mainproductcategory_cleaned, count(distinct(productname_cleaned))
FROM all_sessions_clean
WHERE city = 'Toronto'
GROUP BY mainproductcategory_cleaned
ORDER BY count(distinct(productname_cleaned)) DESC

Answer: Apparel has the most types of products for Toronto with 33 unique products



Question 4: WHat is the average Unit price and how does it compare to it's minimum and maximums

SQL Queries:
SELECT AVG(unit_price_cleaned), MIN(unit_price_cleaned), MAX(unit_price_cleaned)
FROM analytics_clean

Answer: Average: 31. Minimum: 0. Maximum: 995. The average is left skewed meaning the majority of the products are will have lower prices. 



Question 5: Do the values calculated match the values from all_sessions table (data quality)? 

SQL Queries:

SELECT AVG(productprice_cleaned), MIN(productprice_cleaned), MAX(productprice_cleaned)
FROM all_sessions_clean


Answer:
Average: 28.8. Min 0. Max 298
This likely means theres either different products in that are not getting sold (i.e since the maximums of the two tables don't match
