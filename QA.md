What are your risk areas? Identify and describe them.
The risk area is trying to identify each columns and made assumptions on how they contribute to the table or dataset. A proper documentation on what each column stands for or represent will go a long way to help understand the tables and datasets.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
Tables are related using the primary and foreign key constraints
I checked to validate if the primary and foreign key match

SELECT p.productsku, s.productsku
FROM products_new p
INNER JOIN sales_by_sku_new s
USING(productsku)

SELECT p.productsku, al.productsku
FROM products_new p
INNER JOIN all_sessions_new al
USING(productsku)

SELECT an.visitid, al.visitid
FROM analytics_new an
INNER JOIN all_sessions_new al
USING(visitid)	

SELECT sr.productsku, s.productsku
FROM sales_report sr
INNER JOIN sales_by_sku_new s
USING(productsku)	