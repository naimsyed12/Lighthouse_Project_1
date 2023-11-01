# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
Build a database to manage an ecommerce company's data. Utilize SQL techniques to clean data and analyze trends across sales, product, demographic, and other data. 

## Process
### Extract
- Source files from drive in CSV format.
- Review CSV files to identify potential primary keys, appropriate data types, and data cleaning steps. 
### Load 
- Create database
- Create tables with datatypes
- Import CSV
### Transform/Cleaning Data
1. Review for duplicates/nulls to validate a primary key
2. Review Nulls (Incomplete Columns) in a table based on counts for each column. Using 90% Null as a threshold to determine if a column is incomplete. 
3. General review on remaining columns
   - Is the value a foreign key? Is so, cross reference it's corresponding table to confirm it's values match (e.g. product SKU)
   - Can missing values be filled in? E.g. If there's only one currency code, remaining transactions with Null currency codes can be assumed to be in USD.
   - Is there unnecessary text or missing in the formatting. Test by cross referencing other tables where the same text is used (e.g. productname).  
4. Transfer all modification functions/queries into a "cleaned" table version using Views to be used during analysis and avoid losing original data 
### Analysis 
Using the "Cleaned" views, starting with questions can be addressed along with personally tailored questions. 

## Results
Based on analysis questions, it can be determined that the companies primary market is the US and they succeed in selling apparel in both the US and other global locations like Canada. It appears there are different types of products that are not getting sold across tables based on the difference in analyzing price ranges. 
The overall quality of data is high risk as there is clear inconsistency within columns and between tables, along with a large amount of empty data. The sourcing processes should be looked into further to understand why some columns are incomplete/inconsistent/duplicated. 


## Challenges 
1. Setting up appropriate data type was a challenge as one value not fitting the datatype constraints would be enough to prevent a whole CSV from being imported. Had to look further into what limits are in place on different types of datatypes (i.e integers).
2. Not being provided business context made it challenging to make conclusions regarding how data was was retrieved and the overall quality of the data. For example, when one table has different product prices than another table that also references product prices. Is one of the table providing a discounted price which can't be confirmed without business context. 

## Future Goals
If given more time, I would make an attempt to normalize the analytics table as all columns combined are what makes a unique entry which is not ideal. Using the normalized analytics tables, I would also attempt cross reference more of the shared column values across the other tables within the database to verify any potential variances/further cleaning required. 
