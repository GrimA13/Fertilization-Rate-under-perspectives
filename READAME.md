# Game sales revenue analysis in 4 markets North America, Japan, Europe, and other (2006 to 2016)
(The Data credit from Ulrik Thyge Pedersen, thank you for sharing your [work]([url](https://www.kaggle.com/datasets/ulrikthygepedersen/video-games-sales/data)) )
## 1. Summary game sales revenue (millions dollar) in 4 market and percent of each market's sale to total sale
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
|----------------|----------------|----------------|------------------:|
|2377.94 (47.59%)|1504.60 (30.11%)|554.91 (11.11%) |558.90 (11.19%)

 North America has the biggest market share with $2377.94M (46.59%). Second place is EU with $554.91M (30.11%). Third and fourth sale are other and Japan are $554.91M (11.11%), $558.90M (11.19%) respectively

 ## 2. Which 5 gaming platforms were best-selling
 ```sql
SELECT platform, ROUND(SUM(global_sales),2) AS `Top 5 platform`
FROM video_games_sales
WHERE `year` BETWEEN 2006 AND 2016
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```
|platform|Top 5 platform|
|---|---:|
|X360	|961.29
|PS3	|949.35
|Wii	|909.81
|DS	    |669.79
|PS4	|278.07

Xbox360, PS3, Wii, DS, and PS4 are most the used platform that having most highest sale revenue

## 3. Which gaming platforms is the most favorite in 4 markets
```sql
WITH platform_sales AS (
  SELECT platform, SUM(na_sales) AS platform_na_sales, SUM(eu_sales) AS platform_eu_sales, 
         SUM(jp_sales) AS platform_jp_sales, SUM(other_sales) AS platform_other_sales
FROM video_games_sales
WHERE `year` BETWEEN 2006 AND 2016
GROUP BY 1
)
SELECT 
  (select platform from platform_sales
  order by platform_na_sales desc limit 1) AS `fav platform in NA`,
  (select platform from platform_sales
  order by platform_eu_sales desc limit 1) AS `fav platform in EU`,
  (select platform from platform_sales
  order by platform_jp_sales desc limit 1) AS `fav platform in JP`,
  (select platform from platform_sales
  order by platform_other_sales desc limit 1) AS `fav platform in others`;
```
|fav platform in NA|fav platform in EU|fav platform in JP|fav platform in others|
|---|---|---|---:|
|X360|PS3|DS|PS3|

The most favorite gaming platform in NA, EU, Japan, and other respectively is: Xbox360, PS3, DS, PS3

## 4. The bestselling game genre with total revenue of that genre in each market
```sql
WITH genre_sales AS (
  SELECT genre, SUM(na_sales) AS genre_na_sales, SUM(eu_sales) AS genre_eu_sales, 
         SUM(jp_sales) AS genre_jp_sales, SUM(other_sales) AS genre_other_sales
FROM video_games_sales
WHERE `year` BETWEEN 2006 AND 2016
GROUP BY 1
)
SELECT 
  (select concat(genre, " ", "(", round(genre_na_sales, 2),")") from genre_sales
  order by genre_na_sales desc limit 1) AS `NA fav genre`,
  (select concat(genre, " ", "(", round(genre_eu_sales, 2),")") from genre_sales
  order by genre_eu_sales desc limit 1) AS `EU fav genre`,
  (select concat(genre, " ", "(", round(genre_jp_sales, 2),")") from genre_sales
  order by genre_jp_sales desc limit 1) AS `JP fav genre`,
  (select concat(genre, " ", "(", round(genre_other_sales, 2),")") from genre_sales
  order by genre_other_sales desc limit 1) AS `Other fav genre`;
```
|NA fav genre|EU fav genre|JP fav genre|others fav genre|
|---|---|---|---:|
|Action (529.05)|Action (356.11)|Role-Playing (167.24)|Action (136.09)|

NA, EU, Japan, and other most favorite game genre, and revenues (million dollar) in the order: 
Action ($529.05M), Action ($356.11M), Role-Playing ($167.24M), Action ($136.09M)

