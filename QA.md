What are your risk areas? Identify and describe them.

- Overall data quality has a high-risk factor considering how much inconsistency there is amongst column values. This includes examples when cross referencing tables and having extra brand names for a product name (i.e 'google' or 'YouTube').  
- Throughout the data cleaning process, a signicant amount of null columns and values were found. This leads to question why why are there so many Null columns or was there data lost?
- When comparing prices from the analytics table, the unit prices vary compared to all_sessions tables. If given more time, investigate the variance further and determine if the prices represent different values (i.e discounted price)
- A unique identifier should be included for the Analytics table as there are approximately 20-30 duplicates per ID and the only unique identification is the combination of all the columns together. 



QA Process:
Describe your QA process and include the SQL queries used to execute it.
QA was done simultaneously throughout the project. 
1. Data Cleaning to establish which columns have high risk data (inconsistency, incomplete, duplication)
  Incomplete (Null Check Example reviewing for than a 90% Null column)
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

2. Analytics Questions:
   Comparing Max for two tables that have prices. If the max values for a table provides diferent results, that's an indicator that there can be prices listed for the same products are not being listed at the same amount which can negatively impact results if the two table are ever joined.
   
SELECT AVG(a.unit_price_cleaned), MIN(a.unit_price_cleaned), MAX(a.unit_price_cleaned)
FROM analytics_clean as a

SELECT AVG(productprice_cleaned), MIN(productprice_cleaned), MAX(productprice_cleaned)
FROM all_sessions_clean

3. Manual checks throughout the project such as monitoring the total rows when joining tables to ensuring duplication or other errors don't occur. 
    
