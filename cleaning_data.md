What issues will you address by cleaning the data?
Aside dividing the unit cost(productprice) in the dataset by 1,000,000.
I also wrote some queries and subqueries to compare and extract similar productsku between table i.e. products, sales_by_sku and the sales_report tables. I was able to extract similar productsku, so has to create a relationship between the tables.
I also applied the same method for filtering using the WHERE function and subqueries to compare and extract similar variables such as visitid and fullvisitorid in order to establish a relationship between the all_session and the analytics tables. I then followed up by removing duplicate by using the subqueries, WHERE function and the Window funtion(ROW_NUMBER) to remove duplicates, which enabled me to create primary keys and foreign key constraints on the tables.
Other cleanup done was to remove columns that are all NULL and updated the time variable. Although, since I am unsure what units or time was used, I found it challenging to convert to time data type.
I also trimmed the leading spaces before the text under the productname column of the product table.
Updated and altered the columns from varchar to integer
Queries:
Below, provide the SQL queries you used to clean your data.
Products
Trimmed productname column and updated the products table to avoid query error
UPDATE products_new
SET productname = TRIM (productname)

sales_by_sku_new
SELECT *
FROM sales_by_sku_new
WHERE EXISTS
(SELECT productsku FROM products_new WHERE sales_by_sku_new.productsku = products_new.productsku);

products_new
SELECT *
FROM products_new
WHERE EXISTS
(SELECT productsku FROM sales_by_sku_new WHERE sales_by_sku_new.productsku = products_new.productsku);

sales report_new
SELECT *
FROM sales_report_new
WHERE EXISTS
(SELECT productsku FROM products_new WHERE sales_report_new.productsku = products_new.productsku);

analytics_new
SELECT *
FROM analytics_new
WHERE EXISTS
(SELECT fullvisitorid FROM all_sessions_new WHERE all_sessions_new.fullvisitorsid = analytics_new.fullvisitorid);

Deleting duplicate visitid
DELETE FROM all_sessions_new
WHERE visitid IN (SELECT visitid FROM (SELECT visitid, ROW_NUMBER() OVER(PARTITION BY visitid ORDER BY
visitid) AS row_num FROM all_sessions_new) sessions
WHERE sessions.row_num > 1)

all session_new
SELECT *
FROM all_sessions_new
WHERE EXISTS
(SELECT fullvisitorid FROM analytics_new WHERE analytics_new.fullvisitorid = all_sessions_new.fullvisitorid);

UPDATE all_sessions_new
SET productprice = ROUND(CAST(productprice AS DECIMAL)/1000000,2)

-- verify the current range of lenght of time values				  
SELECT MAX(LENGTH(time)), MIN(LENGTH(time)) FROM all_sessions_new
									 
--I will like to assume the time is in Hours, Minutes and Seconds for the time and timeonsite
--variable since it is logical to spend hours on the sites rather than days.

ALTER TABLE all_sessions_new
ALTER COLUMN time
TYPE INTEGER
USING time::INTEGER;				  

SELECT MAX(LENGTH(timeonsite)), MIN(LENGTH(timeonsite)) FROM all_sessions_new									 

ALTER TABLE all_sessions_new
ALTER COLUMN timeonsite
TYPE VARCHAR
USING time::VARCHAR

ALTER TABLE all_sessions_new
ALTER COLUMN time
TYPE INTEGER
USING time::INTEGER;
										   
-- check for mismatched country and city and update	
SELECT DISTINCT(city),country FROM all_sessions_new
WHERE country = 'United States'

UPDATE all_sessions_new
SET city = 'not available in demo dataset'
WHERE city = '(not set)'
										   
SELECT DISTINCT(country) FROM all_sessions_new	

SELECT * FROM all_sessions_new
WHERE totaltransactionrevenue != 'NULL'	
										   
UPDATE all_sessions_new
SET totaltransactionrevenue = NULL
WHERE city = 'NULL'										   

SELECT * FROM all_sessions_new
WHERE productrefundamount = 'NULL'	
										   
ALTER TABLE all_sessions_new
DROP COLUMN productrefundamount
										   
SELECT * FROM all_sessions_new
WHERE productrevenue = 'NULL'	
										   
ALTER TABLE all_sessions_new
DROP COLUMN productrevenue	
										   
SELECT * FROM all_sessions_new
WHERE itemrevenue != 'NULL'	
										   
ALTER TABLE all_sessions_new
DROP COLUMN itemrevenue

SELECT * FROM all_sessions_new
WHERE itemquantity = 'NULL'	
										   
ALTER TABLE all_sessions_new
DROP COLUMN itemquantity										   
		
SELECT * FROM all_sessions_new
WHERE ecommerceaction_option != 'NULL'	
										   
--For analytics
SELECT * FROM analytics_new										   

SELECT * FROM analytics_new
WHERE userid = 'NULL'	
										   
ALTER TABLE analytics_new
DROP COLUMN userid	
							
SELECT * FROM analytics_new
WHERE units_sold != 'NULL'
										   
SELECT * FROM analytics_new
WHERE bounces != 'NULL'	
										   
SELECT * FROM analytics_new
WHERE revenue != 'NULL'

--for products										   
SELECT distinct(productsku) FROM products
										   
--for sales_by_sku
SELECT * FROM sales_by_sku_new
WHERE total_ordered IS NULL
										   
--for sales_report
SELECT * FROM sales_report_new	

SELECT * FROM all_sessions_new
WHERE EXISTS 
(SELECT productsku FROM products_new WHERE products_new.productsku=all_sessions_new.productsku)

ALTER TABLE all_sessions_new
ADD CONSTRAINT all_sessions_new_fk FOREIGN KEY (productsku) REFERENCES products_new(productsku)

UPDATE all_sessions_new
SET transactions = NULL
WHERE transactions = 'NULL'						  
	
ALTER TABLE all_sessions_new
ALTER COLUMN transactions
TYPE INTEGER
USING transactions::INTEGER					  
					  
UPDATE all_sessions_new
SET totaltransactionrevenue = NULL
WHERE totaltransactionrevenue = 'NULL'					  

ALTER TABLE all_sessions_new
ALTER COLUMN totaltransactionrevenue 
TYPE INTEGER
USING totaltransactionrevenue::INTEGER

UPDATE all_sessions_new
SET productquantity = NULL
WHERE productquantity = 'NULL'					  

ALTER TABLE all_sessions_new
ALTER COLUMN productquantity 
TYPE INTEGER
USING productquantity::INTEGER

UPDATE all_sessions_new
SET sessionqualitydim = NULL
WHERE sessionqualitydim = 'NULL'						  
	
ALTER TABLE all_sessions_new
ALTER COLUMN sessionqualitydim
TYPE INTEGER
USING sessionqualitydim::INTEGER	

UPDATE analytics_new
SET units_sold = NULL
WHERE units_sold = 'NULL'						  
	
ALTER TABLE analytics_new
ALTER COLUMN units_sold
TYPE INTEGER
USING units_sold::INTEGER