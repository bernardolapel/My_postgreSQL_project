Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
select city, country,sum(totaltransactionrevenue) as rev_level from all_sessions_new
group by city, country
having sum(totaltransactionrevenue) > 1 					  
order by rev_level desc


Answer:
"not available in demo dataset"	"United States"	"1198960000"
"Sunnyvale"	"United States"	"672230000"
"Seattle"	"United States"	"358000000"
"San Francisco"	"United States"	"312680000"
"Chicago"	"United States"	"306000000"
"Palo Alto"	"United States"	"151000000"



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
--if the ordered product quantity is from the product table
SELECT al.city, al.country, AVG(p.orderedquantity) AS avg_order
FROM products_new p
JOIN all_sessions_new al
USING(productsku)
GROUP BY al.city, al.country
ORDER BY avg_order DESC

--if the total product ordered is from the sales_by_sku table
SELECT al.city, al.country, AVG(s.total_ordered) AS avg_order
FROM sales_by_sku_new s
JOIN all_sessions_new al
USING(productsku)
GROUP BY al.city, al.country
ORDER BY avg_order DESC	


Answer:
--first query
"Detroit"	"United States"	"10075.0000000000000000"
"not available in demo dataset"	"Mali"	"3786.0000000000000000"
"Bellflower"	"United States"	"3786.0000000000000000"
"Santa Fe"	"Argentina"	"3682.0000000000000000"
"not available in demo dataset"	"Honduras"	"2719.0000000000000000"
"not available in demo dataset"	"Réunion"	"2538.0000000000000000"

--second query
"Sacramento"	"United States"	"189.0000000000000000"
"Rio de Janeiro"	"Brazil"	"189.0000000000000000"
"Rexburg"	"United States"	"167.0000000000000000"
"Shinjuku"	"Japan"	"117.3333333333333333"
"not available in demo dataset"	"Honduras"	"112.0000000000000000"
"not available in demo dataset"	"Ethiopia"	"85.0000000000000000"

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT DISTINCT(v2productcategory), city, count(city) as city_count, country, count(country) as country_count
FROM all_sessions_new
GROUP BY v2productcategory, city, country
HAVING v2productcategory != '(not set)' AND v2productcategory != '${escCatTitle}'
ORDER BY country_count

SELECT DISTINCT(v2productcategory), count(v2productcategory) as cat_count
FROM all_sessions_new
GROUP BY v2productcategory
HAVING v2productcategory != '(not set)' AND v2productcategory != '${escCatTitle}'
ORDER BY cat_count DESC	


Answer:
It is not clear or easy to draw a pattern for product category for city and country based on the first query. The second query shows that the "Home/Shop by Brand/YouTube/" category is more popular across the cities and countries




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
SELECT p.productname, sum(an.units_sold) AS total_sold, al.city, al.country 
FROM products p
JOIN all_sessions_new al
USING(productsku)
JOIN analytics_new an
USING(visitid)
GROUP BY p.productname,al.city, al.country
HAVING sum(an.units_sold) > 1
ORDER BY total_sold DESC
LiMIT 1

Answer:
"SPF-15 Slim & Slender Lip Balm"	"168"	"Sunnyvale"	"United States"
The pattern worth noting is that the topmost products sold are from visitors in the United states




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT city, country, SUM(totaltransactionrevenue) AS total_rev
FROM all_sessions_new
GROUP BY city, country
HAVING SUM(totaltransactionrevenue) >= 1
ORDER BY total_rev DESC			


Answer:
The impact of revenue generated from each city and country shows that most revenues were generated from the cities based in the United States.






