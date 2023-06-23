Question 1: 
--find the most used channelgrouping
SQL Queries:
SELECT channelgrouping, COUNT(channelgrouping) AS grp_count FROM analytics
GROUP BY channelgrouping
ORDER BY grp_count DESC
Answer: 
"Organic Search"	"2071832"
"Referral"	"1034520"
"Direct"	"725771"
"Social"	"206239"
"Paid Search"	"182751"


Question 2: 
--find the average time spent on site
SQL Queries:
SELECT AVG(timeonsite) FROM analytics_new
Answer:
"340.4114172385749140"


Question 3: 
--find the total number of unique visitid
SQL Queries:
SELECT COUNT(DISTINCT(visitid)) FROM analytics_new	
Answer:
"2847"


Question 4: 

SQL Queries:

Answer:


Question 5: 

SQL Queries:

Answer:


-- find all duplicate records
select fullvisitorid, count(fullvisitorid) as count_id from all_sessions_new
group by fullvisitorid										   
having count(fullvisitorid) > 1
order by count_id desc

Each records on the all_sessions table is unique but the fullvisitorid shows that there are multiple records with fullvisitorid because the distinct fullvisitorid is 1275

-- find the total number of unique visitors (`fullVisitorID`)
SELECT COUNT(DISTINCT(fullvisitorid)) from all_sessions_new	

The total number of unique visitors is 1275

-- find the total number of unique visitors by referring sites
select count(distinct(fullvisitorid)) from all_sessions
where channelgrouping = 'Referral' 

The total number of unique visitors by referring sites is 2419

-- find each unique product viewed by each visitor
SELECT distinct(v2productname) from all_sessions_new

Each unique product viewed by each visitor is 200
