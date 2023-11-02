What issues will you address by cleaning the data?
1. Primary Key Idenifcation. This is checked by reviewing duplicates within a table and monitoring whether there are any null entries as these are the two characteristics that establish a primary/composite key. 
2. Incomplete columns. This will be checked by counting the total number of rows in a table and deducting the count of values for a specific (difference results in count of null's). If a column is over 90% NULL is should considered incomplete. 
3. General review tailored to the data characteristic of a column. Any columns remaining after filtering out incomplete will be further reviewed with unique tests such as cross referencing foreign keys, reviewing null values to non-null values, etc. Results included reformatting using case statements, implementing built in functions to add leading 0's, division for price conversion, etc.

4. After cleaning steps are completed, reformatting functions/queries are combined to created a 'cleaned' for a table if necessary.  


Queries:
Below, provide the SQL queries you used to clean your data.

--------------------------------------------------------------------- Duplicates and Primary Key Review ---------------------------------------------------------------------

--Note: total rows were monitored to determine if nulls existed in a key. 

-- All_Sessions: 
-- 1. Total Entries (15134 values) vs entries with duplicates (794 values)
SELECT fullvisitorID
FROM all_sessions;

SELECT fullVisitorId
FROM all_sessions
GROUP BY fullvisitorID
HAVING count(*) > 1;

-- 2. validate if fullvisitorid with productsku has duplicates (potential composite key, 19 duplicates)
SELECT fullVisitorID, productsku
FROM all_sessions
GROUP BY fullvisitorid, productsku
HAVING COUNT(*)>1

-- 3. validate if fullvistorid with productsku, visitid has duplicates (5 duplicates)
SELECT fullVisitorID, productsku, visitid
FROM all_sessions
GROUP BY fullvisitorid, productsku, visitid
HAVING COUNT(*)>1

SELECT * FROM all_sessions
WHERE fullvisitorid = '5355700591805934083' AND productSKU = 'GGOEGCBB074199' AND visitid = 1495005901


-- 4. validate if fullvistorid with productsku, visitid, and time has duplicates (0)
SELECT fullVisitorID, productsku, visitid, time
FROM all_sessions
GROUP BY fullvisitorid, productsku, visitid, time
HAVING COUNT(*)>1

-- RESULT Composite Key for all_sessions consisting of FullVisitorID + ProductSKU + VisitID + Time will be set after Reformatting Stage


-- Analytics: 
-- 1. Total Entries (4301122) vs entries with duplicates (118574 values)
SELECT fullvisitorID
FROM analytics;

SELECT fullVisitorId
FROM analytics
GROUP BY fullvisitorID
HAVING count(*) > 1;

--2. validate if fullvisitorid with visitnumber has duplicates (potential composite key 148214 total duplicates)
SELECT fullvisitorid, visitnumber
FROM analytics
GROUP BY fullvisitorid, visitnumber
HAVING count(*) > 1;
-- Sample
SELECT * 
FROM analytics 
WHERE fullvisitorid = '0000000334692759449' AND visitnumber = 3 
-- RESULT:
-- all columns have the same data except for unit_price which has unique values but also duplicates. 
-- Can't distinguish whether an item is being incorrectly duplicated or if it represents multiple transactions during the same visit for one product being purchased twice

-- Products: 
-- 1. Total Entries (1092) vs entries with duplicates (0 values)
SELECT sku
FROM products;

SELECT sku
FROM products
GROUP BY sku
HAVING count(*) > 1;
-- RESULT : SKU is the primary key


-- Sales_by_sku:
-- 1. Total Entries (462) vs entries with duplicates (0 values)
SELECT productsku
FROM sales_by_sku;

SELECT productsku
FROM sales_by_sku
GROUP BY productsku
HAVING count(*) > 1;
-- RESULT : productsku is the primary key


-- Sales_Report: 
-- 1. Total Entries (454) vs entries with duplicates (0 values)
SELECT productsku
FROM sales_report;

SELECT productsku
FROM sales_report
GROUP BY productsku
HAVING count(*) > 1;

-- RESULT : productsku is the primary key



--------------------------------------------------------------------- Incomplete and General Review ---------------------------------------------------------------------
--------------------------------------------------------------------------- all_sessions table---------------------------------------------------------------------------

-- Incomplete Data Review (Null per column)

SELECT 
	count(*) - count(fullVisitorId) 			as fullVisitorId_nulls,
	count(*) - count(channelGrouping) 			as channelGrouping_nulls,	
	count(*) - count(time) 						as time_nulls,	
	count(*) - count(country) 					as country_nulls,	
	count(*) - count(city) 						as city_nulls,
	count(*) - count(totalTransactionRevenue) 	as totalTransactionRevenue_nulls,	
	count(*) - count(transactions) 				as transactions_nulls,
	count(*) - count(timeOnSite) 				as timeOnSite_nulls,	
	count(*) - count(pageviews) 				as pageviews_nulls,	
	count(*) - count(sessionQualityDim) 		as sessionQualityDim_nulls,	
	count(*) - count(date) 						as date_nulls,
	count(*) - count(visitId) 					as visitId_nulls, 	
	count(*) - count(type) 						as type_nulls,	
	count(*) - count(productRefundAmount) 		as productRefundAmount_nulls, 	
	count(*) - count(productQuantity) 			as productQuantity_nulls,	
	count(*) - count(productPrice) 				as productPrice_nulls, 	
	count(*) - count(productRevenue) 			as productRevenue_nulls, 	
	count(*) - count(productSKU) 				as productSKU_nulls, 	
	count(*) - count(v2ProductName) 			as v2ProductName_nulls,	
	count(*) - count(v2ProductCategory) 		as v2ProductCategory_nulls, 	
	count(*) - count(productVariant) 			as productVariant_nulls_nulls,	
	count(*) - count(currencyCode) 				as currencycode_nulls,	
	count(*) - count(itemQuantity) 				as itemQuantity_nulls,	
	count(*) - count(itemRevenue) 				as itemRevenuecount_nulls,	
	count(*) - count(transactionRevenue) 		as transactionRevenue_nulls,	
	count(*) - count(transactionId) 			as transactionId_nulls,
	count(*) - count(pageTitle) 				as pageTitle_nulls,
	count(*) - count(searchKeyword) 			as searchKeyword_nulls, 	
	count(*) - count(pagePathLevel1) 			as pagePathLevel1_nulls,	
	count(*) - count(eCommerceAction_type) 		as eCommerceAction_type_nulls,
	count(*) - count(eCommerceAction_step) 		as eCommerceAction_step_nulls,	
	count(*) - count(eCommerceAction_option) 	as eCommerceAction_option_nulls
FROM all_sessions

-- RESULT:
-- using 90% of the dataset as a threshold to determine incomplete columns, any columns with more than 13621 nulls will be considered incomplete.
-- totaltransactionrevenue, transactions, sessionqualitydim, productrefundamount, productquantity, productrevenue, itemquantity, itemrevenue, transactionrevenue, transactionid, searchKeyword, eCommerceAction_option will be considered incomplete
-- currencycode and timeonsite should be reviewed further


-- FullVisitorID Review
SELECT DISTINCT(fullVisitorId),
FROM all_sessions
ORDER BY fullVisitorID asc

-- reviewed the number of digits for each id to determine a maximum digit amount
SELECT fullvisitorid, CHAR_LENGTH(fullvisitorid) as digitcount
FROM all_sessions
ORDER BY digitcount ASC

-- RESULT: add leading 0's to ID's using maximum number of 19 digits (to be used when creating view)
SELECT fullVisitorID, LPAD(fullVisitorID, 19, '0') as fullvisitorid_adj
FROM all_sessions



--Channelgrouping Review

SELECT channelgrouping, COUNT(channelgrouping)
FROM all_sessions
GROUP BY channelgrouping
-- No null values. Are unique. 
-- RESULT: Reasonable, no adjustments. 



--Time/timeonesite/date Review

SELECT fullvisitorid, time, timeonsite, date, (time-timeonsite) as time_difference
FROM all_sessions
WHERE timeonsite is Not Null 


SELECT 
	CASE 
		WHEN timeonsite IS null THEN 0
		ELSE timeonsite 
	END AS timeonsite_cleaned
FROM all_sessions

-- RESULT:
-- The time column appears to be an interval. Will assume it is based on a starting point from the date column as there's no clear relationship to the timeonsite. 
-- 3300 nulls in timeonsite, due to lack of context cannot calculate missing information. Use case statement to 
-- No indication of what unit of time measurement the column is based on, will assume seconds. 
-- Result: Enter 0 for timeonsite but will consider time/timeonesite as high risk datapoint if needed for a time related metric 


-- Country/City Review

SELECT country
FROM all_sessions
GROUP BY country;

SELECT city
FROM all_sessions
WHERE country = '(not set)'

SELECT city, count(city)
FROM all_sessions
GROUP BY city
ORDER BY count(city) DESC

-- RESULT:
-- 135 unique options, 1 option for not set(each without a city associated) 
-- no assumptions will be made to replace cities that are not set or available in dataset


-- Pageviews Review

SELECT pageviews, count(pageviews)
FROM all_sessions
GROUP BY pageviews
ORDER BY count(pageviews) DESC

-- RESULT:
-- pageviews has no nulls and counts looks reasonable


-- VISITID REVIEW

WITH digits_CTE AS(
SELECT visitid, CHAR_LENGTH(CAST(visitid as varchar)) as digitcount
FROM all_sessions
ORDER BY digitcount ASC)

SELECT digitcount, count(visitid)
FROM digits_CTE
GROUP BY digitcount

-- RESULT:
-- all visitids have 10 digits and there are no nulls

-- TYPE review:
SELECT DISTINCT(s.type), count(s.type)
FROM all_sessions as s
GROUP BY s.type
-- RESULT:
-- type has no nulls and counts are reasonable

--ProductPrice Review
SELECT ROUND((CAST(productprice as numeric)/1000000), 2) as productprice_cleaned
FROM all_sessions
-- RESULT: 
-- based on additional information provided, unit price was adjusted and will be transferred a view


-- ProductSKU and v2ProductName REVIEW
SELECT DISTINCT(a.productSKU), a.v2ProductName, p.SKU as p_SKU, p.name
FROM all_sessions a
LEFT JOIN products as p
ON a.productSKU = p.SKU
ORDER BY a.v2productname


-- productnames with more than one unique productSKU associated
WITH MultipleProductNames AS(	
	SELECT productSKU, count(DISTINCT v2productname) as num_productnames
	FROM all_sessions
	GROUP BY productSKU
	HAVING count(DISTINCT v2productname) > 1
	ORDER BY productSKU)

SELECT s.productSKU, s.v2productname, p.name
FROM all_sessions as s 
LEFT JOIN products as p
ON s.productSKU = p.SKU
WHERE s.productSKU IN 
			(SELECT productSKU FROM MultipleProductNames)
GROUP BY s.productSKU, s.v2productname, p.name
ORDER BY s.productSKU



-- different names were found between tables for the same product. 
-- for consistency accross tables, get rid of Google, YouTube, Nest®, Waze, Deluge, ATOM, Koozie. Trimmed Column will be used to prepare view.
ALTER TABLE all_sessions
ADD v2productname_trim varchar;

UPDATE all_sessions SET v2productname_trim = v2productname;

UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'Google ', '') 	WHERE v2productname_trim LIKE 'Google %';
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'YouTube ', '')	WHERE v2productname_trim LIKE 'YouTube %';
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'You Tube ', '')	WHERE v2productname_trim LIKE 'You Tube %'; 
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'Nest® ', '')		WHERE v2productname_trim LIKE 'Nest® %'; 	 
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'Waze ', '')		WHERE v2productname_trim LIKE 'Waze %'; 	 
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'Deluge ', '')		WHERE v2productname_trim LIKE 'Deluge %'; 		 
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'ATOM ', '')		WHERE v2productname_trim LIKE 'ATOM %'; 
UPDATE all_sessions SET v2productname_trim = REPLACE(v2productname_trim, 'Koozie ', '')		WHERE v2productname_trim LIKE 'Koozie %'; 



