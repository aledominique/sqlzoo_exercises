# Self Join
Link to the tutorial: [Self Join](https://www.sqlzoo.net/wiki/Self_join).

### Name of the tables and columns
Table 1: stops
| id | name |
| --- | --- |
| ... |

Table 2: route
| num  | company |  pos  | stop | 
| --- | --- |--- | --- |
| ... |

### 1. How many stops are in the database.
```sql
SELECT COUNT(id) FROM stops

```

### 2. Find the id value for the stop 'Craiglockhart'
```sql
SELECT id FROM stops WHERE name =  'Craiglockhart'

```

### 3. Give the id and the name for the stops on the '4' 'LRT' service.
```sql
SELECT id, name,r.stop,r. pos from stops s JOIN route r ON (s.id=r.stop)
WHERE r.company="LRT" AND num=4
```

### 4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) =2
```

### 5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 AND b.stop=149
```

### 6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name="London Road"
```

### 7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
```sql
SELECT DISTINCT a.company, a.num
FROM route a JOIN route b ON
(a.company=b.company AND a.num=b.num)
WHERE a.stop=115 AND b.stop=137
```

### 8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```sql
SELECT a.company,a.num FROM route a JOIN route b ON (a.num=b.num AND a.company=b.company) 
JOIN stops stopa ON (a.stop=stopa.id)
JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name='Tollcross'
```

### 9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
```sql
SELECT stopb.name,a.company,a.num FROM route a JOIN route b ON (a.num=b.num AND a.company=b.company)
JOIN stops stopa ON (a.stop=stopa.id)
JOIN stops stopb ON (b.stop=stopb.id)
WHERE a.company = "LRT" AND stopa.name ='Craiglockhart' 
```

### 10. Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.
```sql
select DISTINCT Bus1.num, Bus1.company, Bus1.name, Bus2.num, Bus2.company from
(SELECT a.num, a.company,stopb.name from route a JOIN route b ON (a.num=b.num AND a.company=b.company)
JOIN stops stopa ON (a.stop=stopa.id)
JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name ='Craiglockhart') as Bus1
JOIN
(SELECT a.num, a.company,stopb.name FROM route a JOIN route b ON (a.num=b.num AND a.company=b.company)
JOIN stops stopa ON (a.stop=stopa.id)
JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name ='Lochend') as Bus2
ON Bus1.name=Bus2.name
ORDER BY Bus1.num, Bus1.company, Bus1.name, Bus2.num, Bus2.company
```
