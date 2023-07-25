# SQL-Deforestation-Project
In this project whose mission is to combat deforestation around the world and to raise awareness
about this topic and its impact on the environment,I obtained data from the World Bank that includes forest area and total land area by country and year from 1990 to 2016, as well as a table of countries and the regions to which they belong.
I used SQL to bring these tables together and to query them in an effort to find areas of concern as well as areas that present an opportunity to learn from successes.
The following is a report I created to answer several of these questions by querrying the data using PostgreSQL.
The index for all the SQL querries used is provided below the report.

1. GLOBAL SITUATION
According to the World Bank, the total forest area of the world was 41,282,694.9 in 1990. As
of 2016, the most recent year for which data was available, that number had fallen to
39,958,245.9, a loss of 1,324,449, or 3.21%.
The forest area lost over this time period is slightly more than the entire land area of Peru
listed for the year 2016 (which is 1279999.99).

2. REGIONAL OUTLOOK
In 2016, the percent of the total land area of the world designated as forest was 31.38%. The
region with the highest relative forestation was Latin America & caribbean, with 46.16%, and the
region with the lowest relative forestation was Middle East & North Africa, with 2.07% forestation.
In 1990, the percent of the total land area of the world designated as forest was 42.02%. The
region with the highest relative forestation was Latin America & Caribbean, with 51.03%, and the
region with the lowest relative forestation was Middle East & North Africa, with 1.78% forestation

                     Percent Forest Area by Region, 1990 & 2016:
Region                        1990 Forest Percentage      2016 Forest Percentage
Latin America & Caribbean     51.03                       46.16
Middle East & North Africa    2.07                        1.78
The only regions of the world that decreased in percent forest area from 1990 to 2016 were
Latin America & Caribbean and (dropped from 51.03% to 46.16%) and Sub-Saharan Africa
(30.67% to 28.79%) All other regions actually increased in forest area over this time period.
However, the drop in forest area in the two aforementioned regions was so large, the percent
forest area of the world decreased over this time period from 32.42% to 31.38%.

3. COUNTRY-LEVEL DETAIL
A. SUCCESS STORIES
There is one particularly bright spot in the data at the country level, China. This country actually
increased in forest area from 1990 to 2016 by 527229.062. It would be interesting to study what
has changed in this country over this time to drive this figure in the data higher. The country with
the next largest increase in forest area from 1990 to 2016 was the United States, but it only saw
an increase of 79200, much lower than the figure for China.
China and The United States are of course very large countries in total land area, so when we
look at the largest percent change in forest area from 1990 to 2016, we aren’t surprised to find a
much smaller country listed at the top.Iceland increased in forest area by 213.67% from 1990 to
2016.
B. LARGEST CONCERNS
Which countries are seeing deforestation to the largest degree? We can answer this question in
two ways. First, we can look at the absolute square kilometer decrease in forest area from 1990
to 2016. The following 3 countries had the largest decrease in forest area over the time period
under consideration:
Top 5 Amount Decrease in Forest Area by Country, 1990 & 2016:
Country                                Region                                     Absolute Forest Area Change
Brazil                                Latin America and Caribbean                 -541510.00
Indonesia                             East Asia and pacific                       -282193.98
Myanmar                               East Asia and Pacific                       -107234.00
Nigeria                               Sub-saharan Africa                          -106506.00
Tanzania                              Sub-saharan Africa                          -102320.00

Top 5 Percent Decrease in Forest Area by Country, 1990 & 2016:
Country                               Region                                       Pct Forest Area Change
St. Martin (French part)              Latin America and Caribbean                  -100.00
Togo                                  Sub-saharan Africa                           -75.45
Nigeria                               Sub-saharan Africa                           -61.80
Uganda                                Sub-saharan Africa                           -59.13
Mauritania                            Sub-saharan Africa                           -46.75
When we consider countries that decreased in forest area percentage the most between 1990 and 2016, we find that four of the top 5 countries on the list are in the region of Sub-saharan Africa. The countries are Togo,Nigeria,Uganda and Mauritania. 
The 5th country on the list is St.Martin(French Part), which is in the Latin America and Caribbean region.
From the above analysis, we see that Nigeria is the only country that ranks in the top 5 both in terms of absolute square kilometer decrease in forest as well as percent decrease in forest area from 1990 to 2016. Therefore, this country has a significant opportunity ahead to stop the decline and hopefully spearhead remedial efforts.

C. QUARTILES
Count of Countries Grouped by Forestation Percent Quartiles, 2016:
Quartile                                       Number of Countries
First                                                  85
Second                                                 72
Third                                                  38
Fourth                                                  9
The largest number of countries in 2016 were found in the First quartile.
There were 85 countries in the top quartile in 2016. These are countries with a very high percentage of their land area designated as forest. The following is a list of countries and their respective forest land, denoted as a percentage.
     Top Quartile Countries, 2016:
