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
