# SELECT within SELECT
Link to the tutorial: [SELECT within SELECT](https://www.sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial).

### Name of the table and columns: world(name, continent, area, population, gdp)

### 1. Bigger than Russia
List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')

```

### 2. Richer than UK
Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT name FROM world
     WHERE continent ='Europe' AND gdp/population > 
         (SELECT gdp/population FROM world 
             WHERE name = 'United Kingdom')
# Remeber that we can do operations in the WHERE. 
```
### 3. Neighbours of Argentina and Australia
List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent 
FROM world
WHERE continent IN (
    SELECT continent 
    FROM world 
    WHERE name IN ('Australia', 'Argentina')
)
ORDER BY name;
#As we want more than one value we use "IN" instead of "="
```

### 4. Between Canada and Poland
Which country has a population that is more than United Kingdom but less than Germany? Show the name and the population.
```sql
SELECT name, population 
FROM world
WHERE population >  
             (SELECT population FROM world WHERE name = "United Kingdom") 
             AND
      population < (SELECT population FROM world WHERE name = "Germany")
```

### 5. Percentages of Germany
Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

The format should be Name, Percentage for example:
| name | percentenge |
| --- | --- |
| Albania | 3% |
| Andorra | 0% |
```sql
SELECT name, 
       CONCAT(ROUND(population * 100.0 / (SELECT population FROM world WHERE name = 'Germany'), 0), '%') AS percentage
FROM world 
WHERE continent = 'Europe';
#Also we can do formulas (like Excel formulas) and calculations in the SELECT. 
```

### 6. Bigger than every country in Europe
Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
SELECT name FROM world 
WHERE gdp > ALL (SELECT gdp FROM world WHERE continent="Europe" and gdp > 0) 

```

### 7. Find the largest country (by area) in each continent, show the continent, the name and the area:

The above example is known as a correlated or synchronized sub-query
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND population>0)

```

### 8. First country of each continent (alphabetically)
List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name 
FROM world x 
WHERE name = (SELECT MIN(name) FROM world y WHERE x.continent = y.continent);

```

### 9. Difficult Questions That Utilize Techniques Not Covered In Prior Sections
Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population 
FROM world 
WHERE continent IN (
    SELECT continent 
    FROM world 
    GROUP BY continent 
    HAVING MAX(population) <= 25000000
);
```

### 10. Three time bigger
Some countries have populations more than three times that of all of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent 
FROM world x 
WHERE population > ALL (SELECT 3*population FROM world y WHERE  x.continent= y.continent AND x.name!=y.name)
      
```