-- RESULT: update productnames to product table's names below


SELECT s.productSKU, 
	CASE 
		WHEN p.name is null THEN s.v2productname_trim -- keep name as is if null 
		WHEN s.productSKU in (SELECT productSKU FROM all_sessions GROUP BY productSKU HAVING count(DISTINCT v2productname) > 1) THEN p.name -- change name to product table name if the SKU has more than 1 associated
		ELSE s.v2productname_trim -- SKU's that don't have more than 1 name will remain the same 
	END AS productname_cleaned
FROM all_sessions as s
LEFT JOIN products as p 
ON s.productSKU = p.SKU



-- Same approach as product names, but for SKU now.  
-- multiple productsku's for productnames (134 out of 471 unique product names)
WITH multipleSKUs AS(
	SELECT v2productname_trim, count(DISTINCT productSKU) as num_productsku
	FROM all_sessions
	GROUP BY v2productname_trim
	HAVING count(DISTINCT productSKU) > 1
	ORDER BY v2productname_trim)

SELECT s.v2productname_trim, s.productSKU as all_sessions_SKU, p.productSKU as products_SKU
FROM all_sessions as s 
LEFT JOIN products as p
ON s.v2productname_trim = p.name
WHERE s.v2productname_trim IN 
			(SELECT v2productname_trim FROM multipleSKUs)
