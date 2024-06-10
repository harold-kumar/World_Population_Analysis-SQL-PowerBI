```sql
---CREATE TABLE NAMING WORLD POPULATION
	CREATE TABLE world_population (
	    Rank INT,
	    CCA3 CHAR(3),
	    Country_Territory VARCHAR(100),
	    Capital VARCHAR(100),
	    Continent VARCHAR(50),
	    Population_2022 BIGINT,
	    Population_2020 BIGINT,
	    Population_2015 BIGINT,
	    Population_2010 BIGINT,
	    Population_2000 BIGINT,
	    Population_1990 BIGINT,
	    Population_1980 BIGINT,
	    Population_1970 BIGINT,
	    Area_km2 BIGINT,
	    Density_per_km2 FLOAT,
	    Growth_Rate FLOAT,
	    World_Population_Percentage FLOAT
	);

---IMPORTING THE DATA
	
	---LOOKING AT THE DATA
	SELECT
	*
	FROM world_population
	
	
	/*
	CONTENT
	In this Dataset, we have Historical Population data for every Country/Territory in the world by different parameters 
	like Area Size of the Country/Territory, Name of the Continent, Name of the Capital, Density, Population Growth Rate, Ranking based on Population, 
	World Population Percentage, etc.
	
	Dataset Glossary (Column-Wise)
	1. Rank: Rank by Population.
	2. CCA3: 3 Digit Country/Territories Code.
	3. Country/Territories: Name of the Country/Territories.
	4. Capital: Name of the Capital.
	5. Continent: Name of the Continent.
	6. 2022 Population: Population of the Country/Territories in the year 2022.
	7. 2020 Population: Population of the Country/Territories in the year 2020.
	8. 2015 Population: Population of the Country/Territories in the year 2015.
	9. 2010 Population: Population of the Country/Territories in the year 2010.
	10. 2000 Population: Population of the Country/Territories in the year 2000.
	11. 1990 Population: Population of the Country/Territories in the year 1990.
	12. 1980 Population: Population of the Country/Territories in the year 1980.
	13. 1970 Population: Population of the Country/Territories in the year 1970.
	14. Area (km²): Area size of the Country/Territories in square kilometer.
	15. Density (per km²): Population Density per square kilometer.
	16. Growth Rate: Population Growth Rate by Country/Territories.
	17. World Population Percentage: The population percentage by each Country/Territories.
	*/
```

```sql
-- LIST OF COUNTRIES THAT BELONG TO CERTAIN CONTINENT
	SELECT * FROM world_population
	WHERE Continent = 'Asia';
```

```sql
--- CONTINENT WISE POPULATION IN 2022
	select
		Continent,
		sum(population_2022) as population
	from world_population
	group by Continent
	order by population desc
```


![Screenshot (248) (5)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/27764b4c-bd80-419f-bcbe-aafd7fa817be)



```sql
--PERCENTAGE OF POPULATION HOLD BY DIFFERENT CONTINENT IN 2022
	SELECT 
	    Continent, 
	    SUM(Population_2022) AS total_population_2022,
	    ROUND((SUM(Population_2022) / (SELECT SUM(Population_2022) FROM world_population) * 100),2) AS population_percentage
	FROM 
	    world_population
	GROUP BY 
	    Continent
	ORDER BY population_percentage DESC;
```
![Screenshot (250) (1)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/55027092-78c5-47df-8e2a-891a78ce9f3b)


```sql
-- POPULATION PERCENTAGE HOLD BY DIFFERENT COUNTRIES 2022 (limit 5 means top 5) most populous country with population percentage
	SELECT
	 Country_Territory,
	 SUM(population_2022) as total_population_2022,
	 ROUND((SUM(population_2022) /( SELECT SUM(population_2022) FROM world_population) * 100),2) AS  population_percentage
	FROM 
	 world_population 
	GROUP BY Country_Territory
	ORDER BY population_percentage desc
	LIMIT 5;
```
![Screenshot (251) (1)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/545cc4bb-ad6f-4d7b-9475-46fd66d93ccc)


                                                            ---------DENSITY---------
	--Population density is a measure of the number of people living per unit area, typically expressed as people per square kilometer (km²). 
	--It provides an estimate of how crowded or sparsely populated a given area is.

```sql
-- 1. AVERAGE DENSTIY OF ALL COUNTRIES (can be done with continents as well)
	SELECT 
		AVG(Density_per_km2) AS Average_Density
	FROM world_population
	WHERE Density_per_km2 IS NOT NULL;        
```
-MEANS 452.127 PEOPLE LIVES PER SQUARE KM

