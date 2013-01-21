# SQL Social-Network Query Exercises (core set)

Find the names of all students who are friends with someone named Gabriel.

```sql
select name from Highschooler
where id in (
  select ID1 from friend where id1 = id and
  id2 IN (select id from Highschooler where name="Gabriel")
)
```

For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.

```sql
select h1.name, h1.grade, h2.name, h2.grade
from highschooler as h1
join likes as l on l.id1 = h1.id
join highschooler as h2 on h2.id = l.id2
where h2.grade <= h1.grade - 2
```