GROUP BY s.v2productname_trim, s.productSKU, r.productSKU, s.productprice
ORDER BY s.v2productname_trim
-- RESULT: 
-- Although there are cases of product names having different SKU codes, it could be reasonable to assume 
-- that the company may use the different codes for the same product name when they have price changes (i.e ipod on sale vs ipod regular)

--ProductCategory Review

SELECT v2ProductCategory, count(v2productcategory)
FROM all_sessions
GROUP BY v2ProductCategory
ORDER By v2ProductCategory
-- "${escCatTitle}" should be treated same as '(not set)'


-- RESULT: reclassify categories

SELECT 
	CASE
		WHEN v2ProductCategory = '${escCatTitle}' OR v2ProductCategory = '(not set)' THEN 'Home_Not_Set'
		WHEN v2ProductCategory LIKE '%Apparel%' THEN 'Apparel' 
		WHEN v2ProductCategory LIKE '%Bags%' THEN 'Bags'
		WHEN v2ProductCategory LIKE '%Bottles%' THEN 'Bottles'
		WHEN v2ProductCategory LIKE '%Drinkware%' THEN 'Drinkware'
		WHEN v2ProductCategory LIKE '%Electronics%' THEN 'Electronics'
		WHEN v2ProductCategory LIKE '%Headgear%' THEN 'Headgear'
		WHEN v2ProductCategory LIKE '%Accessories%' THEN 'Accessories'
		WHEN v2ProductCategory LIKE '%Brands%' THEN 'Brands'
		WHEN v2ProductCategory LIKE '%Clearance%' THEN 'Clearance'
		WHEN v2ProductCategory LIKE '%Fruit Games%' THEN 'Fruit Games'
		WHEN v2ProductCategory LIKE '%Fun%' THEN 'Fun'
		WHEN v2ProductCategory LIKE '%Gift Cards%' THEN 'Gift Cards'
		WHEN v2ProductCategory LIKE '%Kids%' THEN 'Kids'
		WHEN v2ProductCategory LIKE '%Lifestyle%' THEN 'Lifestyle'
		WHEN v2ProductCategory LIKE '%Nest%' THEN 'Nest'
		WHEN v2ProductCategory LIKE '%Office%' THEN 'Office'
		WHEN v2ProductCategory LIKE '%Shop by Brand%' THEN 'Shop by Brand'
		WHEN v2ProductCategory LIKE '%Spring Sale%' THEN 'Spring Sale'
		WHEN v2ProductCategory LIKE '%Housewares%' THEN 'Housewares'
		WHEN v2ProductCategory LIKE '%Waze%' THEN 'Waze'
		WHEN v2ProductCategory LIKE '%Wearables%' THEN 'Wearables'
		WHEN v2ProductCategory LIKE '%YouTube%' THEN 'YouTube'
		ELSE v2ProductCategory
	END AS mainProductCategory_cleaned
