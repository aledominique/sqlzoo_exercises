# JOIN operation
Link to the tutorial: [JOIN operation](https://www.sqlzoo.net/wiki/The_JOIN_operation).

### Name of the tables and columns
Table 1: game
| id | mdate | stadium | team1 | team2 |
| --- | --- | --- | --- | --- |
| 1001 | 8 June 2012 | National Stadium, Warsaw	 | POL | GRE |
| 1002 | 8 June 2012 | Stadion Miejski (Wroclaw)	| RUS | CZE |
| ... |


Table 2: goal
| matchid | teamid | player | gtime |
| --- | --- | --- | --- |
| 1001 | POL | Robert Lewandowski | 17 |
| 1001 | GRE | Dimitris Salpingidis	 | 51 |
| ... |

Table 3: eteam
| id | teamname | coach | 
| --- | --- | --- |
| POL | Poland | 	Franciszek Smuda |
| RUS | Russia | 	Dick Advocaat |
| ... |

### Relation between all the tables: 
![image](https://github.com/aledominique/sqlzoo_exercises/assets/93596082/655933e8-b477-4f03-adee-3c36fc18d665)


### 1. The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT matchid, player FROM goal 
  WHERE teamid LIKE 'GER' 
```

### 2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.
Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.
Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id,stadium,team1,team2
  FROM game 
  WHERE id = 1012
```

### 3. You can combine the two steps into a single query with a JOIN
The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in game go with which rows in goal - the matchid from goal must match id from game. (If we wanted to be more clear/specific we could say
  ```sql
  ON (game.id=goal.matchid)
```
The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.
Modify it to show the player, teamid, stadium and mdate for every German goal.
  ```sql
  SELECT go.player,go.teamid,ga.stadium,ga.mdate 
  FROM game ga JOIN goal go ON (id=matchid)
   WHERE go.teamid = "GER"
```

### 4. Use the same JOIN as in the previous question.
Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
  ```sql
  SELECT ga.team1, ga.team2, go.player
FROM game ga JOIN goal go ON (id=matchid)
WHERE go.player like "Mario%"
```

### 5. The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id
Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
  ```sql
 SELECT go.player, go.teamid, e.coach ,go.gtime
  FROM goal go JOIN eteam e ON (teamid=id)
 WHERE gtime<=10
```

### 6. To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)

Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id

List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
  ```sql
 SELECT ga.mdate, e.teamname 
FROM game ga JOIN eteam e ON (ga.team1=e.id)
WHERE e.coach="Fernando Santos"
```

### 7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
  ```sql
SELECT go.player
FROM goal go JOIN game ga ON (matchid=id)
WHERE ga.stadium ='National Stadium, Warsaw'
```

### 8. The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.
  ```sql
SELECT DISTINCT go.player
  FROM game ga JOIN goal go ON (ga.id=go.matchid)
    WHERE (ga.team1='GER' OR ga.team2='GER') 
    AND go.teamid<>'GER'
# We remove GER from the logic (go.teamid<>'GER') if not we are going to have also the german players on the result 
```

### 9. Show teamname and the total number of goals scored.
  ```sql
SELECT e.teamname, COUNT(player) as "total number of goals scored"
  FROM eteam e JOIN goal go ON id=teamid
  GROUP BY e.teamname
 ORDER BY e.teamname

```

### 10. Show the stadium and the number of goals scored in each stadium.
  ```sql
SELECT ga.stadium, COUNT(player) as "number of goals"
FROM game ga JOIN goal go ON id=matchid 
GROUP BY ga.stadium

```

### 11. For every match involving 'POL', show the matchid, date and the number of goals scored. 
  ```sql
SELECT matchid,ga.mdate, COUNT(player)
  FROM game ga JOIN goal go ON matchid = id 
 WHERE (ga.team1 = 'POL' OR ga.team2 = 'POL')
 GROUP BY ga.mdate
 ORDER BY matchid

```

### 12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
 ```sql
SELECT go.matchid, ga.mdate, COUNT(player)
FROM game ga JOIN goal go ON matchid=id 
WHERE go.teamid ="GER"
GROUP BY ga.mdate
ORDER BY go.matchid

```

### 13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your result by mdate, matchid, team1 and team2.
 ```sql
SELECT ga.mdate, ga.team1, 
    COALESCE(SUM(CASE WHEN go.teamid = ga.team1 THEN 1 ELSE 0 END), 0) AS score1,
ga.team2,
    COALESCE(SUM(CASE WHEN go.teamid = ga.team2 THEN 1 ELSE 0 END), 0) AS score2
FROM game ga
LEFT JOIN goal go ON ga.id = go.matchid
GROUP BY ga.mdate, matchid
ORDER BY ga.mdate, matchid

#COALESCE formula replace the null values with 0 in this case
```
