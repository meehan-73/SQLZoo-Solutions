# SQLZoo-Solutions

Here are my solutions to many of the problems on the SQLZoo website!

## Tutorials: Learn SQL In Stages
[1.0: SELECT Basics](#10-select-basics)  
[1.1: SELECT Name](#11-select-name)  
[1.2: SELECT From World](#12-select-from-world)  
[1.3: SELECT From Nobel](#13-select-from-nobel)  
[1.4: SELECT Within SELECT](#14-select-within-select)  
[1.5: SUM and COUNT](#15-sum-count)  
[1.6: JOIN](#16-join)  
[1.7: More JOIN Operations](#17-more-join-operations)  
[1.8: Using Null](#18-using-null)  
[1.8+: Numeric Examples](#18-numeric-examples)  
[1.9-: Window Function](#19--window-function)  
[1.9+: COVID-19](#19-covid-19)  
[1.9: Self Join](#19-self-join)  
[1.10: Tutorial Quizzes](#110-tutorial-quizzes)  
[1.11: Tutorial Student Records](#111-tutorial-student-records)  
[1.12: Tutorial DDL](#112-tutorial-ddl)

## 1.0: SELECT Basics

## 1.1: SELECT Name

## 1.2: SELECT From World

1.
```sql
SELECT name, continent, population FROM world
```

2.
```sql
SELECT name FROM world
WHERE population >= 200000000
```

3.
```sql
SELECT name, gdp/population
FROM world
WHERE population >= 200000000
```

4.
```sql
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
```

5.
```sql
SELECT name, population FROM world
WHERE name IN ('France', 'Germany', 'Italy')
```

6.
```sql
SELECT name FROM world
WHERE name LIKE '%United%'
```

7.
```sql
SELECT name, population, area FROM world
WHERE area >= 3000000 OR population >= 250000000
```

8.
```sql
SELECT name, population, area FROM world
WHERE (area >= 3000000 AND population < 250000000) OR 
(area < 3000000 AND population >= 250000000)
```

9.
```sql
SELECT name, ROUND(population/1000000,2), ROUND(gdp/1000000000,2) FROM world 
WHERE continent = 'South America'
```

10.
```sql
SELECT name, ROUND(gdp/population,-3) FROM world
WHERE gdp >= 1000000000000
```

11.
```sql
SELECT name, capital FROM world
WHERE LENGTH(name) = LENGTH(capital)
```

12.
```sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital,1) AND name <> capital
```

13.
```sql
SELECT name FROM world
WHERE name LIKE '%a%' and name LIKE '%e%' and 
name LIKE '%i%' and name LIKE '%o%' and name LIKE '%u%'
and name NOT LIKE '% %'
```

## 1.3: SELECT From Nobel

1.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```

2.
```sql
SELECT winner
FROM nobel
WHERE yr = 1962
AND subject = 'Literature'
```

3.
```sql
SELECT yr, subject FROM nobel
where winner = 'Albert Einstein'
```

4.
```sql
SELECT winner FROM nobel
WHERE subject = 'Peace' AND yr >= 2000
```

5.
```sql
SELECT yr, subject, winner FROM nobel
WHERE subject = 'Literature' AND 1980 <= yr AND yr <= 1989
```

6.
```sql
SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt','Woodrow Wilson',
'Jimmy Carter', 'Barack Obama')
```

7.
```sql
SELECT winner FROM nobel
WHERE winner like 'John%'
```

8.
```sql
SELECT * FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR
(subject = 'Chemistry' AND yr = 1984)
```

9.
```sql
SELECT * FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine')
```

10.
```sql
SELECT * FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004)
```

11.
```sql
SELECT * FROM nobel
WHERE winner = 'Peter Grünberg'
```

12.
```sql
SELECT * FROM nobel
WHERE winner = 'Eugene O''neill'
```

13.
```sql
SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner ASC
```

14.
```sql
SELECT winner, subject 
FROM nobel
WHERE yr=1984
ORDER BY (subject IN ('Physics','Chemistry')) ASC,
subject ASC, winner ASC
#Key Takeaway: You can use an IN clause to return a column of 0’s and 1’s, and then
#ORDER BY this value to sort in certain ways.
```

## 1.4: SELECT Within SELECT

1.
```sql
SELECT name FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

2.
```sql
SELECT name FROM world
WHERE continent = 'Europe' AND gdp/population >
     (SELECT gdp/population FROM world
     WHERE name = 'United Kingdom')
```

3.
```sql
SELECT name, continent FROM world
WHERE continent IN 
     (SELECT continent FROM world
     WHERE name IN ('Argentina', 'Australia'))
ORDER BY name
```

4.
```sql
SELECT name, population FROM world
WHERE population >
     (SELECT population FROM world
     WHERE name = 'Canada')
AND
     population < 
     (SELECT population FROM world
     WHERE name = 'Poland')
```

5.
```sql
SELECT name, CONCAT(
ROUND(100*population/(SELECT population FROM world
WHERE name = 'Germany'),0), '%') as 'percentage'
FROM world
WHERE continent = 'Europe'
#This problem introduces CONCAT to get the % in the answer, as well as the renaming
#of a column via AS
```

6.
```sql
SELECT name FROM world
WHERE gdp > ALL(SELECT gdp FROM world
WHERE continent = 'Europe' AND gdp>0)
#Exercise deals with the ALL command. Need gdp>0 to deal with null values.
```

7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND population>0)
#This is an example of a correlated subquery. 
```

8.
```sql
SELECT continent, name FROM world x
WHERE name = (SELECT name FROM world y
     WHERE y.continent = x.continent
     ORDER BY name ASC
     LIMIT 1)
```

9.
```sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y
WHERE x.continent = y.continent)
```

10.
```sql
SELECT name, continent FROM world x
WHERE population > ALL(SELECT 3*population FROM world y
WHERE x.continent = y.continent AND x.name <> y.name)
#Think of x temporarily fixed, and y varying.
```

## 1.5: SUM and COUNT

1.
```sql
SELECT SUM(population)
FROM world
```

2.
```sql
SELECT DISTINCT(continent) FROM world
```

3.
```sql
SELECT SUM(gdp) FROM world
WHERE continent = 'Africa'
```

4.
```sql
SELECT COUNT(name) FROM world
WHERE area >= 1000000
```

5.
```sql
SELECT SUM(population) FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```

6.
```sql
SELECT continent, COUNT(name) FROM world
GROUP BY continent
#For each continent, COUNT(name) will be applied.
```

7.
```sql
SELECT continent, COUNT(name) FROM world
WHERE population >= 10000000
GROUP BY continent
#HAVING is used when aggregate clause is needed. WHERE
#is used otherwise.
```

8.
```sql
SELECT continent FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

## 1.6: JOIN

1.
```sql
SELECT matchid, player FROM goal 
WHERE teamid = 'GER'
```

2.
```sql
SELECT id,stadium,team1,team2
FROM game 
WHERE id = 1012
```

3.
```sql
SELECT player, teamid, stadium, mdate
FROM game 
JOIN goal 
ON (id=matchid)
WHERE teamid = 'GER'
```

4.
```sql
SELECT team1, team2, player
FROM game 
JOIN goal 
ON (id=matchid)
WHERE player LIKE 'Mario%'
```

5.
```sql
SELECT player, teamid, coach, gtime
FROM goal 
JOIN eteam
ON (teamid=id)
WHERE gtime <= 10
```

6.
```sql
SELECT mdate, teamname
FROM game
JOIN eteam
ON (team1 = eteam.id)
WHERE coach = 'Fernando Santos'
```

7.
```sql
SELECT player
FROM game 
JOIN goal 
ON (id=matchid)
WHERE stadium = 'National Stadium, Warsaw'
```

8.
```sql
SELECT DISTINCT(player)
FROM game 
JOIN goal 
ON matchid = id 
WHERE (team1='GER' OR team2 = 'GER') AND teamid <> 'GER'
```

9.
```sql
SELECT teamname, COUNT(player)
FROM eteam 
JOIN goal 
ON id=teamid
GROUP BY teamname
```

10.
```sql
SELECT stadium, COUNT(player)
FROM game
JOIN goal 
ON id=matchid
GROUP BY stadium
```

11.
```sql
SELECT matchid, mdate, COUNT(player)
FROM game 
JOIN goal 
ON matchid = id 
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate
```

12.
```sql
SELECT matchid, mdate, COUNT(player)
FROM game 
JOIN goal 
ON matchid = id 
WHERE (team1 = 'GER' OR team2 = 'GER') AND (teamid = 'GER')
GROUP BY matchid, mdate
```

13.
```sql
SELECT mdate, team1, SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) as score1,
team2, SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) as score2
FROM game
LEFT JOIN goal 
ON matchid = id

GROUP BY id, mdate, team1, team2
ORDER BY mdate,matchid,team1,team2
```

## 1.7: More JOIN Operations

1.
```sql
SELECT id, title
FROM movie
WHERE yr = 1962
```

2.
```sql
SELECT yr FROM movie
WHERE title = 'Citizen Kane'
```

3.
```sql
SELECT id, title, yr FROM movie
WHERE title LIKE 'Star Trek%'
ORDER BY yr
```

4.
```sql
SELECT id FROM actor
WHERE name = 'Glenn Close'
```

5.
```sql
SELECT id FROM movie
WHERE title = 'Casablanca'
```

6.
```sql
SELECT name FROM movie 
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON actor.id = casting.actorid
WHERE movieid = 11768
```

7.
```sql
SELECT name FROM movie 
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON actor.id = casting.actorid
WHERE movie.title = 'Alien'
```

8.
```sql
SELECT movie.title FROM movie 
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford'
```

9.
```sql
SELECT movie.title FROM movie 
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford' AND casting.ord > 1
```

10.
```sql
SELECT movie.title, actor.name FROM movie 
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON actor.id = casting.actorid
WHERE movie.yr = 1962 AND casting.ord = 1
```

11.
```sql
SELECT yr,COUNT(title) FROM
movie JOIN casting ON movie.id=movieid
      JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```

12.
```sql
SELECT title, name FROM movie 
JOIN casting
ON (movie.id = casting.movieid AND ord = 1)
JOIN actor
ON actor.id = casting.actorid
WHERE movie.id IN 
(SELECT casting.movieid FROM casting WHERE actorid 
IN 
(SELECT id FROM actor WHERE name = 'Julie Andrews')
)
#Toughest one so far. Restudy this one.
```

13.
```sql
SELECT name FROM movie 
JOIN casting
ON (movie.id = casting.movieid AND ord = 1)
JOIN actor
ON actor.id = casting.actorid
GROUP BY name
HAVING COUNT(name) >= 15
```

14.
```sql
SELECT title, COUNT(title) FROM movie 
JOIN casting
ON (movie.id = casting.movieid)
JOIN actor
ON actor.id = casting.actorid
WHERE yr = 1978
GROUP BY title
ORDER BY COUNT(title) DESC, title
```

15.
```sql
SELECT name FROM actor
WHERE id IN (

SELECT DISTINCT(actorid) FROM casting
WHERE actorid <> 1112 AND movieid IN (

SELECT movieid FROM movie 
JOIN casting
ON (movie.id = casting.movieid)
JOIN actor
ON actor.id = casting.actorid
WHERE name = 'Art Garfunkel')

)
```

## 1.8: Using Null

1.
```sql
SELECT name FROM teacher
WHERE dept IS NULL
```

2.
```sql
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept
ON (teacher.dept=dept.id)
```

3.
```sql
SELECT teacher.name, dept.name
FROM teacher LEFT JOIN dept
ON (teacher.dept=dept.id)
```

4.
```sql
SELECT teacher.name, dept.name
FROM teacher RIGHT JOIN dept
ON (teacher.dept=dept.id)
```

5.
```sql
SELECT name, COALESCE(mobile, '07986 444 2266') FROM teacher
#COALESCE gives the first non-null value.
```

6.
```sql
SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher LEFT JOIN dept
ON (teacher.dept=dept.id)
```

7.
```sql
SELECT COUNT(name), COUNT(mobile) FROM teacher
```

8.
```sql
SELECT dept.name, COUNT(teacher.name) 
FROM teacher RIGHT JOIN dept
ON (teacher.dept=dept.id)
GROUP BY dept.name
```

9.
```sql
SELECT name, CASE 
WHEN (dept = 1 OR dept = 2) THEN 'Sci'
ELSE 'Art'
END
FROM teacher
```

10.
```sql
SELECT name, CASE 
WHEN (dept = 1 OR dept = 2) THEN 'Sci'
WHEN dept = 3 THEN 'Art'
ELSE 'None'
END
FROM teacher
```

## 1.8+: Numeric Examples

1.
```sql
SELECT a_strongly_agree
FROM nss
WHERE question='Q01'
AND institution='Edinburgh Napier University'
AND subject='(8) Computer Science'
```

2.
```sql
SELECT institution, subject
FROM nss
WHERE question='Q15'
AND score >= 100
```

3.
```sql
SELECT institution,score
FROM nss
WHERE question='Q15'
AND score < 50
AND subject='(8) Computer Science'
```

4.
```sql
SELECT subject, SUM(response)
FROM nss
WHERE question ='Q22' AND (subject='(8) Computer Science' OR subject='(H) Creative Arts and Design')
GROUP BY subject
```

5.
```sql
SELECT subject, SUM(response*a_strongly_agree/100)
FROM nss
WHERE question='Q22'
AND (subject = '(8) Computer Science' OR subject = '(H) Creative Arts and Design')
GROUP BY subject
```

6.
```sql
SELECT subject, ROUND(SUM(response * A_STRONGLY_AGREE / 100) / SUM(response) * 100)
FROM nss
WHERE question='Q22'
AND (subject='(8) Computer Science' OR subject='(H) Creative Arts and Design')
GROUP BY subject
```

7.
```sql
SELECT institution, ROUND(SUM((score/100)*response)/SUM(response)*100)
FROM nss
WHERE question='Q22'
AND (institution LIKE '%Manchester%')
GROUP BY institution
```

8.
```sql
SELECT institution, SUM(sample), (SELECT sample
FROM nss x
WHERE question='Q01'
AND (institution LIKE '%Manchester%')
AND subject = '(8) Computer Science'
AND x.institution = y.institution)

FROM nss y
WHERE question='Q01'
AND (institution LIKE '%Manchester%')
GROUP BY institution
#Nifty way to fix subquery returning more than 1 row problem
```

## 1.9-: Window Function

## 1.9+: COVID-19

## 1.9: Self Join

## 1.10: Tutorial Quizzes

## 1.11: Tutorial Student Records

## 1.12: Tutorial DDL