FROM all_sessions

-- Product Variant Review
SELECT productvariant, count(productvariant)
FROM all_sessions
GROUP BY productvariant
-- RESULT: More than 90% of the entries are not set. Treat as incomplete and ignore (don't include when creating view)

-- Currency Review
SELECT currencyCode, count(currencyCode)
FROM all_sessions
GROUP BY currencyCode

-- RESULT: Since the only other currency being used in USD and it is more than 90% of the entries, it will be reasonable to assume the null entries can be filled in as USD

SELECT 
	CASE
		WHEN currencyCode IS null THEN 'USD'
		ELSE currencyCode
	END AS currencyCode_clean
FROM all_sessions

-- pagetitle review
SELECT count(pageTitle), pageTitle 
FROM all_sessions
GROUP BY pageTitle

-- RESULT:
-- the data in this column does not provide a clear relationship other values within the same columns (i.e some entries are product names vs some are transaction prompts)

-- PagePathLevel1 Review
SELECT pagePathLevel1, count(pagePathLevel1) 
FROM all_sessions
GROUP BY PagePathLevel1
-- RESULT: data appears to be reasonable

-- ecommerceactiontype review
SELECT ecommerceaction_type, count(ecommerceaction_type) 
FROM all_sessions
GROUP BY ecommerceaction_type
-- RESULT: data appears to be reasonable

-- eCommerceAction_step
SELECT eCommerceAction_step, count(eCommerceAction_step) 
FROM all_sessions
GROUP BY eCommerceAction_step
-- RESULT: data appears to be reasonable in terms of completeness, but likely won't provide much value since more than 90% is 1


--------------------------------------------------------------------------- analytics table---------------------------------------------------------------------------
-- Incomplete Data Review (Null per column)

SELECT 
	count(*) - count(visitnumber)  			as visitnumber_nulls,
	count(*) - count(visitid) 				as visitid_nulls,	
	count(*) - count(visitstarttime) 		as visitstarttime_nulls,	
	count(*) - count(date) 					as date_nulls,	
	count(*) - count(fullvisitorid) 		as fullvisitorid_nulls,
	count(*) - count(userid) 				as userid_nulls,	
	count(*) - count(channelgrouping) 		as channelgrouping_nulls,
	count(*) - count(socialengagementtype) 	as socialengagementtype_nulls,		
	count(*) - count(units_sold) 			as units_sold_nulls,	
	count(*) - count(pageviews) 			as pageviews_nulls,	
	count(*) - count(timeonsite) 			as timeonsite_nulls,
	count(*) - count(bounces) 				as bounces_nulls, 	
	count(*) - count(revenue) 				as revenue_nulls,	
	count(*) - count(unit_price) 			as unit_price_nulls 	
FROM analytics
-- RESULT
-- userid, units_sold, timeonsite, revenue all are greater than 90% null. Treat as incomplete
-- bounces (89%) will also be treated as incomplete
-- review the pageviews nulls further


-- FullVisitorID Review
-- Reformat to have leading 0's as found in all_sessions table

SELECT fullVisitorID, LPAD(fullVisitorID, 19, '0') as fullvisitorid_adj
FROM analytics


-- visitnumber review
SELECT visitnumber, count(visitnumber)
FROM analytics
GROUP BY visitnumber

--RESULT:
-- assuming that visitnumber reflects which instance an ID came to the site, it makes sense that the counts are largest at 1 and decrease as the visit number increases

--VisitID review

SELECT visitid, count(visitID)
FROM analytics
GROUP BY visitid

--RESULT:
-- as per primarykey review, there are multiple duplicates for each column counts look reasonable


--VisitStartTime Review
SELECT visitStartTime, count(visitStartTime)	
FROM analytics
GROUP BY visitStartTime

--RESULT:
-- start times appear to be UNIX time stamps as per https://www.unixtimestamp.com/

SELECT 
	TO_TIMESTAMP(visitStartTime) as visitStartTime_cleaned
FROM analytics

-- compare to dates

SELECT TO_TIMESTAMP(visitStartTime) as starttime, date, TO_TIMESTAMP(visitStartTime)-date as difference	
FROM analytics
WHERE TO_TIMESTAMP(visitStartTime)-date > INTERVAL '24 Hours'	
-- considering there are 45000 entries that have more than a day between starttime to date, date should be treated as a final purchase date. 


--ChannelGrouping Review
SELECT channelGrouping, count(channelGrouping)
FROM analytics
GROUP BY channelGrouping
-- RESULT
-- channelGrouping breakdown looks reasonable


--SocialEngagementType Review
SELECT socialEngagementType, count(socialEngagementType)	
FROM analytics
GROUP BY socialEngagementType
-- RESULT
-- 100% not socially engaged. Not useful for analysis, leave out from view. 

-- pageviews review
SELECT pageviews, count(pageviews)	
FROM analytics
GROUP BY pageviews
--RESULT
--pageviews breakdown looks reasonable


--UnitPrice REVIEW/RESULT 
SELECT ROUND((CAST(unit_price as numeric)/1000000),2) as unit_price_cleaned
FROM analytics


--------------------------------------------------------------------------- products table---------------------------------------------------------------------------
-- Incomplete Data Review (Null per column)

SELECT 
	count(*) - count(sku)  					as sku_nulls,
	count(*) - count(name) 					as name_nulls,	
	count(*) - count(orderedquantity) 		as orderedquantity_nulls,	
	count(*) - count(stocklevel) 			as stocklevel_nulls,	
	count(*) - count(restockingleadtime) 	as restockingleadtime_nulls,
	count(*) - count(sentimentscore) 		as sentimentscore_nulls,	
	count(*) - count(sentimentmagnitude) 	as sentimentmagnitude_nulls 	
FROM products
-- RESULT
-- No columns can be considered incomplete.

-- SKU Review
SELECT SKU, count(name)
FROM products
GROUP BY SKU
HAVING count(name) > 1
-- RESULT:
-- All SKU's have only 1 name associated meaning they are all unique and also not null. Valid Primary Key. 

-- Name Review
SELECT name, count(SKU)
FROM products
GROUP BY name
-- RESULT
-- as assumed in all_sessions review, 1 name can have multiple SKUs. Breakdown is reasonable. 

-- Ordered Quantity, stocklevel, restockingleadtime reveiw
SELECT SKU, SUM(orderedquantity)
FROM products
GROUP BY SKU
-- all SKU's have an orderedquantity 

SELECT SKU, orderedquantity, stocklevel, restockingleadtime
FROM products
WHERE orderedquantity = 0
-- ordered quantity is reasonable as it has values for all SKU's and when it is 0, it has either a stock level or restockleadtime

SELECT SKU, orderedquantity, stocklevel, restockingleadtime
FROM products
WHERE stocklevel = 0
-- stock level is reasonable as it has values all SKU's and when it's 0, it has a restocking leadtime

SELECT SKU, orderedquantity, stocklevel, restockingleadtime
FROM products
WHERE restockingleadtime > 0
-- restocking leadtime has reasonable values for all SKUs and is never 0. 

-- SentimentScore / SentimentMagnitude Review

SELECT MIN(sentimentscore), MAX(sentimentscore), AVG(sentimentscore), MIN(sentimentmagnitude), MAX(sentimentmagnitude), AVG(sentimentmagnitude)
FROM products
-- RESULT:
-- due to lack of context for ratio data, can't verify columns. AVG for both fall reasonable within the range of columns

-- table required no cleaning


--------------------------------------------------------------------------- sales_report table---------------------------------------------------------------------------
-- Incomplete Data Review (Null per column)
SELECT 
	count(*) - count(productSKU)  			as productSKU_nulls,
	count(*) - count(total_ordered) 		as total_ordered_nulls,	
	count(*) - count(name) 					as name_nulls,	
	count(*) - count(stocklevel) 			as stocklevel_nulls,	
	count(*) - count(restockingleadtime) 	as restockingleadtime_nulls,
	count(*) - count(sentimentscore) 		as sentimentscore_nulls,	
	count(*) - count(sentimentmagnitude) 	as sentimentmagnitude_nulls,
	count(*) - count(ratio) 				as ratio_nulls
FROM sales_report

-- ratio has relevant amount of nulls to review


--productSKU and name REVIEW

SELECT productsku FROM( 
	SELECT s.productSKU, s.name as sales_name, p.name as product_name, s.total_ordered as sales_quantity, p.orderedquantity as products_quantity
	FROM sales_report as s
	JOIN products as p
	ON s.productSKU = p.sku)as sku_with_names
WHERE sales_name != product_name 
--RESULT
-- after joining the two tables, confirmed that the names match for each SKU (disregard quantity columns as it's used later)

-- total_ordered review
SELECT s.productSKU, s.total_ordered as sales_quantity, p.orderedquantity as products_quantity
	FROM sales_report as s
	JOIN products as p
	ON s.productSKU = p.sku
-- RESULT
-- since the quantities are not the same, this means that the total ordered from sales indiciate number or orders while products quantity indicates number of units in an order

-- StockLevel/Restocking lead time/sentimentscore/sentimentmagnitude Review
SELECT 
		s.productSKU, 
		(s.stocklevel - p.stocklevel) as stocklevel_difference, 
		(s.restockingleadtime - p.restockingleadtime) as restocking_difference, 
		(s.sentimentscore - p.sentimentscore) as sentimentscore_difference, 
		(s.sentimentmagnitude - p.sentimentmagnitude) as sentimentmagnitude_difference
FROM sales_report as s
JOIN products as p
ON s.productSKU = p.sku
-- RESULT
-- all values are the same when compared to products table.


-- Ratio Review
SELECT ROUND((ratio - ratio_clean),2) as ratio_diff
FROM(
	SELECT ratio, 
		CASE
			WHEN stockLevel = 0 THEN 0
			ELSE CAST(total_ordered as numeric)/CAST(stockLevel as numeric) 
		END AS ratio_clean
	FROM sales_report) as ratio_calc
-- RESULT
-- ratio calculation is consistent, reformat column in view to replace null's with 0 (can't divide by 0)


--------------------------------------------------------------------------- sales_by_sku table---------------------------------------------------------------------------
-- Incomplete Data Review (Null per column)

SELECT 
	count(*) - count(productSKU)  			as productSKU_nulls,
	count(*) - count(total_ordered) 		as total_ordered_nulls
FROM sales_by_sku
-- RESULT
-- No Nulls
--productsku has all unique values as determined during primary key review


--Total_ordered Review
SELECT a.*, b.total_ordered, (a.total_ordered - b.total_ordered) as difference 
FROM sales_by_sku as a
JOIN sales_report as b
ON a.productsku = b.productsku
--RESULT
-- all total_ordered as the same as what's in the sales_report

-- no further action required for sales_by_sku


------------------------------------------------------------------------- Creating 'Cleaned' Views -------------------------------------------------------------------------

CREATE VIEW all_sessions_clean AS(

	SELECT 
		LPAD(s.fullVisitorID, 19, '0') as fullvisitorid_cleaned,							-- leading 0's for fullvisitorid
		s.channelgrouping,
		s.time,
		s.country,
		s.city, 
		CASE 
			WHEN s.timeonsite IS null THEN 0
			ELSE s.timeonsite 									
		END AS timeonsite_cleaned,														-- entering 0's for empty timeonsites 
		s.pageviews,
		s.date,
		s.visitid,
		s.type,
		ROUND((CAST(s.productprice as numeric)/1000000), 2) as productprice_cleaned,		-- ProductPrice divided by 1000000
		s.productSKU,
		CASE 
			WHEN p.name is null THEN s.v2productname_trim -- keep name as is if null 
			WHEN s.productSKU in (SELECT productSKU FROM all_sessions GROUP BY productSKU HAVING count(DISTINCT v2productname) > 1) THEN p.name -- change name to product table name if the SKU has more than 1 associated
			ELSE s.v2productname_trim -- SKU's that don't have more than 1 name will remain the same 
		END AS productname_cleaned,
		CASE
			WHEN s.v2ProductCategory = '${escCatTitle}' OR s.v2ProductCategory = '(not set)' THEN 'Home_Not_Set'
			WHEN s.v2ProductCategory LIKE '%Apparel%' THEN 'Apparel' 
			WHEN s.v2ProductCategory LIKE '%Bags%' THEN 'Bags'
			WHEN s.v2ProductCategory LIKE '%Bottles%' THEN 'Bottles'
			WHEN s.v2ProductCategory LIKE '%Drinkware%' THEN 'Drinkware'
			WHEN s.v2ProductCategory LIKE '%Electronics%' THEN 'Electronics'
			WHEN s.v2ProductCategory LIKE '%Headgear%' THEN 'Headgear'
			WHEN s.v2ProductCategory LIKE '%Accessories%' THEN 'Accessories'
			WHEN s.v2ProductCategory LIKE '%Brands%' THEN 'Brands'
			WHEN s.v2ProductCategory LIKE '%Clearance%' THEN 'Clearance'
			WHEN s.v2ProductCategory LIKE '%Fruit Games%' THEN 'Fruit Games'
			WHEN s.v2ProductCategory LIKE '%Fun%' THEN 'Fun'
			WHEN s.v2ProductCategory LIKE '%Gift Cards%' THEN 'Gift Cards'
			WHEN s.v2ProductCategory LIKE '%Kids%' THEN 'Kids'
			WHEN s.v2ProductCategory LIKE '%Lifestyle%' THEN 'Lifestyle'
			WHEN s.v2ProductCategory LIKE '%Nest%' THEN 'Nest'
			WHEN s.v2ProductCategory LIKE '%Office%' THEN 'Office'
			WHEN s.v2ProductCategory LIKE '%Shop by Brand%' THEN 'Shop by Brand'
			WHEN s.v2ProductCategory LIKE '%Spring Sale%' THEN 'Spring Sale'
			WHEN s.v2ProductCategory LIKE '%Housewares%' THEN 'Housewares'
			WHEN s.v2ProductCategory LIKE '%Waze%' THEN 'Waze'
			WHEN s.v2ProductCategory LIKE '%Wearables%' THEN 'Wearables'
			WHEN s.v2ProductCategory LIKE '%YouTube%' THEN 'YouTube'
			ELSE s.v2ProductCategory
		END AS mainProductCategory_cleaned,													-- combined similar categories
		CASE
			WHEN s.currencyCode IS null THEN 'USD'
			ELSE s.currencyCode
		END AS currencyCode_clean,															-- replaced null's with USD
		s.pagePathLevel1,
		s.eCommerceAction_type,
		s.eCommerceAction_step
	FROM all_sessions as s
	LEFT JOIN products as p 
	ON s.productSKU = p.SKU)


CREATE VIEW analytics_clean AS(
	SELECT
		visitNumber,	
		visitId,	
		TO_TIMESTAMP(visitStartTime) as visitStartTime_clean,							-- convert visitstart time to a readable timestamp
		date,	
		LPAD(fullVisitorID, 19, '0') as fullvisitorid_clean,							-- add leading 0's to full visitor id
		channelGrouping,	
		pageviews,	
		ROUND((CAST(unit_price as numeric)/1000000),2) as unit_price_cleaned 			-- convert unit price to correct value
	FROM analytics)
	

CREATE VIEW sales_report_clean AS(
	SELECT 
		productSKU,	
		total_ordered,	
		name,	
		stockLevel,	
		restockingLeadTime,	
		sentimentScore,	
		sentimentMagnitude,
		CASE
			WHEN stockLevel = 0 THEN 0
			ELSE CAST(total_ordered as numeric)/CAST(stockLevel as numeric) 
		END AS ratio_clean																-- converted null's to 0 by recalculating the ratio
	FROM sales_report)

-- No modifications were required for products/sales_by_SKU
