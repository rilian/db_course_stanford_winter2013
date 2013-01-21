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

For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order.

```sql
select n2, g2, n1, g1 from
( select h1.id+h2.id as mult, h1.name as n1, h1.grade as g1, h2.name as n2, h2.grade as g2
  from likes as l
  join highschooler as h1 on h1.id = l.id1
  join highschooler as h2 on h2.id = l.id2
  where exists (select * from likes where id1=h2.id and id2=h1.id)
  order by  mult asc , h1.name < h2.name desc
) as t
group by mult
```

Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade.

```sql
select  h1.name, h1.grade
from highschooler as h1
join friend as f on f.id1=h1.id
join highschooler as h2 on h2.id = f.id2
where h1.grade=h2.grade
and not exists (select id from highschooler where id in (
  select id2 from friend where id1=h1.id) and grade <> h1.grade)
group by h1.name
order by h1.grade, h1.name
```

Find the name and grade of all students who are liked by more than one other student.

```sql
select name, grade from highschooler
where id in (
select id2
from likes
group by id2
having count(id1) > 1
)
```
