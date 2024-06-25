# Using Null
Link to the tutorial: [Using NULL](https://www.sqlzoo.net/wiki/Using_Null).

### Name of the tables and columns
Table 1: teacher
| id | dept | name | phone | mobile |
| --- | --- | --- | --- | --- |
| 101 | 1 | Shrivell | 2753 | 07986 555 1234 |
| 102 | 1 | Throd	| 2754 | 07122 555 1920 |
| ... |


Table 2: dept
| id  | name | 
| --- | --- |
| 1 | Computing |
| 2 | Design |
| ... |

### 1. List the teachers who have NULL for their department.
```sql
SELECT name from teacher 
WHERE dept IS NULL 
```

### 2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.
```sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)

```
### 3. Use a different JOIN so that all teachers are listed.
```sql
SELECT t.name, d.name
FROM teacher t LEFT JOIN dept d ON (t.dept=d.id)
```

### 4. Use a different JOIN so that all departments are listed.
```sql
SELECT t.name, d.name
FROM teacher t RIGHT JOIN dept d ON (t.dept=d.id)
```

### 5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'
```sql
SELECT name, COALESCE(mobile,'07986 444 2266') from teacher
```
### 6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
```sql
SELECT t.name, COALESCE (d.name,"None") 
FROM teacher t LEFT JOIN dept d ON (t.dept=d.id)
```

### 7. Use COUNT to show the number of teachers and the number of mobile phones.
```sql
SELECT COUNT(name),COUNT(mobile) FROM teacher
```

### 8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
```sql
SELECT d.name, COUNT(t.name) FROM teacher t RIGHT JOIN dept d ON (t.dept=d.id)
GROUP BY d.name
```

### 9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
```sql
SELECT name,
CASE 
WHEN dept = 1 THEN 'Sci' 
WHEN dept = 2 THEN 'Sci'
ELSE 'Art'  

END AS depttext
FROM teacher
```

### 10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.
```sql
SELECT name,
CASE 
WHEN dept = 1 THEN "Sci" 
WHEN dept = 2 THEN "Sci"
WHEN dept = 3 THEN "Art" 
ELSE "None" 
END AS depttext
FROM teacher
```