```sql
---2. HIGHEST DENSTIY AREA COUNTRY WISE
	SELECT 
	    Country_Territory, 
	    Density_per_km2
	FROM 
	    world_population
	ORDER BY 
	    Density_per_km2 DESC
	LIMIT 10;                    ---23172 people live in per square km in macau
```
![Screenshot (252) (1)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/14c6e691-d019-4070-8acb-1138f4ab7b57)


```sql
---3. LOWEST DENSITY AREA COUNTRY WISE
	SELECT 
	    Country_Territory, 
	    Density_per_km2
	FROM 
	    world_population
	ORDER BY 
	    Density_per_km2 ASC
	LIMIT 10;
```

```sql
--- COUNTRIES WITH HIGHT POPULATION DENSTIY AFTER CERTAIN THRESHOLD LET SAY 1000 PEOPLE PER SQUARE KM
	SELECT 
		Country_Territory, Density_per_km2
	FROM 
		world_population
	WHERE 
		Density_per_km2 > 1000
	ORDER BY Density_per_km2 DESC;
	
	--- COMPARE THE POPULATION DENSITY OF WHOLE COUNTRY RESPECTIVE TO THIER CAPITALS
	--- THIS CANNOT BE DONE BECAUSE CAPITAL DENSITY OR POPULATION IS NOT GIVEN, SO SAME DENSITY OF CAPITAL AND REST OF WHOLE COUNTRY
```	

-------------------------------GROWTH RATE--------------------------------------------------------------------------------
```sql
---3. CONTINENT GROWTH RATE(avg)
	SELECT Continent, 
		AVG(Growth_Rate) AS Average_Growth_Rate     --- sum can also be seen
	FROM world_population 
	GROUP BY Continent;
```
![Screenshot (254) (1)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/f4687c1d-12e8-4cd8-8258-2f263054c7a2)


```sql
---1. COUNTRIES WITH HIGHEST POPULATION GROWTH 
  SELECT
	Country_Territory,
	Growth_Rate
	FROM world_population
  WHERE Growth_Rate is not null
	order by Growth_Rate desc
	limit 10;
```

```sql
--- lOWEST GROWHT RATE BY COUNTRY TOP 10
	select
	Country_Territory,
	Growth_Rate
	from world_population
	where Growth_Rate is not null
	order by Growth_Rate asc
	limit 10;
```

------------------------------------- GROWTH RATE ANALYSIS--------------------------------------------
	