Country                           Region                                 Pct Designated as Forest
Chile                             Latin America andCaribbean                          24.26
British Virgin Islands            Latin America and Caribbean                         24.13
India                             South Asia                                          23.83

4. RECOMMENDATIONS
● What have you learned from the World Bank data?
It is evident from the data that the world’s forest cover is dwindling,Most notably however, only two regions had a decrease in forest cover. There needs to be a deeper dive into these regions to determine what practices need to be stopped or changed to reduce deforestation in these areas and consequently in the whole world.
● Which countries should we focus on over others?
The countries of interest are mainly Nigeria, St. Martin, Togo , Uganda and Mauritania as they have the highest rate of deforestation.

APPENDIX: SQL Queries Used

1.
DROP VIEW IF EXISTS Forestation;
CREATE VIEW forestation AS
(
SELECT f.country_code,f.country_name,f.year,f.forest_area_sqkm,
(l.total_area_sq_mi*2.59) AS total_area_sqkm, r.income_group,r.region
FROM forest_area AS f
INNER JOIN land_area AS l
ON f.country_code=l.country_code
AND f.year=l.year
INNER JOIN regions AS r
ON f.country_code=r.country_code

1.
SELECT forest_area_sqkm
FROM Forestation
WHERE year=1990 AND region='World';

2.
SELECT forest_area_sqkm
FROM Forestation
WHERE year=2016 AND region='World';

3.
WITH forest_area_sqkm AS
(
SELECT Year, forest_area_sqkm
FROM forestation
WHERE Year IN (1990, 2016)
)
SELECT
(MAX(CASE WHEN Year = 1990 THEN forest_area_sqkm END)-MAX(CASE WHEN Year =
2016 THEN forest_area_sqkm END)) AS loss,
((MAX(CASE WHEN Year = 1990 THEN forest_area_sqkm END) - MAX(CASE WHEN Year =
2016 THEN forest_area_sqkm END)) / MAX(CASE WHEN Year = 1990 THEN
forest_area_sqkm END)) * 100 AS percent_loss
FROM forestation;

4.
WITH forest_area_sqkm AS
(
SELECT Year, forest_area_sqkm
FROM forestation
WHERE Year IN (1990, 2016)
)
SELECT
(MAX(CASE WHEN Year = 1990 THEN forest_area_sqkm END)-MAX(CASE WHEN Year =
2016 THEN forest_area_sqkm END)) AS loss,
((MAX(CASE WHEN Year = 1990 THEN forest_area_sqkm END) - MAX(CASE WHEN Year =
2016 THEN forest_area_sqkm END)) / MAX(CASE WHEN Year = 1990 THEN
forest_area_sqkm END)) * 100 AS percent_loss
FROM forestation;

5.
SELECT country_name, total_area_sqkm
FROM forestation
WHERE year=2016 AND total_area_sqkm<1324449
ORDER BY total_area_sqkm DESC;

6.
SELECT country_name,
ROUND(total_area_sqkm::NUMERIC,2) AS total_area_2016
FROM forestation
WHERE year=2016 AND total_area_sqkm<1324449
ORDER BY total_area_sqkm DESC;

REGIONAL OUTLOOK

1.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_2016
FROM forestation
WHERE year=2016 and region='World'
GROUP BY 1
ORDER BY 2;

2.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_2016
FROM forestation
WHERE year=2016
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;

3.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_2016
FROM forestation
WHERE year=2016
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;

4.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_2016
FROM forestation
WHERE year=2016
GROUP BY 1
ORDER BY 2
LIMIT 1;

5.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_2016
FROM forestation
WHERE year=2016
GROUP BY 1
ORDER BY 2
LIMIT 1;

6.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_1990
FROM forestation
WHERE year=1990 and region='World'
GROUP BY 1
ORDER BY 2;

7.
SELECT region,
(sum(forest_area_sqkm)*100)/sum(total_area_sqkm) as Pct_fa_1990
FROM forestation
WHERE year=1990
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;

9. , 9., 10., 11., 12., 13., 14., 15.
WITH forest_area_by_region AS
(
SELECT
region,
SUM(CASE
WHEN year = 1990 THEN forest_area_sqkm ELSE 0 END) * 100 /
SUM(CASE
WHEN year = 1990 THEN total_area_sqkm ELSE 0 END) AS pctfa_1990,
SUM(CASE
WHEN year = 2016 THEN forest_area_sqkm ELSE 0 END) * 100 /
SUM(CASE
WHEN year = 2016 THEN total_area_sqkm ELSE 0 END) AS pctfa_2016
FROM forestation
GROUP BY region
)
SELECT
region,pctfa_1990,pctfa_2016,
ROUND((pctfa_1990 - pctfa_2016)::numeric,2) AS pct_change
FROM forest_area_by_region
WHERE pctfa_1990 > 0 AND pctfa_2016 < pctfa_1990;

COUNTRY-LEVEL DETAIL
SUCCESS STORIES

1.
WITH forest_area_by_country AS (
SELECT
country_name,
SUM(CASE
WHEN year = 1990 THEN forest_area_sqkm ELSE 0 END) AS fa_1990,
SUM(CASE
WHEN year = 2016 THEN forest_area_sqkm ELSE 0 END) AS fa_2016
FROM
Forestation
GROUP BY
country_name
)
SELECT
country_name, fa_1990,fa_2016,(fa_2016-fa_1990) As Fa_growth
FROM
forest_area_by_country
WHERE
fa_1990 > 0
AND fa_1990 <fa_2016
ORDER BY Fa_growth DESC

2.
WITH forest_area_by_country AS
(
SELECT
country_name,
SUM(CASE
WHEN year = 1990 THEN forest_area_sqkm ELSE 0 END) AS fa_1990,
SUM(CASE
WHEN year = 2016 THEN forest_area_sqkm ELSE 0 END) AS fa_2016
FROM
forestation
GROUP BY
country_name
)
SELECTcountry_name,
((fa_2016-fa_1990)/fa_1990)*100 As Fa_growth
FROM forest_area_by_country
WHERE fa_1990 > 0 AND fa_1990 <fa_2016
ORDER BY Fa_growth DESC
LIMIT 1;

LARGEST CONCERNS

1. Sqkm decrease
   
WITH forest_area_by_country AS (
SELECT
country_name,region,
SUM(CASE
WHEN year = 1990 THEN forest_area_sqkm ELSE 0 END) AS fa_1990,
SUM(CASE
WHEN year = 2016 THEN forest_area_sqkm ELSE 0 END) AS fa_2016
FROM
forestation
GROUP BY
country_name,region
)
SELECT
country_name,region,(fa_2016-fa_1990) As Absolute_fa_change
FROM
forest_area_by_country
WHERE
fa_1990 > 0
AND fa_1990 >fa_2016
ORDER BY Absolute_fa_change
LIMIT 6;

3. Percentage decrease
   
WITH forest_area_by_country AS
(
SELECT
country_name,region,
SUM(CASE
WHEN year = 1990 THEN forest_area_sqkm ELSE 0 END) AS fa_1990,
SUM(CASE
WHEN year = 2016 THEN forest_area_sqkm ELSE 0 END) AS fa_2016
FROM forestation
GROUP BY country_name,region
)
SELECT
country_name,region,
((fa_2016-fa_1990)/fa_1990)*100 As Absolute_fa_change
FROM forest_area_by_country
WHERE fa_1990 > 0
AND fa_1990 >fa_2016
ORDER BY Absolute_fa_change
LIMIT 5;

QUARTILES

WITH fa_pct AS
(
SELECT country_name,region,year,(FOREST_AREA_SQKM/TOTAL_AREA_SQKM)*100 AS
pct_fa
FROM forestation
),
quart_range AS
(
SELECT country_name,
CASE
WHEN pct_fa BETWEEN 75 AND 100 THEN 'Fourth'
WHEN pct_fa<=75 AND pct_fa>50 THEN 'Third'
WHEN pct_fa<=50 AND pct_fa>25 THEN 'Second' ELSE 'First'
END AS quartile_range
FROM fa_pct
WHERE year=2016 AND region<>'World' AND pct_fa IS NOT NULL
)
SELECT
quartile_range,COUNT(country_name) AS No_of_countries
FROM
quart_range
GROUP BY 1
ORDER BY 2 DESC

2.
WITH fa_pct AS
(
SELECT country_name,region,year,(FOREST_AREA_SQKM/TOTAL_AREA_SQKM)*100 AS
pct_fa
FROM forestation
),
quart_range AS
(
SELECT
country_name, region,pct_fa,
CASE
WHEN pct_fa BETWEEN 75 AND 100 THEN 'Fourth'
WHEN pct_fa<=75 AND pct_fa>50 THEN 'Third'
WHEN pct_fa<=50 AND pct_fa>25 THEN 'Second'
ELSE 'First' END AS quartile_range
FROM fa_pct
WHERE year = 2016
AND region <> 'World'
AND pct_fa IS NOT NULL
)
SELECT qa.country_name,qa.region, fp.pct_fa
FROM quart_range qa
INNER JOIN fa_pct fp
ON qa.country_name = fp.country_name
AND qa.pct_fa = fp.pct_fa
WHERE quartile_range = 'First'
ORDER BY qa.pct_fa DESC;


