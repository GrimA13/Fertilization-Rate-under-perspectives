# Game sales revenue analysis in 4 markets North America, Japan, Europe, and other (2006 to 2016)
## 1. Summary game sales revenue in 4 market
```sql
WITH 
  total_revenue AS (
    SELECT 
	 CAST(SUM(na_sales) AS DECIMAL(10,2)) AS total_na_sales,
	 CAST(SUM(eu_sales) AS DECIMAL(10,2)) AS total_eu_sales,
     CAST(SUM(jp_sales) AS DECIMAL(10,2)) AS total_jp_sales,
     CAST(SUM(other_sales) AS DECIMAL(10,2)) AS total_other_sales,
     CAST(SUM(na_sales) + SUM(eu_sales) + SUM(jp_sales) + SUM(other_sales) 
       AS DECIMAL(10,2)) AS total_region_sales
    FROM video_games_sales
    WHERE `year` BETWEEN 2006 AND 2016
)
SELECT 
  CONCAT(total_na_sales," ", "(", ROUND(total_na_sales/total_region_sales *100, 2), "%", ")") AS `total (%) in NA`,  
  CONCAT(total_eu_sales," ", "(", ROUND(total_eu_sales/total_region_sales *100, 2), "%", ")") AS `total (%) in EU`,
  CONCAT(total_jp_sales," ", "(", ROUND(total_jp_sales/total_region_sales *100, 2), "%", ")") AS `total (%) in JP`,
  CONCAT(total_other_sales," ", "(", ROUND(total_other_sales/total_region_sales *100, 2) , "%", ")") AS `total (%) in others`
FROM total_revenue;
```
|Total (%) in NA |Total (%) in EU | Total (%) in JP|Total (%) in others|
-----------------|----------------|----------------|------------------:|
|2377.94 (47.59%)|1504.60 (30.11%)|554.91 (11.11%) |558.90 (11.19%)

 North America has the biggest market share with 46.59%

 ## 2. Which 5 game platforms were best-selling
 ```sql
SELECT platform, ROUND(SUM(global_sales),2) AS `Top 5 platform`
FROM video_games_sales
WHERE `year` BETWEEN 2006 AND 2016
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
platform|Top 5 platform|
|---|---:|
|X360	|961.29
|PS3	|949.35
|Wii	|909.81
|DS	  |669.79
|PS4	|278.07