```sql
 DROP TABLE IF EXISTS world_population2;
	CREATE TABLE world_population2 (
	    Rank INT,
	    CCA3 CHAR(3),
	    Country_Territory VARCHAR(100),
	    Capital VARCHAR(100),
	    Continent VARCHAR(50),
	    Population_2022 FLOAT,
	    Population_2020 FLOAT,
	    Population_2015 FLOAT,
	    Population_2010 FLOAT,
	    Population_2000 FLOAT,
	    Population_1990 FLOAT,
	    Population_1980 FLOAT,
	    Population_1970 FLOAT,
	    Area_km2 FLOAT,
	    Density_per_km2 FLOAT,
	    Growth_Rate FLOAT,
	    World_Population_Percentage FLOAT
	);--- new table create just changed data type for following calculation

```	
```sql
---- GROWTH PERCENTAGE COUNTRY WISE FROM 2000 TO 2022 (DESCRESING ORDER)
	SELECT 
	    Country_Territory,
	    population_2020,
	    population_2022,
	    ((population_2022 - population_2000) / population_2000) * 100 AS Growth_Percentage
	FROM 
	    world_population2
	ORDER BY
	    Growth_Percentage desc;
```
![Screenshot (255) (1) (1)](https://github.com/harold-kumar/World_Population_Analysis-SQL-PowerBI/assets/84094260/49eb73da-b2ab-4a9d-bf17-7d94ad3917a8)


```sql
---- GROWTH PERCENTAGE FO DIFFERENT COUNTRIES (ASCENDING ORDER) COUNTRIES WITH LEAST GROWHT FROM 2000 TO 2022 
	SELECT 
	    Country_Territory,
	    population_2020,
	    population_2022,
	    ((population_2022 - population_2000) / population_2000) * 100 AS Growth_Percentage
	FROM 
	    world_population2
	ORDER BY
	    Growth_Percentage asc;
	
	
	---- GROWTH PERCENTAGE OF COUNTRY FROM 2020 TO 2022 (LATEST HOW IT IS GROWING FOR DIFFERENT COUNTRIES)
	SELECT 
	    Country_Territory,
	    population_2020,
	    population_2022,
	    ((population_2022 - population_2020) / population_2020) * 100 AS Growth_Percentage
	FROM 
	    world_population2
	ORDER BY
	    Growth_Percentage desc;
	
	---- LEAST GROWTH COUNTRIES IN RECENT YEAR OF 2020 TO 2022
	SELECT 
	    Country_Territory,
	    population_2020,
	    population_2022,
	    ((population_2022 - population_2020) / population_2020) * 100 AS Growth_Percentage
	FROM 
	    world_population2
	ORDER BY
	    Growth_Percentage asc;
```


----------------------------------------------------------PROJECTION ANALYSIS----------------------------------
```sql
	---1. Calculate the historical growth rate from 2000 to 2022
	SELECT 
	    Country_Territory,
	    CASE 
	        WHEN Population_2000 = 0 THEN NULL  
	        ELSE ((Population_2022 - Population_2000) / Population_2000) * 100  
	    END AS Growth_Rate_2000_2022
	FROM 
	    world_population2
	WHERE 
	    Population_2000 IS NOT NULL 
	    AND Population_2022 IS NOT NULL;
	
	
	--2. Use the calculated growth rate to project the population for 2032
	SELECT 
	    Country_Territory,
	    Population_2022,
	    Population_2022 * POWER((1 + Growth_Rate / 100), 10) AS Projected_Population_2032
	FROM (
	    SELECT 
	        Country_Territory,
	        Population_2022,
	        CASE 
	            WHEN Population_2000 = 0 THEN NULL  
	            ELSE ((Population_2022 - Population_2000) / Population_2000) * 100  
	        END AS Growth_Rate
	    FROM 
	        world_population
	    WHERE 
	        Population_2000 IS NOT NULL 
	        AND Population_2022 IS NOT NULL
	) AS growth_rates
	ORDER BY 
	    Projected_Population_2032 DESC;
```

----CREATE NEW TABLE CALLED world_statistics CONTAINING GDP_PER_CAPITA AND MEDIAN AGE (TABLE GENERATED BY ME NOT AVAILABLE ON ANY WEBSTIE
----TO SEE THE EFFECT OF GDP_PER_CAPITA AND MEDIAN_AGE ON POPULATION 

select * from world_population-- TABLE FROM ABOVE

```sql
CREATE TABLE country_statistics (
    CCA3 CHAR(3) PRIMARY KEY,
    Median_Age FLOAT,
    GDP_per_Capita FLOAT
);
```

```sql
----COUNTRIES LOWEST GDP PER CAPITA
SELECT 
wp.Country_territory,
cs.GDP_per_Capita
FROM world_population wp
JOIN country_statistics cs 
ON wp.CCA3 = cs.CCA3
ORDER BY cs.GDP_per_Capita ASC
LIMIT 10;


----COUNTRIES HIGHEST MEDIAN AGE
SELECT
wp.Country_territory,
cs.Median_Age
FROM world_population wp
JOIN country_statistics cs 
ON wp.CCA3 = cs.CCA3
ORDER BY cs.Median_Age DESC
LIMIT 10;


----COUNTRIES LOWEST MEDIAN AGE
SELECT
wp.Country_territory,
cs.Median_Age
FROM world_population wp
JOIN country_statistics cs 
ON wp.CCA3 = cs.CCA3
ORDER BY cs.Median_Age ASC
LIMIT 10;
```

-------------------------------------------------CORRRELATION ANALYSIS------------------------------

```sql
-- Calculate correlation between median age, GDP per capita, and population changes
SELECT
    CORR(cs.Median_Age, wp.Population_2022) AS Correlation_MedianAge_Population,
    CORR(cs.GDP_per_Capita, wp.Population_2022) AS Correlation_GDP_Population
FROM
    world_population wp
JOIN
    country_statistics cs ON wp.CCA3 = cs.CCA3;


/* Correlation between Median Age and Population Changes: 0.029507805865376213

This correlation coefficient indicates a very weak positive correlation between the median age of a country's population and its population changes in 2022.
In other words, there's a slight tendency for countries with higher median ages to have slightly larger population increases, but the correlation is not strong.
Correlation between GDP per Capita and Population Changes: -0.04835662479493124

This correlation coefficient indicates a very weak negative correlation between a country's GDP per capita and its population changes in 2022. 
It suggests a slight tendency for countries with higher GDP per capita to have slightly smaller population increases, but again, the correlation is very weak.
In summary, based on these correlation coefficients, there doesn't seem to be a strong relationship between median age, GDP per capita, and population changes in 2022.
However, it's important to note that correlation does not imply causation, and other factors may influence population dynamics.

*/
```
